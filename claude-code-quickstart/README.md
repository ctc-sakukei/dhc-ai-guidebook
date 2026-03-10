# Claude Code 快速上手指南

> CTC 项目的 Claude Code 使用文档，从零开始掌握 AI 辅助编程。

---

## 📖 快速开始

### 新手入门（必读）

按顺序阅读以下文档，即可开始使用 Claude Code：

1. **[0-基本知识.md](0-基本知识.md)** - AI 和 Claude Code 基础概念
2. **[1-环境相关.md](1-环境相关.md)** - CTC 环境配置指南
   - [1.1-AWS-IAM准备.md](1.1-AWS-IAM准备.md)
   - [1.2-Bedrock环境配置.md](1.2-Bedrock环境配置.md)
3. **[2-ClaudeCode快速入门.md](2-ClaudeCode快速入门.md)** - 开始使用 Claude Code
4. **[3-Skill使用指南.md](3-Skill使用指南.md)** - 学习使用 Skill 提升效率

**遇到问题？** 查看 [99-入门常见问题FAQ.md](99-入门常见问题FAQ.md)

---

## 📚 进阶学习

完成入门学习后，可以深入学习以下内容：

### [advanced/](advanced/) - 进阶内容

> 适合已掌握基本使用，希望提升效率和降低成本的用户

- [AI通用知识.md](advanced/AI通用知识.md) - AI 工作原理
- [核心概念.md](advanced/核心概念.md) - Claude Code 机制
- [高级Prompt技巧.md](advanced/高级Prompt技巧.md) - 提示词技巧
- [使用技巧.md](advanced/使用技巧.md) - 实用技巧
- [成本与安全.md](advanced/成本与安全.md) - 成本控制

📌 [查看完整进阶内容索引](advanced/README.md)

---

### [expert/](expert/) - 高级开发

> 适合 Team Leader、技术负责人，需要开发自定义 Agent 和 Skill

- [500-Agent与Skill开发.md](expert/500-Agent与Skill开发.md) - 开发自定义工具
- [510-CLAUDE.md编写指南.md](expert/510-CLAUDE.md编写指南.md) - 项目配置
- [520-团队协作最佳实践.md](expert/520-团队协作最佳实践.md) - 团队规范
- [530-Memory系统高级应用.md](expert/530-Memory系统高级应用.md) - Memory 深度应用
- [540-性能调优与监控.md](expert/540-性能调优与监控.md) - 性能优化

📌 [查看完整高级内容索引](expert/README.md)

---

### [reference/](reference/) - 参考资料

> 对 AI 原理、工具对比感兴趣的读者

- [设计哲学.md](reference/设计哲学.md) - Claude Code 设计理念
- [其他AI工具对比.md](reference/其他AI工具对比.md) - AI 工具横向对比

📌 [查看参考资料索引](reference/README.md)

---

## 🗂️ 文档结构

```
claude-code-quickstart/
├── README.md                        ← 你在这里
├── 0-基本知识.md                    ← 入门必读
├── 1-环境相关.md                    ← 入门必读
├── 1.1-AWS-IAM准备.md              ← 环境配置
├── 1.2-Bedrock环境配置.md          ← 环境配置
├── 2-ClaudeCode快速入门.md         ← 入门必读
├── 3-Skill使用指南.md              ← 入门必读
├── 99-入门常见问题FAQ.md           ← 问题排查
│
├── advanced/                        ← 进阶内容
│   ├── README.md
│   ├── AI通用知识.md
│   ├── 核心概念.md
│   └── ...
│
├── expert/                          ← 高级开发
│   ├── README.md
│   ├── 500-Agent与Skill开发.md
│   └── ...
│
└── reference/                       ← 参考资料
    ├── README.md
    ├── 设计哲学.md
    └── 其他AI工具对比.md
```