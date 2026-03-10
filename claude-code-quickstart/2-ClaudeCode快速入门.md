# Claude Code 快速入门

> 本文档帮助你快速上手 Claude Code，了解基本配置和使用方法。

---

## 前提条件

✅ 确保已完成环境配置：[1-环境相关.md](1-环境相关.md)

---

## Claude Code 项目配置

### CLAUDE.md 文件

**作用：**
Claude Code 的项目说明文件，告诉 AI 这个项目的基本信息。

**位置：**
项目根目录 `CLAUDE.md`

**包含内容：**

- 项目简介和目标
- 技术栈和架构
- 开发规范和约定
- 目录结构说明
- 注意事项

**创建方式：**

```bash
# 在 Claude Code 对话中输入
/init
```

> 💡 Claude Code 会自动扫描项目并生成 CLAUDE.md 文件。

**示例内容：**

```markdown
# 项目名称

## 项目简介
用户管理系统，提供用户注册、登录、权限管理功能

## 技术栈
- 后端：Java + Spring Boot
- 前端：React + TypeScript
- 数据库：MySQL

## 目录结构
- src/main/java - 后端代码
- src/main/resources - 配置文件
- frontend/ - 前端代码

## 开发规范
- 使用驼峰命名
- 所有 API 返回统一格式
- 必须编写单元测试
```

---

### .claude 目录

Claude Code 使用 `.claude/` 目录存放项目相关的配置和扩展功能：

| 目录/文件 | 作用 |
|----------|------|
| **skills/** | 自定义快捷指令 |
| **agents/** | 专用 AI 助手 |
| **memory/** | 项目记忆信息（自动生成） |
| **settings.json** | 项目配置文件 |

---

### Skill（快捷指令）

**位置：** `.claude/skills/` 目录

**作用：** 存放项目自定义的快捷指令，实现重复任务的自动化。

Skill 是预定义的任务流程，可以通过简短的命令触发复杂的操作。

#### 示例：代码审查 Skill

文件：`.claude/skills/code-review.md`

```markdown
# Code Review

检查代码质量：
1. 代码规范
2. 潜在 bug
3. 性能问题
4. 安全隐患
```

#### 使用方式

Skill 支持两种执行方式：

| 方式 | 说明 | 示例 | 推荐度 |
|------|------|------|--------|
| **直接命令** | 使用 `/` 开头的命令 | `/code-review` | ⭐⭐⭐ |
| **自然语言** | Claude Code 自动识别意图 | "请帮我审查代码"<br>"帮我检查一下代码质量"<br>"review 一下代码" | ⭐⭐⭐⭐⭐ |

> 💡 **推荐使用自然语言方式**
> Claude Code 会分析你的自然语言，判断是否匹配某个 Skill，自动执行。

#### 查看项目中的 Skill

```bash
# 查看 skills 目录
ls .claude/skills/

# 查看 Skill 内容
cat .claude/skills/code-review.md
```

---

### Agent（专用助手）

**位置：** `.claude/agents/` 目录

**作用：** 定义专门用途的 AI 助手，处理特定类型的任务。

Agent 是针对特定场景优化的 AI 配置，具有专门的提示词和行为模式。

#### 常见 Agent 类型

| Agent 类型 | 用途 |
|-----------|------|
| **Explore Agent** | 代码库探索和搜索 |
| **Custom Agent** | 项目自定义的专用助手 |

#### 示例：探索 Agent

文件：`.claude/agents/explore.md`

```markdown
# Explore Agent

专门用于代码库搜索和分析：
- 快速定位文件和函数
- 分析代码依赖关系
- 生成项目结构报告
```

> 💡 Agent 通常在需要时自动调用，也可以在项目配置中指定使用特定 Agent。

---

## 开始使用 Claude Code

### 启动 Claude Code

#### 方式 1：使用 VSCode 插件 ⭐⭐⭐⭐⭐

**步骤：**

1. **打开项目**
   - 在 VSCode 中打开项目文件夹

2. **打开 Claude Code 面板**
   - 点击侧边栏的 Claude Code 图标
   - 或按 `Ctrl+Shift+P` → 输入 "Claude Code"

3. **开始对话**
   - 在聊天框中输入指令

#### 方式 2：使用命令行

```bash
# 进入项目目录
cd ~/projects/your-project

# 启动 Claude Code
claude-code
```

---

### 第一次对话

#### 示例 1：了解项目

```
你: 请介绍一下这个项目的结构
```

Claude Code 会自动搜索和阅读项目文件，然后给出项目结构说明。

#### 示例 2：创建文件

```
你: 请在 src/utils 目录下创建一个 formatDate 函数，
    将日期格式化为 YYYY-MM-DD 格式
```

Claude Code 会生成代码文件，VSCode 会自动刷新显示。

#### 示例 3：修改代码

```
你: 请在 UserController.java 的 login 方法中添加参数验证
```

---

## 常用对话模式

### 文件操作

```bash
# 读取文件
请阅读 src/utils/helper.js 文件

# 修改文件
请在 UserController.java 的第 45 行添加参数验证

# 创建文件
请创建 src/config/database.ts 配置文件
```

---

### 代码操作

```bash
# 生成代码
请生成一个计算两个日期之间天数差的函数

# 重构代码
请将 calculateTotal 函数重构为更易读的版本

# 代码审查
请 review src/api/user.ts 文件，检查是否有潜在问题
```

---

### 测试操作

```bash
# 生成测试
请为 formatDate 函数生成单元测试

# 运行测试
请运行测试并告诉我结果

# 修复测试
测试 user.test.js 失败了，请修复
```

---

**相关文档：**

- [3-Skill使用指南.md](3-Skill使用指南.md) - 深入学习 Skill 使用
- [99-入门常见问题FAQ.md](99-入门常见问题FAQ.md) - 常见问题解答
