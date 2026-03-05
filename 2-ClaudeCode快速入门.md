# Claude Code 快速入门

本文帮助新手快速上手Claude Code，了解基本概念和常用命令。

## 5分钟上手Claude Code

如果你是第一次使用Claude Code（以下简称CC），按照以下步骤快速体验：

**步骤1：启动CC**
```bash
# 在项目目录下运行
cd /path/to/your/project
claude-code
```

**步骤2：第一次对话**
```
你: 请介绍一下这个项目的结构
CC: [会自动搜索和阅读项目文件，然后给出项目结构说明]
```

**步骤3：尝试简单任务**
```
你: 请在src/utils目录下创建一个formatDate函数，将日期格式化为YYYY-MM-DD格式
CC: [会生成代码文件]
```

**步骤4：查看修改**
- VSCode会自动刷新显示CC创建的文件
- 可以review代码，如有问题可以继续对话修改

## 新手最容易犯的3个错误

### 错误1：指令过于模糊

❌ **错误示例：**
```
请修复一下这个bug
```

✅ **正确示例：**
```
在UserController.java的login方法中，当密码错误时应该返回401状态码，
但现在返回的是500。请修复这个问题。
```

### 错误2：一次性要求太多

❌ **错误示例：**
```
请生成完整的用户管理模块，包括增删改查、权限控制、日志记录、
数据验证、单元测试和集成测试
```

✅ **正确示例：**
```
步骤1：请先生成User实体类和UserRepository
步骤2：（等第一步完成后）现在请生成UserService
步骤3：（等第二步完成后）现在请生成UserController
```

### 错误3：忘记Review CC生成的代码

⚠️ **重要提醒：**
- CC生成的代码可能存在bug或不符合项目规范
- **必须**仔细review每一处修改
- 关键代码必须进行测试验证
- 不要盲目信任AI的输出

## 第一次使用建议

1. **从简单任务开始**：先尝试生成工具函数、单元测试等简单任务
2. **创建CLAUDE.md**：让CC了解你的项目（使用`/init`命令自动生成）
3. **设置权限确认**：不要禁用命令执行前的确认提示
4. **保持耐心**：AI不是万能的，需要多次对话才能达到预期效果

## CLAUDE.md 简介

### 什么是CLAUDE.md

`CLAUDE.md`是项目的交接文档，类似于新成员入职时阅读的项目介绍。CC会通过阅读这份文档来理解项目架构、功能和代码规范。

### 推荐包含的内容

- 项目概述和技术栈
- 目录结构说明
- 常用命令和脚本
- 代码规范和最佳实践
- 项目特有的术语和缩写

### 如何创建

```bash
# 使用CC自动生成
/init

# 或手动创建后让CC优化
你: 请review并优化CLAUDE.md
```

## Agent、Skill、Command基础

### Command（命令）

CC的内置命令，用于控制CC行为：
- `/help` - 显示帮助
- `/init` - 生成CLAUDE.md
- `/clear` - 清空上下文
- `/compact` - 压缩上下文

### Skill（技能）

项目自定义的可复用工具，通过`/skill名`调用：
```
/backend:create-api
/test:run
```

### Agent（代理）

CC内置的专门化子任务处理器，拥有独立上下文：
- `Explore` - 代码探索
- `Plan` - 实现计划
- 各种QA工程师agent

## 常用命令速查

| 命令 | 功能 | 使用场景 |
|------|------|---------|
| `/help` | 显示帮助信息 | 查看可用命令 |
| `/init` | 生成CLAUDE.md | 首次使用项目 |
| `/clear` | 清空上下文 | 开始新任务 |
| `/compact` | 压缩上下文 | 上下文接近满 |
| `/fast` | 快速模式 | 需要更快响应 |

**VSCode快捷键：**
- `Ctrl+Shift+P` → Claude Code - 打开CC面板
- `Ctrl+L` - 聚焦到输入框
- `Ctrl+K` - 清空对话历史

## 常用对话模式

### 文件操作
```bash
# 读取文件
请阅读 src/utils/helper.js 文件

# 修改文件
请在 UserController.java 的第45行添加参数验证

# 创建文件
请创建 src/config/database.ts 配置文件
```

### 代码操作
```bash
# 生成代码
请生成一个计算两个日期之间天数差的函数

# 重构代码
请将 calculateTotal 函数重构为更易读的版本

# 代码审查
请review src/api/user.ts 文件，检查是否有潜在问题
```

### 测试操作
```bash
# 生成测试
请为 formatDate 函数生成单元测试

# 运行测试
请运行测试并告诉我结果

# 修复测试
测试 user.test.js 失败了，请修复
```

## 下一步学习

- [3-ClaudeCode核心概念.md](3-ClaudeCode核心概念.md) - 深入了解CC的工作机制
- [4-ClaudeCode使用技巧.md](4-ClaudeCode使用技巧.md) - 掌握实用技巧和最佳实践
- [5-ClaudeCode进阶指南.md](5-ClaudeCode进阶指南.md) - 成本控制和高级技巧
