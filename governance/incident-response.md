# Incident Response

> Tier: 3 (Enterprise)

## Severity Classification

| 级别 | 定义 | 响应时间 | 处理方式 |
|------|------|----------|----------|
| **P0 - Critical** | 生产系统完全不可用，数据丢失 | 立即 | 回滚到上一个稳定版本 |
| **P1 - High** | 核心功能不可用，有 workaround | 1 小时内 | 评估回滚 vs 热修复 |
| **P2 - Medium** | 非核心功能异常 | 24 小时内 | 排入下一个 patch 修复 |
| **P3 - Low** | 外观/文案问题 | 下个版本 | 正常排期修复 |

## Decision Tree

```
事故发生
  │
  ├── 是否数据丢失/损坏？
  │     ├── 是 → P0 → 立即回滚 → 恢复数据 → 事后复盘
  │     └── 否 → 继续
  │
  ├── 是否核心功能完全不可用？
  │     ├── 是 → P1 → 15min 内评估：
  │     │     ├── 修复 < 30min → 热修复
  │     │     └── 修复 > 30min → 回滚
  │     └── 否 → 继续
  │
  └── P2/P3 → 排期修复，不打断当前迭代
```

## Rollback Command

```
git checkout main
git revert -m 1 <bad-merge-hash>
git tag -a v<X.Y.Z>-hotfix -m "hotfix: revert <description>"
# 通知所有相关人员
```

## Post-Incident (事后复盘)

- 事故发生后 48h 内产出复盘报告:
  - 根因分析（5 Why）
  - 影响范围
  - 修复措施
  - 预防措施（是否需要更新 skill 规则？）
- 报告存入 `agents_files/agent_memory/incident-<date>.md`
