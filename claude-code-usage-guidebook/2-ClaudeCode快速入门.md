# Claude Code 快速入门

本文帮助您快速上手Claude Code，了解基本概念和常用命令。

## 前提条件

开始使用前，请确保已完成：
- ✅ 环境配置（参考 [1-环境相关.md](1-环境相关.md)）
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

## Skill快速上手

### 什么是Skill

Skill是Claude Code中预定义的可复用任务，类似于"快捷指令"。

**Skill vs 普通Prompt的区别：**

| 对比项 | Skill | 普通Prompt |
|--------|-------|-----------|
| 定义方式 | 预先定义好的脚本 | 临时描述需求 |
| 标准化 | 统一的执行流程 | 每次描述可能不同 |
| 适用场景 | 重复性标准任务 | 灵活临时任务 |
| 学习成本 | 查看文档即可使用 | 需要清晰描述需求 |
| 团队协作 | 保证团队统一规范 | 每人表达方式不同 |

**示例对比：**
```bash
# 使用Skill（推荐）
/code-review

# 使用普通Prompt（不推荐用于标准任务）
请帮我审查代码，检查以下几点：
1. 代码规范是否符合团队标准
2. 是否有潜在的bug
3. 性能是否有优化空间
4. 安全性问题
5. 测试覆盖率
...（还需要描述更多细节）
```

### 如何查看项目可用的Skill

**方法1：查看项目文档（推荐）**
```bash
# 项目文档通常在以下位置说明Skill清单：
- README.md
- CLAUDE.md
- docs/skills.md
```

**方法2：在Claude Code中询问**
```bash
你: 这个项目有哪些可用的skill？
Claude Code: [会列出项目定义的所有Skill]
```

**方法3：查看.claude/skills/目录**
```bash
# 在WSL终端中
ls .claude/skills/

# 查看某个Skill的详细信息
cat .claude/skills/code-review.md
```

### 执行第一个Skill

**基本语法：**
```bash
/skill-name
```

**示例：执行代码审查Skill**
```bash
# 1. 在Claude Code对话中输入
/code-review

# 2. Claude Code会自动执行预定义的代码审查流程
# 3. 等待执行完成，查看审查结果
```

**带参数的Skill：**
```bash
# 某些Skill可以接受参数
/deploy production          # 部署到生产环境
/run-tests src/utils        # 运行指定目录的测试
/generate-api User          # 生成User相关的API
```

**对话中说明调用：**
```bash
# 也可以在对话中自然语言调用
你: 请使用code-review skill检查我刚才修改的代码
Claude Code: [会执行/code-review skill]
```

### Skill的执行流程

1. **用户输入**：`/skill-name` 或自然语言说明
2. **Skill加载**：Claude Code读取Skill定义
3. **执行任务**：按照Skill脚本执行操作
4. **结果反馈**：显示执行结果和下一步建议

### 何时使用Skill、何时使用普通Prompt

**使用Skill的场景：**
- ✅ 项目定义的标准流程（代码审查、部署、测试等）
- ✅ 重复性工作（每次都要做的任务）
- ✅ 团队统一的操作规范
- ✅ 复杂的多步骤流程

**使用普通Prompt的场景：**
- ✅ 临时性需求（一次性任务）
- ✅ 探索性工作（不确定具体怎么做）
- ✅ 需要灵活对话的场景
- ✅ Skill无法覆盖的特殊需求

**实际例子：**
```bash
# 场景1：执行标准的代码审查 → 使用Skill
/code-review

# 场景2：临时需求：重构某个函数 → 使用Prompt
请帮我重构 calculateTotal 函数，使其更易读

# 场景3：运行项目定义的测试流程 → 使用Skill
/run-tests

# 场景4：探索性任务：研究新技术 → 使用Prompt
请帮我分析一下使用Redis作为缓存的优缺点
```

### Skill执行失败的常见原因

1. **Skill不存在**
   - 检查Skill名称是否正确（区分大小写）
   - 确认项目是否定义了该Skill

2. **参数错误**
   - 查看Skill文档了解需要哪些参数
   - 检查参数格式是否正确

3. **环境问题**
   - 确认当前目录是否在项目根目录
   - 检查是否有必要的依赖和权限

4. **Skill定义错误**
   - 联系项目管理员或Team Leader
   - 查看Skill源代码排查问题

### 重要说明

**本文档教授Skill的使用方法（方法论），具体项目有哪些Skill及其用途，请查看：**
- 项目README.md
- 项目CLAUDE.md
- 项目docs/目录下的Skill文档

详细的Skill开发指南请参考：[500-Agent与Skill开发.md](500-Agent与Skill开发.md)（高级用户）

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

## 跨系统使用实战

### 在WSL中启动Claude Code的完整步骤

**步骤1：连接到WSL**
```bash
# 在VSCode中：
# 1. 点击左下角绿色按钮
# 2. 选择 "Connect to WSL"
# 3. 等待连接成功
```

**步骤2：打开项目**
```bash
# 在VSCode中：
# File → Open Folder → 选择WSL中的项目目录
# 例如：/home/username/projects/my-project
```

**步骤3：启动Claude Code**
```bash
# 在VSCode的WSL终端中（Ctrl+`）
claude-code
```

**步骤4：开始对话**
```bash
# 输入你的第一个指令
你: 请介绍一下这个项目
```

### Windows IDE中查看修改的验证方法

**方法1：实时查看（IDE远程开发）**
- VSCode已连接到WSL，文件修改会实时显示
- 无需额外操作

**方法2：手动刷新（直接文件访问）**
```bash
# 如果文件没有自动刷新，在VSCode中：
# 1. 点击资源管理器中的文件
# 2. 或按 Ctrl+R 刷新窗口
```

**方法3：Git查看（Git同步）**
```bash
# 在Windows的Git Bash或VSCode终端中
git pull

# 查看修改
git log
git diff
```

### 文件同步问题的排查

**问题1：WSL中修改了文件，Windows IDE看不到**

**检查步骤：**
```bash
# 1. 确认路径是否一致
# WSL中：
pwd
# 输出：/home/username/projects/my-project

# Windows VSCode中：
# 应该打开的是 \\wsl$\Ubuntu\home\username\projects\my-project
```

**解决方案：**
```bash
# 方法1：使用VSCode Remote-WSL（推荐）
# 左下角绿色按钮 → Connect to WSL → 打开项目

# 方法2：在Windows中打开WSL路径
# File → Open Folder → 输入 \\wsl$\Ubuntu\home\username\projects\my-project
```

**问题2：权限错误导致无法修改**

**解决方案：**
```bash
# 在WSL中修改文件权限
chmod -R 755 ~/projects/my-project
```

**问题3：文件系统性能问题**

**解决方案：**
```bash
# 将项目从 /mnt/c/ 移到 WSL文件系统
mv /mnt/c/projects/my-project ~/projects/
cd ~/projects/my-project
```

### 验证配置是否正确

运行以下检查清单：

```bash
# 1. 检查WSL连接
# VSCode左下角应显示 "WSL: Ubuntu"（或其他发行版名称）

# 2. 检查项目路径
pwd
# 应该在 /home/username/... 而不是 /mnt/c/...

# 3. 检查Claude Code
claude-code --version

# 4. 测试文件创建
echo "test" > test.txt
# 在Windows IDE中应该能看到test.txt文件

# 5. 清理测试文件
rm test.txt
```

## 下一步学习

**入门路径（按顺序学习）：**
1. [3-Skill使用指南.md](3-Skill使用指南.md) - 深入学习Skill使用
3. [99-入门常见问题FAQ.md](99-入门常见问题FAQ.md) - 遇到问题时查阅

**进阶学习（掌握基础后）：**
- [110-核心概念.md](110-核心概念.md) - 深入了解Claude Code的工作机制
- [130-使用技巧.md](130-使用技巧.md) - 掌握实用技巧和最佳实践
- [140-成本与安全.md](140-成本与安全.md) - 成本控制和高级技巧
