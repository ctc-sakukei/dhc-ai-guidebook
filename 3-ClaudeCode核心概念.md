# Claude Code 核心概念

本文深入介绍Claude Code的核心机制，包括项目结构、Agent系统和上下文管理。

## .claude 目录详解

### 目录结构

`.claude`文件夹保存Claude Code的项目级配置：

```
.claude/
├── skills/         # 项目自定义技能（推荐）
├── commands/       # 项目自定义命令（⚠️ 已废弃）
├── memory/         # 持久化记忆
└── resources/      # 项目资源文件
```

> **重要提示：** 从Claude Code v2.1.3开始，Command功能已被Skill取代。现有commands虽然能用，但不再维护。

### Skill深入使用

**常见Skill类型：**

| Skill类型 | 用途示例 |
|----------|---------|
| 后端开发 | `backend:create-api` 创建REST API |
| 前端开发 | `frontend:create-component` 生成组件 |
| 测试 | `test:run` 执行测试套件 |
| 代码审查 | `review:security` 安全检查 |

**调用方式：**

```bash
# 方式1：直接命令
/backend:create-api

# 方式2：对话中说明
请使用backend:create-api创建用户管理API

# 方式3：带参数调用
/test:run --coverage
```

**自定义Skill开发：**

根据团队需求创建专属工具：
- 符合团队规范的代码生成器
- 自动化测试脚本
- 定制化代码审查工具

### Memory系统详解

**目录结构：**

```
.claude/projects/[项目名]/memory/
├── MEMORY.md          # 主记忆（自动加载）
├── debugging.md       # 调试经验
├── patterns.md        # 代码模式
└── api-conventions.md # API规约
```

**MEMORY.md特性：**

- ✅ 每次对话自动加载到上下文
- ⚠️ 仅加载前200行，超出部分被截断
- 📌 适合保存架构要点、常见问题、代码规范

**最佳实践：**

- **保持简洁**：MEMORY.md只记录关键要点
- **使用链接**：详细内容放在专门文件中
- **定期更新**：及时记录新的决策和经验
- **按主题组织**：而非按时间顺序

## Agent、Skill、Command区别

### 对比表格

| 特性 | Agent | Skill | Command |
|-----|-------|-------|---------|
| 定义 | 内置子任务处理器 | 项目级工具 | 系统控制命令 |
| 上下文 | 独立上下文空间 | 共享主上下文 | 无上下文 |
| 适用场景 | 复杂多步骤任务 | 标准化开发任务 | 控制CC行为 |
| 调用方式 | 对话中指定或自动 | `/skill名` | `/命令名` |
| 示例 | Explore, Plan | test:run | /clear, /help |

### Agent详解

**内置Agent类型：**

| Agent | 主要用途 | 特点 |
|-------|---------|------|
| `Explore` | 代码探索、文件搜索 | 快速定位代码 |
| `Plan` | 设计实现方案 | 架构规划 |
| `typescript-qa-engineer` | TS质量检查 | 专业测试 |
| `springboot-developer` | Spring Boot开发 | 后端开发 |

**何时使用Agent：**

✅ **推荐场景：**
- 需要大量文件搜索
- 上下文接近80%
- 并行处理独立任务
- 复杂代码生成/审查

❌ **不推荐场景：**
- 简单单文件读取
- 需要频繁交互确认

**使用示例：**

```
# 明确指定
请使用Explore agent搜索所有validation规则

# 自动选择
请帮我调查整个认证流程实现

# 并行处理
请同时用Explore调查前后端的验证逻辑
```

## 上下文管理深度解析

### 上下文工作原理

Claude Code本质上是**无状态**的，不会真正"记住"对话。所谓"记忆"是每次都附加历史对话。

**对话示例：**

```
第一轮：
你：你好，我叫Jason
API发送：你好，我叫Jason
CC：你好，Jason

第二轮：
你：我的名字是什么？
API发送：
  <历史>
    用户：你好，我叫Jason
    CC：你好，Jason
  </历史>
  <新指令>我的名字是什么？</新指令>
CC：<思考>从历史中看到用户叫Jason</思考>
    你的名字叫Jason
```

**关键影响：**

- 对话越长 → 上下文越大 → Token消耗越多 → 响应越慢
- 上下文达到100% → 自动执行`/compact`压缩
- 压缩会**永久丢失**部分信息

### 上下文管理策略

#### 策略1：主动清理

```bash
# 完成任务后立即清空
/clear

# 上下文接近满时压缩
/compact
```

#### 策略2：缓存策略

适用于复杂长任务，避免信息丢失：

**问题场景：**
测试10个用例，到第5个时上下文耗尽，最后生成报告时已"忘记"前5个结果。

**解决方案：**

```
步骤1：测试case1-3，结果保存到./temp/result1.md
步骤2：/clear
步骤3：测试case4-6，结果保存到./temp/result2.md
步骤4：/clear
步骤5：测试case7-10，结果保存到./temp/result3.md
步骤6：/clear
步骤7：读取所有result文件，生成汇总报告
```

**优势：**
- 避免上下文压缩导致的信息丢失
- 可处理任意规模的任务
- 中间结果持久保存

#### 策略3：Sub-Agent隔离

使用独立上下文的Agent处理大型子任务：

```
# 主对话上下文保持清洁
你：请用Explore agent全面搜索项目的认证实现

# Agent在独立上下文中工作
[Agent执行大量搜索，不占用主上下文]

# 返回精简结果到主对话
CC：[汇总Agent发现的关键信息]
```

## CLAUDE.md遵守程度

### 实际情况

Claude Code会**尽量遵守**CLAUDE.md规则，但**不是100%严格**。

**原因：**

当上下文接近容量上限需要压缩时，CC可能判断部分CLAUDE.md内容优先级较低，将其从上下文中移除。

### 应对策略

**策略1：关键规则重申**

对于特别重要的规则，在关键对话时再次强调：

```
你：请生成UserService，注意必须使用项目的BaseService
CC：[遵守规则生成代码]
```

**策略2：Prompt中明确说明**

直接在指令中写明核心要求：

```
你：请按照以下规范生成代码：
- 使用4空格缩进
- 所有public方法必须有JavaDoc
- 参考UserService.java的风格
```

**策略3：保持CLAUDE.md简洁**

- 只保留最核心的规则
- 将详细说明放在其他文档
- 提高被保留在上下文的优先级

## 下一步学习

- [4-ClaudeCode使用技巧.md](4-ClaudeCode使用技巧.md) - 掌握实用对话技巧
- [5-ClaudeCode进阶指南.md](5-ClaudeCode进阶指南.md) - 成本控制和高级技巧
