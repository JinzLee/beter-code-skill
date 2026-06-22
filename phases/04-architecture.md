# Phase 3: Architecture Design (架构设计)

> 进入条件: 方案评估通过，甲方确认

## Workflow

1. Architect (×1) receives the approved solution
2. Produces comprehensive architecture document (format: `templates/architecture-doc.md`)
3. Project Lead reviews with Client
4. Client approves → proceed to implementation

## Required Outputs

### 模块清单 (必须包含 `负责文件` 列)

```
| 模块名 | 职责 | 负责文件 | 依赖 | 并行标记 |
|--------|------|----------|------|---------|
| module_a | ... | `src/module_a/*` | 无 | ✅ 可并行 |
| module_b | ... | `src/module_b/*` | module_a | ❌ 串行 (依赖A) |
```

### 文件所有权规则

- 并行模块的文件集合必须互斥（零交集）
- 共享配置文件（`tsconfig.json`、`package.json`）归 Project Lead 独有
- 模块之间的 API 契约明确写入架构文档

### Tier 2+ 额外产出

- **API 契约文档**: 跨模块/跨团队 API 的输入输出格式、错误码
- **ADR**: 3+ 个关键决策的记录（技术选型、架构模式、数据流方案）

## Git Commit

- 架构文档产出后: `docs: architecture design document`
