# Skill使用指南

本文档详细介绍如何使用Claude Code中的Skill功能。

## Skill vs 普通Prompt的本质区别

### 本质对比

| 维度 | Skill | 普通Prompt |
|------|-------|-----------|
| **本质** | 预定义的可执行脚本 | 自然语言描述 |
| **执行方式** | 加载脚本后自动执行 | AI理解后生成执行计划 |
| **一致性** | 每次执行完全一致 | 每次可能有细微差异 |
| **复杂度** | 可封装复杂多步流程 | 需要详细描述每个步骤 |
| **维护性** | 统一修改脚本即可 | 需要每个人调整说法 |
| **学习成本** | 查看文档即可使用 | 需要学习如何描述需求 |

### 代码类比

```javascript
// Skill 类似于函数调用
codeReview();  // 执行预定义的代码审查流程

// 普通Prompt 类似于内联代码
// 每次都要重新描述：
// "请检查代码规范、安全性、性能问题..."
```

## 为什么项目要使用Skill

### 1. 标准化团队操作

**问题场景：**
- 团队成员A：请帮我审查代码，检查规范和bug
- 团队成员B：请review代码，看看有没有问题
- 团队成员C：请检查代码质量、性能、安全性

结果：每个人审查的标准不一致，质量参差不齐。

**使用Skill后：**
```bash
# 所有人使用统一的审查标准
/code-review
```

### 2. 可复用降低成本

**问题场景：**
每次部署都要重复输入：
```
请执行以下步骤：
1. 运行测试
2. 构建项目
3. 检查环境变量
4. 部署到服务器
5. 验证部署结果
6. 发送通知
```

**使用Skill后：**
```bash
/deploy production
```

### 3. 降低学习成本

**常见痛点：**
- 不知道项目有哪些标准流程
- 不知道如何正确描述需求
- 不知道需要检查哪些细节

**使用Skill后：**
- 查看Skill列表就知道可以做什么
- 直接执行Skill，无需了解实现细节
- Skill会自动处理所有必要步骤

## 如何查看项目可用的Skill列表

### 方法1：查看项目文档（推荐）

**位置：**
```bash
# 项目根目录下的文档
README.md
CLAUDE.md
docs/skills.md
docs/SKILLS.md
```

**示例格式：**
```markdown
## 项目Skill说明

### /code-review - 代码审查
**用途**：执行标准的代码审查流程
**使用场景**：提交PR前必须执行
**示例**：`/code-review`

### /run-tests - 运行测试
**用途**：运行完整的测试套件
**使用场景**：代码修改后验证功能
**示例**：`/run-tests` 或 `/run-tests src/utils`
```

### 方法2：在Claude Code中询问

```bash
# 方式1：直接询问
你: 这个项目有哪些可用的skill？

# 方式2：查看帮助
你: 请列出所有skill及其说明

# 方式3：搜索特定功能
你: 有没有用于测试的skill？
```

### 方法3：查看.claude/skills/目录

```bash
# 列出所有Skill文件
ls -la .claude/skills/

# 查看Skill详情
cat .claude/skills/code-review.md

# 搜索特定Skill
find .claude/skills/ -name "*test*"
```

## Skill的调用语法

### 基本调用

```bash
# 语法
/skill-name

# 示例
/code-review
/run-tests
/deploy
```

### 带参数调用

```bash
# 单个参数
/deploy production
/run-tests src/utils

# 多个参数
/generate-api User CRUD
/create-component Button primary large

# 参数说明
/skill-name [参数1] [参数2] [参数3]
```

### 对话中说明调用

```bash
# 自然语言触发Skill
你: 请使用code-review skill检查我的代码

你: 帮我执行deploy到production环境

你: 用run-tests skill测试utils目录
```

### 查看Skill帮助

```bash
# 查看特定Skill的使用说明
你: /code-review --help

你: code-review这个skill怎么使用？
```

## Skill的执行流程和反馈

### 执行流程

```
用户输入: /skill-name
    ↓
Claude Code加载Skill定义
    ↓
解析Skill脚本内容
    ↓
执行定义的操作步骤
    ↓
返回执行结果
    ↓
提供下一步建议
```

### 反馈信息类型

**1. 执行进度反馈**
```
🔄 正在执行 code-review skill...
  ✓ 检查代码规范
  ✓ 分析潜在bug
  ✓ 检查安全问题
  🔄 性能分析中...
```

**2. 执行结果反馈**
```
✅ Code Review 完成

发现的问题：
1. [规范] UserController.java:45 - 缺少参数验证
2. [性能] DataService.java:102 - 存在N+1查询问题

建议：
- 修复上述问题后重新审查
- 添加单元测试覆盖边界情况
```

**3. 错误反馈**
```
❌ Skill执行失败

原因：缺少必需参数
用法：/deploy [环境名称]
示例：/deploy production
```

## 如何判断何时用Skill、何时用Prompt

### 决策树

```
开始
  ↓
是否是重复性任务？
  ├─ 是 → 项目是否定义了相关Skill？
  │        ├─ 是 → 使用Skill ✅
  │        └─ 否 → 考虑建议团队创建Skill
  └─ 否 → 是否是一次性临时任务？
           ├─ 是 → 使用Prompt ✅
           └─ 否 → 是否需要灵活对话？
                    ├─ 是 → 使用Prompt ✅
                    └─ 否 → 使用Skill ✅
```

### 使用Skill的场景

**标准化任务：**
- ✅ 代码审查（必须检查固定的质量标准）
- ✅ 测试执行（固定的测试流程）
- ✅ 部署操作（标准的部署步骤）
- ✅ 文档生成（统一的文档格式）

**重复性工作：**
- ✅ 每次提交PR前的代码检查
- ✅ 定期的代码质量分析
- ✅ 标准的API生成

**团队规范操作：**
- ✅ 统一的代码格式化
- ✅ 标准的项目初始化
- ✅ 规范的组件创建

### 使用Prompt的场景

**临时性需求：**
- ✅ 重构某个特定函数
- ✅ 修复一个临时bug
- ✅ 快速验证一个想法

**探索性任务：**
- ✅ 研究新技术方案
- ✅ 分析复杂问题
- ✅ 讨论设计方案

**需要灵活对话：**
- ✅ 需要多轮对话澄清需求
- ✅ 需要根据反馈调整方案
- ✅ 复杂的问题诊断

### 实际案例对比

**案例1：代码审查**
```bash
# ❌ 不推荐：使用Prompt（费时且不标准）
你: 请审查UserController.java，检查代码规范、是否有bug、
    性能问题、安全问题、测试覆盖率、注释是否完整...

# ✅ 推荐：使用Skill（快速且标准）
/code-review
```

**案例2：重构特定函数**
```bash
# ✅ 推荐：使用Prompt（灵活临时任务）
你: 请重构calculateTotal函数，将其拆分为更小的函数，
    提高可读性

# ❌ 不推荐：创建Skill（过度工程化）
# 为一次性重构创建Skill没有必要
```

**案例3：项目初始化**
```bash
# ✅ 推荐：使用Skill（标准流程）
/init-project

# ❌ 不推荐：使用Prompt（容易遗漏步骤）
你: 请帮我初始化项目，创建目录结构、配置文件...
```

## Skill执行失败的常见原因和解决方法

### 错误1：Skill未找到

**现象：**
```
❌ Error: Skill 'code-reviw' not found
```

**原因：**
- Skill名称拼写错误（注意大小写）
- 项目未定义该Skill
- .claude/skills/目录配置错误

**解决方案：**
```bash
# 1. 检查Skill名称
你: 这个项目有哪些skill？

# 2. 查看Skill列表
ls .claude/skills/

# 3. 确认正确的Skill名称
/code-review  # 正确
/code-reviw   # 错误（拼写）
```

### 错误2：参数错误

**现象：**
```
❌ Error: Missing required parameter [environment]
Usage: /deploy [environment]
```

**原因：**
- 缺少必需参数
- 参数格式错误
- 参数值不在允许范围内

**解决方案：**
```bash
# 1. 查看Skill帮助
你: /deploy --help

# 2. 按照正确格式调用
/deploy production  # 正确
/deploy            # 错误（缺少参数）
```

### 错误3：权限问题

**现象：**
```
❌ Error: Permission denied
```

**原因：**
- 缺少文件执行权限
- 缺少目录访问权限
- WSL权限配置问题

**解决方案：**
```bash
# 检查和修复权限
chmod -R 755 .claude/skills/
chmod +x .claude/skills/deploy.sh
```

### 错误4：环境依赖缺失

**现象：**
```
❌ Error: Command 'npm' not found
```

**原因：**
- Skill依赖的工具未安装
- 环境变量未配置
- Node.js、Python等运行时缺失

**解决方案：**
```bash
# 安装缺失的依赖
npm install  # 安装Node.js依赖
pip install -r requirements.txt  # 安装Python依赖

# 检查环境
node --version
python --version
```

### 错误5：Skill定义错误

**现象：**
```
❌ Error: Invalid skill definition
```

**原因：**
- Skill脚本语法错误
- Skill配置文件格式错误

**解决方案：**
```bash
# 1. 查看Skill源文件
cat .claude/skills/problematic-skill.md

# 2. 联系项目管理员或Team Leader
# 3. 如果有权限，修复Skill定义后重试
```

## 重要说明

### 文档分工

**本文档（通用指南）：**
- ✅ 教授Skill的使用方法
- ✅ 说明何时使用Skill
- ✅ 介绍Skill的基本概念

**项目文档（项目特定）：**
- ✅ 列出本项目有哪些Skill
- ✅ 说明每个Skill的具体用途
- ✅ 提供项目特定的使用示例

### 学习路径

**入门阶段（当前）：**
1. 理解Skill基本概念 ✓
2. 学会查看和使用Skill ✓
3. 判断何时使用Skill ✓

**进阶阶段：**
- 参考项目文档学习项目特定的Skill
- 掌握常用Skill的高级用法
- 了解Skill的最佳实践

**高级阶段：**
- 学习如何开发自定义Skill
- 参考 [500-Agent与Skill开发.md](500-Agent与Skill开发.md)

## 下一步

- [99-入门常见问题FAQ.md](99-入门常见问题FAQ.md) - 查阅常见问题
- 查看项目文档 - 了解项目特定的Skill清单
