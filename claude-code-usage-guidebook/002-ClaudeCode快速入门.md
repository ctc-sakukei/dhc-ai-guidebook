# Claude Code 快速入门

本文帮助您快速上手Claude Code，了解基本概念和常用命令。

## 前提条件

开始使用前，请确保已完成：
- ✅ 环境配置（参考 [001-环境相关.md](001-环境相关.md)）
- ✅ 跨系统配置（如使用WSL，参考项目环境配置文档）

## 5分钟上手Claude Code

如果你是第一次使用Claude Code，可以按照以下步骤快速体验：

**步骤1：在WSL中启动Claude Code**
```bash
# 1. 使用VSCode连接到WSL（左下角绿色按钮）
# 2. 在WSL中打开项目目录
cd ~/projects/your-project

# 3. 启动Claude Code
claude-code
```

**步骤2：第一次对话**
```
你: 请介绍一下这个项目的结构
Claude Code: [会自动搜索和阅读项目文件，然后给出项目结构说明]
```

**步骤3：尝试简单任务**
```
你: 请在src/utils目录下创建一个formatDate函数，将日期格式化为YYYY-MM-DD格式
Claude Code: [会生成代码文件]
```

**步骤4：在Windows IDE中查看修改**
- VSCode会自动刷新显示Claude Code创建的文件
- 可以review代码，如有问题可以继续对话修改

**步骤5：验证跨系统同步**
- 检查Windows IDE中是否能看到文件变化
- 如果看不到，参考 [99-入门常见问题FAQ.md](99-入门常见问题FAQ.md) 排查

## 最容易犯的3个错误

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

### 错误3：忘记Review Claude Code生成的代码

⚠️ **重要提醒：**
- Claude Code生成的代码可能存在bug或不符合项目规范
- **必须**仔细review每一处修改
- 关键代码必须进行测试验证
- 不要盲目信任AI的输出

## 第一次使用建议

1. **从简单任务开始**：先尝试生成工具函数、单元测试等简单任务
2. **创建CLAUDE.md**：让Claude Code了解你的项目（使用`/init`命令自动生成）
3. **设置权限确认**：不要禁用命令执行前的确认提示
4. **保持耐心**：AI不是万能的，需要多次对话才能达到预期效果

## Skill快速体验

> **Skill是什么？** 预定义的可复用任务脚本，类似于"快捷指令"。详见 [000-基本知识.md#skill技能](000-基本知识.md#skill技能)

### 快速示例

```bash
# 基本用法
/code-review    # 执行代码审查
/run-tests      # 运行测试
/deploy staging # 部署到staging环境
```

### 查看项目可用的Skill

```bash
# 方法1：查看项目文档（README.md 或 CLAUDE.md）
# 方法2：在对话中询问
你: 这个项目有哪些可用的skill？
```

> 📖 **深入学习**：Skill的完整使用指南、决策方法、故障排查等，请查看 [003-Skill使用指南.md](003-Skill使用指南.md)

## 常用对话模式

### 使用Skill（推荐）
```bash
# 代码审查
/code-review

# 运行测试
/run-tests

# 生成文档
/generate-docs

# 部署
/deploy staging
```

### 使用普通Prompt（临时任务）

#### 文件操作
```bash
# 读取文件
请阅读 src/utils/helper.js 文件

# 修改文件
请在 UserController.java 的第45行添加参数验证

# 创建文件
请创建 src/config/database.ts 配置文件
```

#### 代码操作
```bash
# 生成代码
请生成一个计算两个日期之间天数差的函数

# 重构代码
请将 calculateTotal 函数重构为更易读的版本

# 代码审查（临时审查，非标准流程）
请review src/api/user.ts 文件，检查是否有潜在问题
```

#### 测试操作
```bash
# 生成测试
请为 formatDate 函数生成单元测试

# 运行测试（临时测试，非标准流程）
请运行测试并告诉我结果

# 修复测试
测试 user.test.js 失败了，请修复
```

## 下一步学习

**入门路径（按顺序学习）：**
1. [003-Skill使用指南.md](003-Skill使用指南.md) - 深入学习Skill使用
3. [099-入门常见问题FAQ.md](099-入门常见问题FAQ.md) - 遇到问题时查阅

**进阶学习（掌握基础后）：**
- [110-核心概念.md](110-核心概念.md) - 深入了解Claude Code的工作机制
- [130-使用技巧.md](130-使用技巧.md) - 掌握实用技巧和最佳实践
- [140-成本与安全.md](140-成本与安全.md) - 成本控制和高级技巧
