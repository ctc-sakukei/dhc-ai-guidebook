# 500-Agent与Skill开发

## Agent

### 定义

Agent 是 Claude Code 中用于处理复杂、多步骤任务的自主子进程。每个 Agent 都具有特定的能力和可用工具集，能够独立完成任务并返回结果。Agent 以专门化的方式运行，例如 general-purpose（通用）、Explore（代码探索）、Plan（实现规划）等类型。

### 作用

Agent 的主要作用是将复杂任务分解并自主执行,避免占用主对话的上下文空间。它可以执行深度代码搜索、多轮探索、架构规划等需要大量操作的任务，并在完成后将精炼的结果返回给主对话，提高整体工作效率。

### 使用时机

当任务需要多次搜索、文件读取或探索才能完成时，应使用 Agent。典型场景包括：在代码库中搜索不确定位置的关键字或文件、理解复杂的代码架构、规划多文件的实现方案、需要并行执行多个独立的研究任务等情况。

### 编写规范

编写 Agent 调用时需要明确指定 `subagent_type` 参数来选择合适的 Agent 类型。必须提供清晰的任务描述（`prompt`），让 Agent 能够自主工作。使用 `description` 参数提供 3-5 词的简短摘要。对于独立任务，应使用单个消息并行启动多个 Agent 以提高性能。可选择使用 `run_in_background` 参数让 Agent 在后台运行。如需延续之前的 Agent 工作，可使用 `resume` 参数传入 Agent ID。避免在 Agent 已经执行的搜索任务上重复工作。

### 示例

**Explore Agent（代码探索 Agent）：**

```
类型: Explore
描述: 快速探索代码库的专用 Agent

可用工具:
  - Glob: 文件模式匹配
  - Grep: 代码内容搜索
  - Read: 文件读取
  - WebFetch: 获取网页内容
  - WebSearch: 网络搜索

工作方式:
  1. 接收探索任务（如"查找所有 API 端点定义"）
  2. 根据任务选择合适的搜索策略（文件模式、关键字等）
  3. 执行多轮搜索和文件读取操作
  4. 分析和整理发现的代码结构
  5. 返回精炼的探索结果（文件位置、代码模式、架构信息）

特点:
  - 不能修改文件（无 Edit/Write 权限）
  - 可设置彻底度级别：quick/medium/very thorough
  - 适合回答"如何实现某功能"、"在哪里定义某类"等问题
```

---

## Skill

### 定义

Skill 是 Claude Code 中可在主对话中执行的专门能力单元，类似于斜杠命令。用户通过 `/skill-name` 的方式调用，例如 `/commit`、`/simplify`。每个 Skill 都封装了特定领域的知识和操作流程，提供结构化的任务执行能力。

### 作用

Skill 为常见的开发工作流提供标准化、可复用的能力。它简化了复杂操作，让用户无需记忆详细步骤即可完成专业任务。例如 simplify Skill 可自动审查代码质量并修复问题，skill-creator Skill 可辅助创建和优化新的 Skill。

### 使用时机

当用户需要执行标准化的开发任务时应使用 Skill。典型场景包括：代码提交（/commit）、代码审查和优化（/simplify）、创建或修改 Skill（/skill-creator）等。如果用户明确提到斜杠命令或任务与已有 Skill 的功能匹配，应主动调用相应的 Skill。

### 编写规范

#### 手动编写

手动编写 Skill 需要创建包含完整提示词和元数据的 YAML 文件。文件应放置在 `.claude/skills/` 目录下，包含 `name`、`description`、`instructions` 等必要字段。`instructions` 字段需详细描述 Skill 的执行步骤和逻辑。可使用 `params` 定义参数，使用 `tools` 限制可用工具。编写时应确保指令清晰、完整、可执行，避免歧义。良好的 Skill 应具有单一职责，专注于解决特定问题。

#### 通过官方工具 `skill-creator` 创建

使用 `skill-creator` Skill 可以通过交互式对话创建新 Skill。调用 `/skill-creator` 后，按照提示描述 Skill 的功能、使用场景和期望行为，系统会自动生成结构化的 Skill 文件。`skill-creator` 还支持修改现有 Skill、运行评估测试、性能基准测试和优化触发描述。这种方式降低了编写门槛，确保生成的 Skill 符合规范，并可通过迭代优化提升质量。

### 示例

**代码审查 Skill（完整定义）：**

```yaml
name: code-review
description: 对指定文件或当前修改进行全面的代码审查，提供改进建议

instructions: |
  你是一个经验丰富的代码审查专家。执行以下审查流程：

  1. 确定审查范围
     - 如果用户提供了文件路径参数，审查该文件
     - 否则运行 git diff 审查所有已修改的代码

  2. 进行多维度分析
     - 代码质量：命名规范、函数长度、复杂度、注释完整性
     - 潜在问题：空指针、边界条件、异常处理、资源泄漏
     - 性能优化：算法复杂度、不必要的计算、数据结构选择
     - 安全性：SQL 注入、XSS、敏感信息泄露、权限检查
     - 可维护性：重复代码、耦合度、可测试性

  3. 提供结构化反馈
     - 按严重程度分类问题（严重/警告/建议）
     - 给出具体的代码位置和改进方案
     - 对优秀的代码实践给予肯定

  4. 不直接修改代码
     - 仅提供审查报告和建议
     - 让用户决定是否采纳建议

params:
  - name: file_path
    description: 可选，指定要审查的文件路径
    required: false

tools:
  - Read
  - Bash
  - Grep
  - Glob

examples:
  - user_input: "/code-review"
    description: 审查所有 git 已修改的文件
  - user_input: "/code-review src/auth.ts"
    description: 审查指定文件
```

---

## Skill完整开发流程

### 1. 需求分析
- 识别重复性任务
- 评估标准化可行性
- 确定目标用户和使用场景

### 2. 设计
- 定义Skill名称和描述
- 确定输入参数
- 设计执行步骤
- 选择所需工具

### 3. 实现
- 编写YAML文件或使用`/skill-creator`
- 测试基本功能
- 处理边界情况

### 4. 测试
- 单元测试（各种输入）
- 集成测试（实际项目）
- 性能测试（Token消耗）

### 5. 部署
- 提交到`.claude/skills/`
- 更新CLAUDE.md文档
- 通知团队成员

### 6. 文档
- 编写使用说明
- 提供示例
- 说明注意事项

---

## Skill开发参考案例

### 案例1：简单Skill - 代码格式化

```yaml
name: format
description: 格式化指定文件或所有修改的文件

instructions: |
  1. 确定格式化范围
     - 如有file_path参数，格式化该文件
     - 否则格式化所有git修改的文件

  2. 根据文件类型选择工具
     - JavaScript/TypeScript: prettier
     - Python: black
     - Java: google-java-format

  3. 执行格式化并报告结果

params:
  - name: file_path
    required: false

tools:
  - Bash
  - Read
```

**复杂度**：⭐ 简单
**Token消耗**：约500-1000

---

### 案例2：中等复杂度 - API生成器

```yaml
name: api-gen
description: 根据数据模型自动生成RESTful API代码

instructions: |
  1. 读取用户提供的模型文件

  2. 分析模型结构
     - 字段名称和类型
     - 关系（一对多、多对多）
     - 验证规则

  3. 生成标准CRUD endpoints
     - GET /api/{resource}
     - GET /api/{resource}/:id
     - POST /api/{resource}
     - PUT /api/{resource}/:id
     - DELETE /api/{resource}/:id

  4. 生成代码文件
     - Controller
     - Service
     - Repository
     - DTO

  5. 添加标准错误处理和验证

params:
  - name: model_file
    required: true
  - name: output_dir
    required: false
    default: src/api/

tools:
  - Read
  - Write
  - Edit
```

**复杂度**：⭐⭐ 中等
**Token消耗**：约2000-5000

---

### 案例3：复杂Skill - 端到端测试流程

```yaml
name: e2e-test
description: 执行完整的端到端测试流程，包括环境准备、测试执行和报告生成

instructions: |
  1. 环境准备
     - 检查测试环境是否就绪
     - 启动必要的服务（数据库、Redis等）
     - 清理测试数据

  2. 测试执行
     - 运行Cypress/Playwright测试套件
     - 监控测试进度
     - 捕获失败截图

  3. 结果分析
     - 统计通过/失败用例
     - 分析失败原因
     - 识别不稳定测试

  4. 报告生成
     - 生成HTML测试报告
     - 创建趋势分析图表
     - 发送通知（如配置）

  5. 清理
     - 停止测试服务
     - 归档测试数据
     - 恢复环境

params:
  - name: test_suite
    description: 测试套件名称（可选，默认运行全部）
    required: false
  - name: headless
    description: 是否无头模式
    required: false
    default: true

tools:
  - Bash
  - Read
  - Write
```

**复杂度**：⭐⭐⭐ 复杂
**Token消耗**：约5000-10000

---

## 如何设计一个好的Skill

### 单一职责原则
```markdown
❌ 不好：name: dev-workflow
       （构建、测试、部署全包含）

✅ 好的：name: build
       name: test
       name: deploy
       （各司其职，可组合使用）
```

### 参数设计
- **必需参数**：任务无法完成时才设为required
- **可选参数**：提供合理默认值
- **参数命名**：清晰、符合惯例

### 错误处理
```yaml
instructions: |
  1. 验证输入参数
     - 检查文件是否存在
     - 验证参数格式

  2. 执行主要逻辑
     - 使用try-catch思维
     - 提供清晰错误信息

  3. 异常情况说明
     - 告诉用户出了什么问题
     - 提供解决建议
```

### 用户反馈
- 开始时：说明正在做什么
- 过程中：显示关键步骤
- 结束时：总结结果
- 失败时：清晰的错误信息

---

## Skill测试和调试方法

### 测试方法

1. **手动测试**：
   ```
   /skill-name 正常参数
   /skill-name 边界情况
   /skill-name 错误输入
   ```

2. **回归测试**：
   - 修改后重新测试所有场景
   - 确保没有破坏现有功能

3. **性能测试**：
   - 监控Token消耗
   - 测试大文件/大项目场景

### 调试技巧

1. **添加日志输出**：
   ```yaml
   instructions: |
     1. 开始任务
        输出："正在处理 {file_path}"

     2. 关键步骤
        输出："已找到 X 个问题"
   ```

2. **分步执行**：
   - 将复杂Skill拆分测试
   - 逐步验证每个环节

3. **查看生成的Prompt**：
   - 理解Skill如何被展开
   - 优化指令清晰度

---

## Skill性能优化

### 优化策略

1. **减少文件读取**：
   ```yaml
   ❌ 读取所有文件再过滤
   ✅ 使用Glob精确匹配
   ```

2. **精简instructions**：
   ```yaml
   ❌ 详细解释每个概念（500行）
   ✅ 清晰简洁的步骤（50行）
   ```

3. **复用上下文**：
   ```yaml
   如果用户刚执行了git diff，
   不需要再次读取相同内容
   ```

4. **并行处理**（如适用）：
   ```yaml
   如果需要处理多个独立文件，
   可以使用Agent并行处理
   ```

---

## 常见错误和解决方案

### 错误1：Skill未被识别

**原因**：
- YAML语法错误
- 文件未保存到`.claude/skills/`
- 文件扩展名错误

**解决**：
- 验证YAML语法
- 确认文件位置
- 使用`.yaml`或`.yml`扩展名

### 错误2：参数传递失败

**原因**：
- params定义与instructions不匹配
- 参数名称拼写错误

**解决**：
- 确保instructions中使用`{param_name}`引用
- 检查大小写和拼写

### 错误3：执行超时

**原因**：
- 任务过于复杂
- 读取文件过多
- 进入死循环

**解决**：
- 拆分为多个小Skill
- 限制文件范围
- 添加退出条件

---

## 如何编写Skill文档

### 文档模板

```markdown
## /skill-name - 一句话描述

### 用途
详细说明这个Skill的功能和价值

### 使用场景
- 场景1：何时使用
- 场景2：解决什么问题
- 场景3：适合什么情况

### 语法
/skill-name [必需参数] [可选参数]

### 参数说明
- `param1`（必需）：参数说明
- `param2`（可选，默认：value）：参数说明

### 使用示例
**示例1：基本用法**
/skill-name file.ts

**示例2：带参数**
/skill-name file.ts --format prettier

### 预期结果
执行后会产生什么输出或效果

### 注意事项
- 特殊情况说明
- 限制和约束
- 前置条件

### 故障排除
常见问题和解决方法
```

### 文档最佳实践

1. **用户视角**：从使用者角度写，不是开发者
2. **具体示例**：真实场景，不是抽象说明
3. **清晰简洁**：重点突出，避免冗长
4. **及时更新**：Skill变更时同步更新文档

---

## Agent调用时机决策树

```
需要执行任务？
  │
  ├─ 是简单查询或单步操作？
  │   └─ 是 → 直接操作
  │
  ├─ 需要多次搜索/探索？
  │   ├─ 是 → 使用Explore Agent
  │   └─ 否 → 继续判断
  │
  ├─ 需要规划复杂实现？
  │   ├─ 是 → 使用Plan Agent
  │   └─ 否 → 继续判断
  │
  ├─ 有多个独立子任务？
  │   ├─ 是 → 使用多个Agent并行
  │   └─ 否 → 继续判断
  │
  └─ 需要隔离环境测试？
      ├─ 是 → 使用Agent (isolation=worktree)
      └─ 否 → 直接操作
```

---

**相关文档**：
- [510-CLAUDE.md编写指南.md](510-CLAUDE.md编写指南.md)
- [高级开发常见问题FAQ.md](高级开发常见问题FAQ.md)
