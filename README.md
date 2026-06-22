# Better-Code · Tower-Style Multi-Agent Collaborative Coding

[English](#english) | [中文](#chinese)

---

<a id="english"></a>
## English

**Better-Code** is an opencode skill implementing a tower-style multi-agent coding workflow. A single **Project Lead** agent interfaces with you (the Client), orchestrating up to **11 specialized sub-agent roles** through a structured 6-phase pipeline — from requirements to production-ready code. Scales from solo projects to enterprise multi-team systems.

### Architecture

```
Client (甲方)
  │
  └── Project Lead (Tower Top) ── sole interface
        │
        ├── Requirements Analyst (×≤3)
        ├── Solution Evaluator (×≤3)
        ├── Code Architect (×1)
        ├── Code Programmer (×≤3)
        ├── Code Reviewer (×≤3)
        ├── Frontend Designer (×1)
        ├── Test Engineer (×≤3)
        │
        ├── [Tier 2] DevOps Engineer (×1)
        ├── [Tier 3] Security Auditor (×1)
        ├── [Tier 3] Technical Writer (×1)
        └── [Tier 3] Project Manager (×1)
```

### Tiered Scaling

Choose your team size. Start small, scale up as needed.

| Tier | Use Case | Roles | Gate Upgrades |
|------|----------|-------|---------------|
| **1 · Solo** | ≤5 modules, single dev | 7 core roles | type-check + test + build |
| **2 · Team** | 6-15 modules, 1-2 parallel teams | + DevOps | + coverage ≥80%, dependency audit, ADR, dev/staging envs |
| **3 · Enterprise** | 16+ modules, 3+ parallel teams | + Security Auditor, Tech Writer, Project Manager | + OWASP scan, secret detection, contract/E2E tests, incident response, multi-team coordination |

The Project Lead scans your project and recommends a tier during initialization. **You always have the final say** — add or remove any role or rule.

### Key Features

- **One Interface** — Project Lead is the sole bridge to sub-agents. Never direct sub-agent ↔ Client communication.
- **6-Phase Pipeline** — Init → Requirements → Evaluation → Architecture → Implementation (Code → Review → Test → UI Review) → Delivery & Retrospective
- **11 Agent Roles** — 7 core + 4 extended (DevOps, Security, Docs, PM) activated by tier
- **Parallel Execution** — Modules marked `[可并行]` run concurrently (up to 3 agents). File ownership enforced — zero overlap.
- **Quality Gates** — Pre-commit (lint + type-check), pre-merge (test + build + coverage + audit). Merge blocked on failure.
- **Git Governance** — Standardized commits, feature branches, pre-merge gate (`merge --no-commit` → verify → commit), rollback procedures, semantic versioning + changelog
- **Iteration Safeguards** — Max iterations per phase; Client decides at limits
- **Smart Testing** — Unit/integration tests for testable code; manual test guides for DB/network/API-dependent code
- **Client-Controlled** — Every milestone, every `git push`, every tier decision requires your approval
- **Retrospective** — Post-delivery review: what worked, what broke, how to improve the skill itself

### File Structure

```
skills/better-code/
├── SKILL.md                     # Orchestrator: overview, scaling matrix, file index
├── config-example.jsonc         # opencode.jsonc command registration example
├── roles/                       # Agent role definitions (one per file)
│   ├── requirement-analyst.md   # Tier 1
│   ├── evaluation-analyst.md    # Tier 1
│   ├── code-architect.md        # Tier 1
│   ├── code-programmer.md       # Tier 1
│   ├── code-reviewer.md         # Tier 1
│   ├── frontend-designer.md     # Tier 1 (web only)
│   ├── test-engineer.md         # Tier 1
│   ├── devops-engineer.md       # Tier 2
│   ├── security-auditor.md      # Tier 3
│   ├── tech-writer.md           # Tier 3
│   └── project-manager.md       # Tier 3
├── phases/                      # Phase workflows
│   ├── 01-initialization.md     # Project scan + tier recommendation + setup
│   ├── 02-requirements.md       # Brainstorming, clarification, doc format
│   ├── 03-evaluation.md         # Feasibility, time, risk analysis
│   ├── 04-architecture.md       # Module breakdown, dependency graph, file ownership
│   ├── 05-implementation.md     # Code → Review → Test → UI Review → Merge
│   └── 06-delivery.md           # Acceptance, version tag, changelog, retrospective
├── governance/                  # Cross-cutting rules
│   ├── git-management.md        # Branch strategy, commit template, merge gate, rollback
│   ├── quality-gates.md         # Type-check / test / build / coverage / lint / audit
│   ├── environment-strategy.md  # dev/staging/prod separation, secret management (Tier 2+)
│   └── incident-response.md     # Severity classification, rollback decision tree (Tier 3)
└── templates/                   # Document format templates
    ├── requirement-doc.md
    ├── evaluation-report.md
    ├── architecture-doc.md
    ├── code-review-report.md
    ├── adr.md                    # Architecture Decision Record (Tier 2+)
    └── changelog.md
```

### Installation

1. Copy the entire `better-code/` directory to your skills location:

   ```
   # Global (all projects)
   ~/.config/opencode/skills/better-code/

   # Project-specific
   .opencode/skills/better-code/
   ```

2. Register `/better-code` in your `opencode.json` or `opencode.jsonc` (see `config-example.jsonc`)

3. Restart opencode.

### Quick Start

1. Navigate to your project in opencode
2. Type `/better-code`
3. If first time in this project: type `请你先初始化`
   - The Project Lead scans your project and recommends a tier
   - You confirm (or customize) the team configuration
   - Workspace is created automatically
4. Describe your requirements — the pipeline begins.

### Workflow Phases

| Phase | Agents | Output |
|-------|--------|--------|
| **0. Init** | Project Lead | `agents_files/`, `.opencode/agents/`, env, git, tier config |
| **1. Requirements** | Lead + Analysts (≤3) | `requirements/需求文档_vN.md` |
| **2. Evaluation** | Evaluators (≤3) | `evaluations/方案评估报告_vN.md` |
| **3. Architecture** | Architect (×1) | `architecture/architecture_design.md` |
| **4. Implementation** | Programmers → Reviewers → Testers → Frontend | Code + review reports + test reports |
| **5. Delivery** | Project Lead | Merge → tag → changelog → retrospective |

### Project Workspace (created on init)

```
project_root/
├── agents_files/              # Agent workspace (gitignored)
│   ├── process.md             # Progress tracker with @agent-name tags
│   ├── requirements/
│   ├── evaluations/
│   ├── architecture/
│   ├── code_reviews/
│   ├── tests/
│   └── agent_memory/
├── .opencode/
│   └── agents/                # Agent definition files (auto-generated by tier)
└── .gitignore                 # +agents_files/
```

### Git Convention

```
<type>(<scope>): <brief description>

修改文件:
- path/to/file1.ext (<新增/修改/删除>)

修改原因: <why>

修改结果: <what>
```

Types: `feat` | `fix` | `refactor` | `style` | `docs` | `test` | `chore` | `revert`

Mandatory commits on 9 trigger events — no Client reminder needed.

### License

MIT — see [LICENSE](LICENSE).

---

<a id="chinese"></a>
## 中文

**Better-Code** 是一个 opencode skill，实现塔式多 Agent 协作编程工作流。一个 **项目负责人 (Project Lead)** 作为与您（甲方）的唯一接口，协调整合最多 **11 种专业 Agent 角色**，通过 6 个结构化阶段推进项目。从单人项目到企业级多团队系统均可伸缩。

### 架构

```
甲方 (Client)
  │
  └── 项目负责人 (塔顶) ── 唯一沟通桥梁
        │
        ├── 需求分析师 (×≤3)
        ├── 方案评估师 (×≤3)
        ├── 代码架构师 (×1)
        ├── 代码编程师 (×≤3)
        ├── 代码审核师 (×≤3)
        ├── 前端设计师 (×1)
        ├── 测试工程师 (×≤3)
        │
        ├── [Tier 2] DevOps 工程师 (×1)
        ├── [Tier 3] 安全审计师 (×1)
        ├── [Tier 3] 技术文档师 (×1)
        └── [Tier 3] 项目经理 (×1)
```

### 三级伸缩

按项目规模选择团队配置，从小开始，按需扩展。

| Tier | 适用场景 | 角色数 | 质量升级 |
|------|----------|--------|----------|
| **1 · Solo** | ≤5 模块，单人开发 | 7 个核心角色 | type-check + test + build |
| **2 · Team** | 6-15 模块，1-2 并行团队 | + DevOps | + 覆盖率≥80%、依赖审计、ADR、dev/staging 环境 |
| **3 · Enterprise** | 16+ 模块，3+ 并行团队 | + 安全审计、技术文档、项目经理 | + OWASP 扫描、密钥检测、契约/E2E 测试、事故响应、跨团队协调 |

初始化时项目负责人扫描项目并推荐 tier。**甲方始终拥有最终决定权** — 可随意增减角色和规则。

### 核心特性

- **唯一接口** — 项目负责人是子 Agent 的唯一桥梁，子 Agent 绝不直接与甲方沟通
- **6 阶段流水线** — 初始化 → 需求分析 → 方案评估 → 架构设计 → 实现（编程→审核→测试→UI审核）→ 交付验收与回顾
- **11 种 Agent 角色** — 7 核心 + 4 扩展（DevOps、安全、文档、项目管理），按 tier 启用
- **并行执行** — 通过 `[可并行]` 标记并发的模块，文件所有权零交集保障安全
- **质量闸门** — 提交前（lint + type-check），合并前（test + build + coverage + audit），失败即阻塞
- **Git 治理** — 标准化提交、feature 分支、合并前闸门（`merge --no-commit` → 验证 → 提交）、回滚流程、语义化版本 + changelog
- **迭代保护** — 各阶段最大迭代上限，达到上限时甲方拍板
- **智能测试** — 可测代码自动化覆盖；依赖数据库/内网/第三方API的产出人工测试指南
- **甲方主导** — 每个里程碑、每次 push、每项 tier 决策都需甲方确认
- **回顾复盘** — 交付后总结：什么做得好、什么出了问题、skill 本身如何优化

### 文件结构

```
skills/better-code/
├── SKILL.md                     # 编排入口：概览、伸缩矩阵、文件索引
├── config-example.jsonc         # opencode.jsonc 命令注册示例
├── roles/                       # Agent 角色定义（每角色一文件）
│   ├── requirement-analyst.md   # Tier 1
│   ├── evaluation-analyst.md    # Tier 1
│   ├── code-architect.md        # Tier 1
│   ├── code-programmer.md       # Tier 1
│   ├── code-reviewer.md         # Tier 1
│   ├── frontend-designer.md     # Tier 1（网页项目专属）
│   ├── test-engineer.md         # Tier 1
│   ├── devops-engineer.md       # Tier 2
│   ├── security-auditor.md      # Tier 3
│   ├── tech-writer.md           # Tier 3
│   └── project-manager.md       # Tier 3
├── phases/                      # 阶段流程
│   ├── 01-initialization.md     # 项目扫描 + tier 推荐 + 工作区创建
│   ├── 02-requirements.md       # 头脑风暴、需求澄清、文档格式
│   ├── 03-evaluation.md         # 可行性、工时、风险分析
│   ├── 04-architecture.md       # 模块拆分、依赖图、文件所有权
│   ├── 05-implementation.md     # 编程→审核→测试→UI审核→合并
│   └── 06-delivery.md           # 验收、版本标签、changelog、回顾
├── governance/                  # 跨阶段治理规则
│   ├── git-management.md        # 分支策略、提交模板、合并闸门、回滚
│   ├── quality-gates.md         # type-check/test/build/coverage/lint/audit
│   ├── environment-strategy.md  # dev/staging/prod 环境分离、密钥管理（Tier 2+）
│   └── incident-response.md     # 事故分级、回滚 vs 热修复决策（Tier 3）
└── templates/                   # 文档格式模板
    ├── requirement-doc.md
    ├── evaluation-report.md
    ├── architecture-doc.md
    ├── code-review-report.md
    ├── adr.md                    # 架构决策记录（Tier 2+）
    └── changelog.md
```

### 安装

1. 将整个 `better-code/` 目录复制到 skills 位置：

   ```
   # 全局（所有项目可用）
   ~/.config/opencode/skills/better-code/

   # 项目专属
   .opencode/skills/better-code/
   ```

2. 在 `opencode.json` 或 `opencode.jsonc` 中注册 `/better-code` 命令（参考 `config-example.jsonc`）

3. 重启 opencode

### 快速开始

1. 在 opencode 中进入项目目录
2. 输入 `/better-code`
3. 首次使用时输入 `请你先初始化`
   - 项目负责人扫描项目并推荐 tier
   - 你确认（或自定义）团队配置
   - 工作区自动创建
4. 描述需求 — 流水线启动

### 工作流阶段

| 阶段 | 参与 Agent | 产出物 |
|------|-----------|--------|
| **0. 初始化** | 项目负责人 | `agents_files/`、`.opencode/agents/`、环境、Git、tier 配置 |
| **1. 需求分析** | 负责人 + 分析师 (≤3) | `需求文档_vN.md` |
| **2. 方案评估** | 评估师 (≤3) | `方案评估报告_vN.md` |
| **3. 架构设计** | 架构师 (×1) | `architecture_design.md` |
| **4. 实现阶段** | 编程师→审核师→测试师→前端设计师 | 代码 + 审核报告 + 测试报告 |
| **5. 交付验收** | 项目负责人 | 合并 → 版本标签 → changelog → 回顾 |

### 项目工作区（初始化时自动创建）

```
project_root/
├── agents_files/              # Agent 工作区（已 gitignore）
│   ├── process.md             # 进度追踪，@agent名 标注责任
│   ├── requirements/
│   ├── evaluations/
│   ├── architecture/
│   ├── code_reviews/
│   ├── tests/
│   └── agent_memory/
├── .opencode/
│   └── agents/                # Agent 定义文件（按 tier 自动生成）
└── .gitignore                 # 已追加 agents_files/
```

### Git 提交规范

```
<type>(<scope>): <简短描述>

修改文件:
- path/to/file1.ext (<新增/修改/删除>)

修改原因: <为什么>

修改结果: <效果>
```

类型：`feat` | `fix` | `refactor` | `style` | `docs` | `test` | `chore` | `revert`

9 个触发事件强制执行提交，不等甲方提醒。

### 许可证

MIT — 详见 [LICENSE](LICENSE) 文件。
