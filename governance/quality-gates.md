# Quality Gates

> Tier: 1+ (基础闸门) | Tier 2+ (覆盖率 + lint) | Tier 3+ (安全 + 性能)

## Baseline Gates (Tier 1+)

以下检查在每次 commit 前和 pre-merge 时执行：

| 闸门 | 命令（按技术栈） | 失败处理 |
|------|-----------------|----------|
| **Type-check** | `vue-tsc --noEmit` / `tsc --noEmit` / `mypy` | 阻塞 merge，强制修复 |
| **Unit tests** | `vitest run` / `pytest` / `go test` | 阻塞 merge，强制修复 |
| **Build** | `vite build` / `npm run build` / `cargo build` | 阻塞 merge，强制修复 |

## Team Gates (Tier 2+)

| 闸门 | 阈值 | 说明 |
|------|------|------|
| **Code coverage** | ≥ 80% for logic-heavy modules | 低于阈值 → 补充测试 → 重新评估 |
| **Lint** | 0 errors, 0 warnings | `eslint` / `ruff` / `clippy` |
| **Dependency audit** | 0 Critical / High CVE | `npm audit --audit-level=high` |
| **Bundle size** | 不超过上次构建的 +15% | 超出 → 分析增量来源 → 报告 |

## Enterprise Gates (Tier 3+)

| 闸门 | 说明 |
|------|------|
| **Security scan** | OWASP ZAP / Snyk / Trivy 自动扫描 |
| **Secret detection** | `gitleaks` / `trufflehog` 扫描提交历史 |
| **E2E smoke test** | 核心用户路径（登录 → 主要操作 → 退出） |
| **Contract test** | API 输入输出格式与契约文档一致 |

## Gate Enforcement

- **Pre-commit**: lint + type-check（轻量，<30s）
- **Pre-merge**: 全量闸门（test + build + coverage + audit）
- **Gate failure → 不允许 merge**，无例外（除非甲方明确 override）
