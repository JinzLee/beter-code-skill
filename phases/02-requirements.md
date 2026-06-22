# Phase 1: Requirements Analysis (需求分析)

> 进入条件: 初始化完成，甲方陈述需求

## Workflow

1. Project Lead + requirement analysts (≤3) brainstorm the Client's stated needs
2. For every unclear point → Project Lead formulates precise questions → asks Client
3. If the Client seems unfamiliar with a domain → **first warn about key considerations**, then let Client decide
4. Produce `agents_files/requirements/需求文档_v1.md` (format: `templates/requirement-doc.md`)
5. Present to Client for confirmation

## Client Domain Unfamiliarity Protocol

When the Project Lead detects the Client is unfamiliar with a technical domain:
```
⚠️ 甲方请注意，关于 [领域名称]：
   - <风险/注意事项 1>
   - <风险/注意事项 2>
   - <建议或替代方案>

请甲方在上述提醒的基础上做出决定：
   - 选项 A: <方案A，含影响说明>
   - 选项 B: <方案B，含影响说明>
```

## Iteration Rules

- Requirement doc → Client review → revise → repeat until Client approves
- Track iterations in `agents_files/requirements/` (v1, v2, v3...)
- After **every 4 iterations**: summarize unresolved items, ask Client "是否降低部分需求优先级以加速推进？"
- After **10 iterations maximum**: present final summary, Client makes final binding decision

## Git Commit

- 每版需求文档产出后: `docs: v<N> requirements document`
