# Git Management

> Tier: 1+ (所有层级强制)

## Commit Message Template

```
<type>(<scope>): <简短描述>

修改文件:
- path/to/file1.py (<新增/修改/删除>)
- path/to/file2.py (<新增/修改/删除>)

修改原因: <为什么做这个改动，一句话>

修改结果: <改动后达到什么效果，一句话>
```

**Type**: `feat` | `fix` | `refactor` | `style` | `docs` | `test` | `chore` | `revert`

## Branch Strategy

- **Initial commit**: `main` 直接提交
- **Feature branches**: `feature/better-code-<描述>`
  - 分支名需甲方确认后创建
- **No direct commits to main** after initialization

## Branch Lifecycle

```
main ──────────────────────────────────────
  │                    │
  ├── feature/xxx ──→ merge ✓
  │                    │
  │                    └── feature/yyy (from latest main)
```

### Merge Policy

**触发条件**（全部满足后 Project Lead 自主执行）:
1. 分支上所有模块代码已通过 code-review
2. 测试全部通过
3. Web 项目：UI review 问题已修复
4. Type-check / build 在 feature 分支上无错误

**Pre-Merge Gate**（Project Lead 执行）:
```
git checkout main
git merge --no-commit feature/better-code-xxx
# 在合并结果上运行: type-check + test + build
# 通过 → git commit merge
# 失败 → git merge --abort, 修复冲突后重试
# 无法自动解决冲突 → 报告冲突文件清单给甲方
```

**合并后**: 删除本地 feature 分支 `git branch -d feature/better-code-xxx`

### Rollback Procedure

**合并后回滚**:
```
git revert -m 1 <merge-commit-hash>
# 通知所有子 agent "main 已回滚，请重新拉取"
# 修复后重新走合并流程
```

**Feature 分支废弃**:
```
git checkout main
git branch -D feature/better-code-xxx
# process.md 标记 [-] 取消
```

- Revert commit 使用 `revert:` 前缀
- 回滚后必须通知受影响的子 agent

### Sub-Agent Sync

- 每个子 agent 启动前: `git checkout main && git pull` → 从最新 main 切新分支
- 禁止在旧分支上继续修改

## Push Policy

- **NEVER push without Client confirmation**
- Push 前展示: branch name, commit list, files changed
- Client 明确说 "可以推送" 才执行

## Mandatory Commit Triggers

| 触发事件 | Type | 示例 |
|----------|------|------|
| 项目初始化 | `chore` | `chore: initial commit` |
| 模块代码完成 | `feat`/`refactor` | `feat(auth): add login service` |
| 测试通过 | `test` | `test(auth): 9 tests passing` |
| 审核问题修复 | `fix` | `fix(auth): correct response validation` |
| UI 修复 | `fix`/`style` | `fix(ui): align sidebar icons` |
| 文档产出 | `docs` | `docs: v2 requirements confirmed` |
| 依赖变更 | `chore` | `chore: add pinia dependency` |
| 合并到 main | merge | `git merge --no-ff feature/xxx` |
| 回滚 | `revert` | `revert: rollback broken auth merge` |

- **Project Lead 主动执行**，不等甲方提醒
- 禁止批量提交：一个模块一个 commit
- **单次 commit 文件上限**: 不超过 15 个文件。超量时分批提交，每批独立 commit message
- 提交前 `git status` 确认只暂存了相关文件
