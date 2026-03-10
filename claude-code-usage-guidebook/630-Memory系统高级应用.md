# Memory系统高级应用

## Memory架构设计

### 小型项目（< 10k行代码）
```
.claude/memory/
├── MEMORY.md           # 核心记忆（< 200行）
└── patterns.md         # 设计模式
```

### 中型项目（10k-100k行）
```
.claude/memory/
├── MEMORY.md           # 索引和核心内容
├── architecture.md     # 架构设计
├── modules/            # 按模块组织
│   ├── auth.md
│   ├── payment.md
│   └── notification.md
└── conventions.md      # 规范约定
```

### 大型项目（> 100k行）
```
.claude/memory/
├── MEMORY.md           # 只保留索引
├── core/               # 核心架构
├── modules/            # 业务模块
├── infrastructure/     # 基础设施
└── historical/         # 历史决策（归档）
```

---

## Memory与文档系统关系

### 分工原则

**MEMORY.md**：
- 频繁变化的信息
- Claude需要记住的上下文
- 临时决策和实验结果

**文档（README/Wiki）**：
- 稳定的架构设计
- 完整的API文档
- 用户使用手册

**CLAUDE.md**：
- 项目规范和规则
- Skill清单
- 开发注意事项

### 示例对比
```markdown
# MEMORY.md
- 最近修改：将认证从JWT改为OAuth2
- 临时方案：支付模块暂时硬编码测试环境
- TODO：优化用户查询性能

# CLAUDE.md
## 认证规范
使用OAuth2进行认证

## 代码规范
遵循Airbnb JavaScript风格

# README.md
# 项目架构
完整的架构图和说明...
```

---

## 定期维护策略

### 每周维护（15分钟）
- [ ] 删除已完成的TODO
- [ ] 更新过时的技术决策
- [ ] 合并重复内容

### 每月维护（30分钟）
- [ ] Review所有Memory文件
- [ ] 将稳定内容迁移到文档
- [ ] 归档历史决策

### 季度维护（1小时）
- [ ] 重构Memory结构
- [ ] 评估Memory有效性
- [ ] 团队培训和分享

---

## 性能优化

### 减少Memory大小
```markdown
❌ 过于详细：
## 用户模块
- User类位于 src/models/User.java
- 字段：id, name, email, password, createdAt...
- 方法：getUser(), setUser(), validate()...

✅ 恰当简洁：
## 用户模块
- 模型：User (src/models/)
- 认证：BCrypt加密
- 规则：email必须唯一
```

### 使用链接而非重复
```markdown
## 支付模块
详细架构见：[架构文档](docs/payment-architecture.md)

关键记忆点：
- 使用Stripe API
- 异步处理回调
- 失败重试3次
```

---

## 实战案例

### 案例1：重构记忆
```markdown
**情况**：大规模重构后，Memory过时

**方案**：
1. 创建 refactoring-2026-03.md
2. 记录主要变更
3. 更新相关模块Memory
4. 一周后归档到 historical/
```

### 案例2：多人冲突
```markdown
**情况**：两个开发者修改了同一Memory文件

**方案**：
1. Git合并时人工Review
2. 讨论保留哪些内容
3. 建立Memory维护责任人制度
```

---

**相关文档**：[110-核心概念.md](110-核心概念.md) | [620-团队协作最佳实践.md](620-团队协作最佳实践.md)
