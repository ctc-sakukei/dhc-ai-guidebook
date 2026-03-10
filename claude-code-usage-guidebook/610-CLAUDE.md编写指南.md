# CLAUDE.md编写指南

## CLAUDE.md的作用

CLAUDE.md是项目的"说明书"，告诉Claude Code：
- 项目的技术栈和架构
- 代码规范和最佳实践
- 可用的Skill及其用途
- 特殊注意事项

---

## 结构设计原则

### 基本结构

```markdown
# 项目名称

## 项目概述
简要说明项目用途、技术栈

## 技术栈
- 语言：Java 17
- 框架：Spring Boot 3.x
- 数据库：PostgreSQL

## 代码规范
### 命名规范
### 代码风格
### 最佳实践

## 可用Skill
### /test - 运行测试
### /build - 构建项目

## 特殊说明
项目特定的注意事项
```

---

## 不同项目类型最佳实践

### Web全栈应用

```markdown
## 前端架构
- React 18 + TypeScript
- 状态管理：Zustand
- 路由：React Router v6

## 后端架构
- Express.js
- RESTful API设计
- JWT认证

## 重要规则
- API路径统一使用 /api/v1/ 前缀
- 所有异步操作必须有错误处理
- 组件必须有PropTypes或TypeScript类型
```

### 微服务API

```markdown
## 服务架构
- 服务发现：Eureka
- API网关：Spring Cloud Gateway
- 配置中心：Apollo

## 开发规范
- 每个服务独立数据库
- 使用OpenFeign进行服务间调用
- 统一异常处理和日志格式
```

---

## 规则详细度平衡

### ❌ 过于简单
```markdown
## 代码规范
使用驼峰命名
```

### ❌ 过于详细
```markdown
## 代码规范
1. 类名使用大驼峰，首字母大写
2. 方法名使用小驼峰，首字母小写
3. 常量使用全大写加下划线
... (100条规则)
```

### ✅ 恰当平衡
```markdown
## 代码规范
- 命名：遵循Java标准（类用PascalCase，方法用camelCase）
- 注释：公共API必须有Javadoc
- 异常：业务异常继承BusinessException
```

**原则**：只写重要的、容易出错的规则。

---

## 多人协作维护策略

### 1. 版本管理
```markdown
<!-- 文档版本：v2.1 -->
<!-- 最后更新：2026-03-08 by TeamLead -->
```

### 2. 分工维护
- 架构师：技术栈、架构设计部分
- 各模块负责人：特定模块规范
- Team Leader：整体协调和审核

### 3. 更新流程
```
1. 提交PR修改CLAUDE.md
2. 团队Review
3. 合并后通知全员
```

---

## 常见问题

### Q: CLAUDE.md应该多长？
A: 建议200-500行。过短信息不足，过长影响上下文。

### Q: 是否需要列出所有Skill？
A: 是的，必须列出。让团队成员知道有哪些工具可用。

### Q: 技术栈变更怎么办？
A: 及时更新CLAUDE.md，并通知团队。

---

**相关文档**：[600-Agent与Skill开发.md](600-Agent与Skill开发.md) | [620-团队协作最佳实践.md](620-团队协作最佳实践.md)
