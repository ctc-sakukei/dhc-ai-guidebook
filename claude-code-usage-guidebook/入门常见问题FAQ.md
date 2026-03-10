# 99-入门常见问题FAQ

> **难度级别**：⭐ 入门
> **目标用户**：初次接触Claude Code的用户

本文档收集整理了使用Claude Code时最常遇到的问题及解决方案。所有问题按类别组织，并提供详细的解决步骤。

---

## 环境配置问题

### WSL连接失败怎么办？

**问题描述**：无法在VSCode中连接到WSL环境，或者Claude Code CLI无法在WSL中正常运行。

**原因分析**：
- WSL未正确安装或未启用
- VSCode Remote-WSL插件未安装
- WSL版本过旧

**解决方案**：
1. 检查WSL是否已安装：
   ```bash
   wsl --version
   ```
2. 如未安装，在PowerShell中运行：
   ```powershell
   wsl --install
   ```
3. 在VSCode中安装Remote-WSL插件
4. 按F1，输入"WSL: Connect to WSL"连接

**相关文档**：[1-环境相关.md](1-环境相关.md)

---

### Claude Code安装失败？

**问题描述**：执行安装命令后提示错误或无法找到claude命令。

**原因分析**：
- npm/npx未安装或版本过旧
- 网络问题导致下载失败
- 权限不足

**解决方案**：
1. 检查Node.js版本（需要v18+）：
   ```bash
   node --version
   ```
2. 如版本过旧，更新Node.js
3. 使用npx安装（推荐）：
   ```bash
   npx @anthropic/claude-code@latest
   ```
4. 如需全局安装：
   ```bash
   npm install -g @anthropic/claude-code
   ```

**相关文档**：[1-环境相关.md](1-环境相关.md)

---

### 路径找不到的问题

**问题描述**：Claude Code无法找到项目文件，或提示路径不存在。

**原因分析**：
- Windows路径和WSL路径混淆
- 相对路径使用错误
- 文件权限问题

**解决方案**：
1. **WSL中访问Windows文件**：
   - Windows路径：`C:\Users\username\project`
   - WSL路径：`/mnt/c/Users/username/project`

2. **Windows中访问WSL文件**：
   - WSL路径：`/home/username/project`
   - Windows路径：`\\wsl$\Ubuntu\home\username\project`

3. **推荐做法**：
   - 使用绝对路径
   - 在VSCode中使用Remote-WSL，自动处理路径转换

**相关文档**：[1-环境相关.md](1-环境相关.md)

---

### 权限错误如何解决？

**问题描述**：执行操作时提示"Permission denied"或权限不足。

**原因分析**：
- 文件所有者不匹配
- WSL和Windows文件系统权限冲突
- 需要sudo权限

**解决方案**：
1. **检查文件所有者**：
   ```bash
   ls -la /path/to/file
   ```

2. **修改文件所有者**：
   ```bash
   sudo chown -R $USER:$USER /path/to/directory
   ```

3. **WSL文件权限配置**：
   编辑`/etc/wsl.conf`添加：
   ```ini
   [automount]
   options = "metadata,umask=22,fmask=11"
   ```

**相关文档**：[1-环境相关.md](1-环境相关.md)

---

## Skill使用问题

### 如何查看项目有哪些Skill？

**问题描述**：不知道当前项目提供了哪些Skill可以使用。

**解决方案（3种方法）**：

**方法1：查看项目文档**（推荐）
- 查看项目的README.md或CLAUDE.md
- 通常会有"Skill清单"部分

**方法2：询问Claude Code**
- 在对话中直接询问：
  ```
  这个项目有哪些可用的skill？
  ```

**方法3：查看.claude目录**
- 检查项目中的`.claude/skills/`目录
- 每个文件对应一个Skill

**重要说明**：
- 本通用指南教授**如何使用Skill**
- **具体项目有哪些Skill**由项目文档说明
- 不同项目的Skill可能完全不同

**相关文档**：[3-Skill使用指南.md](3-Skill使用指南.md)

---

### Skill执行失败提示"未找到"

**问题描述**：执行`/skill-name`时提示Skill不存在或未找到。

**原因分析**：
- Skill名称拼写错误
- 项目未定义该Skill
- .claude目录配置问题

**解决方案**：
1. **确认Skill名称**：
   - 检查拼写是否正确
   - 注意大小写（通常使用小写和连字符）

2. **确认Skill存在**：
   - 查看项目文档或`.claude/skills/`目录
   - 询问Claude："可用的skill有哪些？"

3. **检查.claude配置**：
   - 确保`.claude`目录存在于项目根目录
   - 检查Skill文件格式是否正确

**相关文档**：[3-Skill使用指南.md](3-Skill使用指南.md)

---

### 何时用Skill、何时用Prompt？

**问题描述**：不确定什么时候应该使用Skill，什么时候使用普通对话。

**判断原则**：

**使用Skill的场景**：
- 项目定义的标准流程（如代码审查、测试、部署）
- 重复性工作（如格式化、文档生成）
- 团队统一的操作规范
- 需要多步骤的复杂任务

**使用Prompt的场景**：
- 临时性需求（如"帮我查找某个函数"）
- 探索性任务（如"分析这段代码的问题"）
- 需要灵活对话的场景
- 一次性的特殊操作

**简单记忆法**：
- **标准化、重复性** → 用Skill
- **灵活、临时性** → 用Prompt

**相关文档**：[Skill使用指南.md](Skill使用指南.md)

---

## 基础使用问题

### 第一次使用应该做什么？

**推荐步骤**：

1. **配置环境**（30分钟）
   - 安装WSL（如需跨系统）
   - 安装Claude Code CLI
   - 配置VSCode Remote-WSL
   - 参考：[010-环境相关.md](010-环境相关.md)

2. **了解基础概念**（15分钟）
   - 阅读[基本知识.md](基本知识.md)
   - 了解Prompt、Token、Context Window

3. **快速入门实践**（30分钟）
   - 跟随[ClaudeCode快速入门.md](ClaudeCode快速入门.md)
   - 尝试第一个对话
   - 执行第一个Skill（如项目提供）

4. **查看项目文档**（15分钟）
   - 阅读项目的README或CLAUDE.md
   - 了解项目特定的Skill和规范

**总计时间**：约90分钟即可上手

---

### Claude Code响应慢怎么办？

**问题描述**：等待时间过长，或者长时间无响应。

**原因分析**：
- 任务复杂度高
- 上下文过大
- 网络问题
- 模型负载高

**解决方案**：

1. **拆分任务**：
   - 将大任务分解为小任务
   - 逐步完成而不是一次性请求

2. **减少上下文**：
   - 只读取必要的文件
   - 避免一次性读取大量文件

3. **检查网络**：
   - 确认网络连接稳定
   - 考虑使用代理（如需要）

4. **切换模型**：
   - 简单任务可使用更快的模型
   - 参考：[基本知识.md](基本知识.md)

**相关文档**：[ClaudeCode快速入门.md](ClaudeCode快速入门.md)

---

### 如何取消正在执行的任务？

**解决方案**：

**在CLI中**：
- 按`Ctrl+C`中断当前任务
- 如无响应，再次按`Ctrl+C`

**在VSCode中**：
- 点击停止按钮（方形图标）
- 或按`Ctrl+C`（在终端中）

**注意事项**：
- 已执行的文件修改不会自动回滚
- 建议使用Git管理代码，随时可以恢复
- 取消后可以重新开始任务

---

## 文件同步问题

### Windows IDE看不到WSL中的修改

**问题描述**：Claude Code在WSL中修改了文件，但Windows IDE中没有显示更新。

**原因分析**：
- IDE未配置自动刷新
- 使用了错误的文件路径
- 文件系统缓存问题

**解决方案**：

1. **推荐方案：使用Remote-WSL**（最佳）
   - VSCode安装Remote-WSL插件
   - 直接在WSL环境中打开项目
   - 自动同步，无需手动刷新

2. **手动刷新方案**：
   - VSCode中右键点击文件/文件夹
   - 选择"Refresh"或按F5

3. **配置自动刷新**：
   - VSCode设置中启用`files.autoSave`

**相关文档**：[010-环境相关.md](010-环境相关.md)

---

## 快速索引

### 按紧急程度
- 🔴 **紧急**：Q1, Q2, Q3, Q11
- 🟡 **重要**：Q5, Q7, Q8
- 🟢 **一般**：Q4, Q6, Q9, Q10, Q12

### 按类别
- **环境配置**：Q1-Q4
- **Skill使用**：Q5-Q7
- **基础使用**：Q8-Q10
- **文件同步**：Q11-Q12

---

## 仍然有问题？

如果本FAQ未能解决你的问题：

1. **查看更详细的文档**：
   - [010-环境相关.md](010-环境相关.md) - 环境配置详解
   - [ClaudeCode快速入门.md](ClaudeCode快速入门.md) - 入门教程
   - [Skill使用指南.md](Skill使用指南.md) - Skill使用详解

2. **查看项目文档**：
   - 项目特定的问题请参考项目README或CLAUDE.md

3. **寻求帮助**：
   - 向团队成员咨询
   - 在项目issues中提问

---

**文档更新时间**：2026-03-08
**文档级别**：入门（0-99）
**预计阅读时间**：10-15分钟
