# Agent & Skill

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
