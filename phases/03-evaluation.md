# Phase 2: Solution Evaluation (方案评估)

> 进入条件: 需求文档经甲方确认

## Workflow

1. Project Lead sends the approved requirement document to evaluation analysts (≤3)
2. Each evaluator independently assesses: feasibility, time estimate, risk
3. Project Lead aggregates evaluations, resolves conflicts among evaluators
4. Present unified evaluation to Client (format: `templates/evaluation-report.md`)

## Evaluation Dimensions

- **技术可行性**: 是否存在技术阻塞点？
- **资源可行性**: 依赖/环境/人员是否可获取？
- **时间可行性**: 预估工时是否在可接受范围？
- **风险评估**: 技术/进度/人员/集成风险清单

## Rating

| 标记 | 含义 | 后续动作 |
|------|------|----------|
| ✅ Recommended | 方案可行 | 进入 Phase 3 |
| ⚠️ Conditional | 有条件接受 | 列出条件，修改后重新评估 |
| ❌ Rejected | 不可行 | 返回 Phase 1 调整需求 |

## Iteration Rules

- If ⚠️ or ❌ → return to Phase 1 with specific issues
- Every **4 iterations**: summarize for Client
- **10 iterations maximum**: Client makes final decision

## Tier 2+ 额外评估

- **工时偏差反馈**: 项目经理对比预估 vs 实际，反馈给评估师
- **依赖风险**: 评估外部依赖（第三方 API、后端接口）的可用性

## Git Commit

- 每版评估报告产出后: `docs: v<N> evaluation report`
