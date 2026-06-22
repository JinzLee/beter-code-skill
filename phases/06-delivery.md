# Phase 5: Delivery & Retrospective (交付验收与回顾)

> 进入条件: 所有模块已合并到 main，构建通过

## 5.1 Acceptance Review (验收审核)

Project Lead 向甲方展示：

```
📊 最终交付审核

版本: v<X.Y.Z>
分支: main

完成内容:
  ✅ <按需求文档逐项对照>
  ✅ ...

质量指标:
  🧪 测试: <N>/<N> 通过
  📦 构建: <status>
  🔒 安全审计: <status> (Tier 3)

CHANGELOG:
  <changelog 摘要>

请甲方验收，确认后完成交付。
```

## 5.2 Version Tagging (版本标签)

- Project Lead 按 `SKILL.md` 8.6 版本规则判定版本号
- `git tag -a v<X.Y.Z> -m "<描述>"`
- 更新 `CHANGELOG.md`

## 5.3 Retrospective (回顾复盘)

Project Lead 总结本次项目经验：

```markdown
# 项目回顾 — <项目名> v<X.Y.Z>

## 做了什么的
- 功能交付
- 质量指标

## 做的不好的
- 哪些阶段出现了预期外的问题
- 哪些 skill 规则没有被遵守或效果不佳

## 改进建议
- 对 better-code skill 本身的修改建议
- 对流程的优化建议

## 工时对比 (Tier 2+)
| 阶段 | 预估 | 实际 | 偏差 |
|------|------|------|------|
```

回顾总结存入 `agents_files/agent_memory/retrospective-<version>.md`

## Git Commit

- 交付完成后: `chore: v<X.Y.Z> release`
