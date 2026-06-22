# Phase 4: Implementation (实现阶段)

> 进入条件: 架构设计经甲方确认

## 4.1 Programming (代码编写)

1. Project Lead translates architecture into `process.md` subtasks
2. Before launching parallel agents: cross-check `负责文件` lists — zero overlap required
3. Programmers implement assigned modules
4. Project Lead manages git commits per module (see `governance/git-management.md`)

### Git Commit Rules (每次模块交付必须执行)

- 每个模块/文件组完成后: `git add <相关文件>` → commit → update `process.md`
- Commit types: `feat(<scope>)` for new, `refactor(<scope>)` for restructured
- **No cross-module bulk commits** — one module group = one commit

## 4.2 Code Review (代码审核)

1. Project Lead sends code to reviewer(s) (≤3)
2. Issues classified as `[架构级]` → architect redesign, or `[编码级]` → programmer fix
3. Max 3 review iterations per module; if still failing → escalate to Project Lead
4. Review report format: `templates/code-review-report.md`

## 4.3 Testing (测试)

1. Test engineer(s) write and execute tests after code passes review
2. **Testable code**: automated unit/integration tests
3. **Untestable code**: `MANUAL_TEST_GUIDE.md` with step-by-step instructions
4. Test failures → programmer fix with reproduction steps

### Tier 2+: Coverage Gate
- Logic-heavy modules must achieve ≥ 80% code coverage
- Coverage report appended to test report
- Below threshold → return to programmer

### Tests Passed Commit
- `test(<scope>): add tests for <模块名>` — include pass count

## 4.4 Frontend Review (Web 项目专属)

1. After code review + tests pass → frontend-designer reviews UI/UX
2. Issues → programmer fix → re-review
3. Commit fixes: `fix(ui): <描述>` or `style(<scope>): finalize UI`

## 4.5 Pre-Merge Gate (合并前质量闸门)

All modules pass review + test + UI review → Project Lead executes:

1. `git checkout main && git merge --no-commit feature/better-code-xxx`
2. On merge result: run `type-check` + `test` + `build`
3. ✅ Pass → `git commit` merge | ❌ Fail → `git merge --abort`, fix, retry
4. Merge → delete feature branch → tag version → update CHANGELOG

## Tier 3: 跨团队依赖管理

- 接口契约文档先行，在模块开发前定稿
- 被依赖方提供 mock-server
- 集成窗口: 所有团队在约定时间窗口内完成集成，避免持续冲突
