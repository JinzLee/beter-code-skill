# Better-Code · 塔式多Agent协作编程

[English](#english) | [中文](#中文)

---

<a id="english"></a>
## English

**Better-Code** is an opencode skill that implements a tower-style multi-agent collaborative coding workflow. A single **Project Lead** agent interfaces directly with you (the Client), orchestrating up to 7 specialized sub-agent teams through a structured 6-phase pipeline — from requirements analysis to production-ready code.

### Architecture

```
┌─────────────────────────────────────────┐
│         Client (User / 甲方)             │
│              ▲  │  direct comm           │
│              │  ▼  only                 │
│     ┌──────────────────────┐            │
│     │   Project Lead (主Agent) │◄── Tower top
│     └──┬───┬───┬───┬───┬──┬┘            │
│        │   │   │   │   │  │             │
│  ┌─────▼┐ ┌▼──┐ ┌▼──┐ │  │  ┌▼──────┐  │
│  │需求分析│ │方案│ │架构│ │  │  │前端设计│  │
│  │ (≤3) │ │评估│ │(×1)│ │  │  │ (×1)  │  │
│  └──────┘ │(≤3)│ └────┘ │  │  └───────┘  │
│           └────┘        │  │             │
│         ┌────▼────┐  ┌──▼──▼──┐         │
│         │代码编程  │  │代码审核 │         │
│         │ (≤3)    │  │ (≤3)   │         │
│         └────┬────┘  └────┬───┘         │
│              │            │             │
│         ┌────▼────────────▼──┐          │
│         │   测试工程师 (≤3)   │          │
│         └───────────────────┘          │
└─────────────────────────────────────────┘
```

### Key Features

- **Tower-Style Management** — One Project Lead, many specialists. No sub-agent ever talks to the Client directly.
- **6-Phase Pipeline** — Init → Requirements → Evaluation → Architecture → Implementation (Code → Review → Test) → Delivery
- **7 Specialized Agent Roles** — Requirements Analyst, Solution Evaluator, Code Architect, Code Programmer, Code Reviewer, Frontend Designer, Test Engineer
- **Parallel Execution** — Up to 3 programmers/analysts/evaluators/reviewers working in parallel on independent modules
- **Built-in Quality Gates** — Code review classifies issues as architecture-level or coding-level, routing fixes to the right agent
- **Iteration Safeguards** — Max iterations per phase prevent infinite loops; Client makes the final call at limits
- **Client-First Decision Making** — The Client is always in control. Every milestone, every git push requires approval.
- **Domain-Unfamiliarity Protection** — When the Client is unfamiliar with a domain, the system warns about key considerations BEFORE asking for decisions.
- **Smart Testing** — Auto-tests what's testable; produces manual test guides for code requiring DB connections, internal networks, or third-party APIs.
- **Git Integration** — Standardized commit messages (type + scope + files + reason + result), branch strategy, push-approval policy.
- **Process Tracking** — `process.md` tracks every subtask with `@agent-name` accountability tags.

### Workflow Phases

| Phase | Agents | Output |
|-------|--------|--------|
| **0. Init** | Project Lead | `agents_files/`, `.opencode/agents/`, virtual env, git setup |
| **1. Requirements** | Lead + Analysts (≤3) | `agents_files/requirements/需求文档_vN.md` |
| **2. Evaluation** | Evaluators (≤3) | `agents_files/evaluations/方案评估报告_vN.md` |
| **3. Architecture** | Architect (×1) | `agents_files/architecture/architecture_design.md` |
| **4. Implementation** | Programmers (≤3) → Reviewers (≤3) → Testers (≤3) → Frontend (×1, optional) | Code + review reports + test reports |
| **5. Delivery** | Project Lead | Git commit with Client approval |

### Agent Roles

| # | Role | Max | Responsibility |
|---|------|-----|----------------|
| 1 | **需求分析师** (Requirement Analyst) | 3 | Brainstorm needs, identify gaps, warn about unfamiliar domains |
| 2 | **方案评估师** (Solution Evaluator) | 3 | Assess feasibility, time, risk; recommend ✅/⚠️/❌ |
| 3 | **代码架构师** (Code Architect) | 1 | Design architecture, module breakdown, dependency graph |
| 4 | **代码编程师** (Code Programmer) | 3 | Implement assigned modules per spec |
| 5 | **代码审核师** (Code Reviewer) | 3 | Review code, classify issues (architecture-level vs coding-level) |
| 6 | **前端设计师** (Frontend Designer) | 1 | UI/UX review (web projects only) |
| 7 | **测试工程师** (Test Engineer) | 3 | Write/run tests; manual guides for untestable code |

### Installation

1. Copy `SKILL.md` to your global or project skills directory:

   ```
   # Global (all projects)
   ~/.config/opencode/skills/better-code/SKILL.md

   # Project-specific
   .opencode/skills/better-code/SKILL.md
   ```

2. Register the `/better-code` command in your `opencode.json` or `opencode.jsonc`:

   ```jsonc
   {
     "$schema": "https://opencode.ai/config.json",
     "command": {
       "better-code": {
         "description": "启动塔式多Agent协作编程流程",
         "prompt": "加载 better-code skill，以项目负责人身份接管当前项目..."
       }
     }
   }
   ```

   See `config-example.jsonc` for the full command prompt.

3. Restart opencode.

### Quick Start

1. Navigate to your project directory in opencode
2. Type `/better-code`
3. The Project Lead will check if `agents_files/` exists:
   - **New project**: Type `请你先初始化` to initialize the workspace
   - **Existing session**: Resumes from `process.md` last checkpoint
4. Answer the initialization questions (tech stack, virtual environment, work scope)
5. Describe your requirements — the pipeline begins.

### Git Commit Convention

```
<type>(<scope>): <brief description>

修改文件:
- path/to/file1.py (<新增/修改/删除>)
- path/to/file2.py

修改原因: <why this change>

修改结果: <what effect this change achieves>
```

Types: `feat` | `fix` | `refactor` | `style` | `docs` | `test` | `chore`

### Project Workspace

After initialization, the project gains:

```
project_root/
├── agents_files/              # Agent workspace (gitignored)
│   ├── process.md             # Progress tracker
│   ├── requirements/          # Requirement docs
│   ├── evaluations/           # Evaluation reports
│   ├── architecture/          # Architecture designs
│   ├── code_reviews/          # Code review reports
│   ├── tests/                 # Test reports & guides
│   └── agent_memory/          # Agent state persistence
├── .opencode/
│   └── agents/                # Agent definition files (7 agents)
│       ├── requirement-analyst.md
│       ├── evaluation-analyst.md
│       ├── code-architect.md
│       ├── code-programmer.md
│       ├── code-reviewer.md
│       ├── frontend-designer.md
│       └── test-engineer.md
└── .gitignore                 # +agents_files/
```

### Permissions

Sub-agent permissions are reviewed by the Project Lead. If the scope is unclear, the Project Lead asks the Client: *"What is the team's acceptable working range?"* (e.g., external network access, file modifications outside project root).

### License

MIT — see [LICENSE](LICENSE) file.

---

<a id="中文"></a>
## 中文

**Better-Code** 是一个 opencode skill，实现了塔式多 Agent 协作编程工作流。一个 **项目负责人 (Project Lead)** 作为与您（甲方）的唯一接口，协调整合最多 7 种专业子 Agent 团队，通过 6 个结构化阶段推进项目——从需求分析到可交付代码。

### 架构图

```
┌─────────────────────────────────────────┐
│         用户 (甲方 / Client)              │
│              ▲  │  唯一沟通通道            │
│              │  ▼                       │
│     ┌──────────────────────┐            │
│     │   项目负责人 (主Agent)  │◄── 塔顶     │
│     └──┬───┬───┬───┬───┬──┬┘            │
│        │   │   │   │   │  │             │
│  ┌─────▼┐ ┌▼──┐ ┌▼──┐ │  │  ┌▼──────┐  │
│  │需求分析│ │方案│ │架构│ │  │  │前端设计│  │
│  │ (≤3) │ │评估│ │(×1)│ │  │  │ (×1)  │  │
│  └──────┘ │(≤3)│ └────┘ │  │  └───────┘  │
│           └────┘        │  │             │
│         ┌────▼────┐  ┌──▼──▼──┐         │
│         │代码编程  │  │代码审核 │         │
│         │ (≤3)    │  │ (≤3)   │         │
│         └────┬────┘  └────┬───┘         │
│              │            │             │
│         ┌────▼────────────▼──┐          │
│         │   测试工程师 (≤3)   │          │
│         └───────────────────┘          │
└─────────────────────────────────────────┘
```

### 核心特性

- **塔式管理** — 一个项目负责人，多个专家。子 Agent 绝不直接与甲方沟通。
- **6 阶段流水线** — 初始化 → 需求分析 → 方案评估 → 架构设计 → 实现（编程→审核→测试）→ 交付
- **7 种专业角色** — 需求分析师、方案评估师、代码架构师、代码编程师、代码审核师、前端设计师、测试工程师
- **并行执行** — 最多 3 个编程师/分析师/评估师并行处理独立模块
- **内置质量关卡** — 代码审核区分架构级问题和编码级问题，精准返工
- **迭代保护** — 各阶段设置最大迭代次数，上限时由甲方拍板决定
- **甲方主导决策** — 每个里程碑、每次 git push 都需甲方确认
- **领域不熟保护** — 甲方对某领域不熟悉时，先提醒注意事项再让甲方决定
- **智能测试** — 可自动测试的自动化覆盖；需要数据库/内网/第三方API的产出人工测试指南
- **Git 集成** — 标准化提交格式（类型+范围+文件+原因+结果）、分支策略、推送审批
- **进度追踪** — `process.md` 记录每个子任务并标注 `@agent名` 实现责任追溯

### 工作流阶段

| 阶段 | 参与 Agent | 产出物 |
|------|-----------|--------|
| **0. 初始化** | 项目负责人 | `agents_files/`、`.opencode/agents/`、虚拟环境、Git 配置 |
| **1. 需求分析** | 负责人 + 分析师 (≤3) | `需求文档_vN.md` |
| **2. 方案评估** | 评估师 (≤3) | `方案评估报告_vN.md` |
| **3. 架构设计** | 架构师 (×1) | `architecture_design.md` |
| **4. 实现阶段** | 编程师 (≤3) → 审核师 (≤3) → 测试师 (≤3) → 前端 (×1, 可选) | 代码 + 审核报告 + 测试报告 |
| **5. 交付** | 项目负责人 | Git 提交（甲方确认后） |

### Agent 角色详解

| # | 角色 | 上限 | 职责 |
|---|------|------|------|
| 1 | **需求分析师** | 3 | 头脑风暴需求、识别模糊点、对甲方不熟领域预警 |
| 2 | **方案评估师** | 3 | 评估可行性/耗时/风险，给出 ✅/⚠️/❌ 建议 |
| 3 | **代码架构师** | 1 | 设计架构、模块拆分、依赖关系图 |
| 4 | **代码编程师** | 3 | 按架构规范实现指定模块 |
| 5 | **代码审核师** | 3 | 审核代码，区分架构级/编码级问题 |
| 6 | **前端设计师** | 1 | UI/UX 审核（仅网页项目） |
| 7 | **测试工程师** | 3 | 编写执行测试；不可测代码出人工测试指南 |

### 安装

1. 将 `SKILL.md` 复制到全局或项目 skills 目录：

   ```
   # 全局（所有项目可用）
   ~/.config/opencode/skills/better-code/SKILL.md

   # 项目专属
   .opencode/skills/better-code/SKILL.md
   ```

2. 在 `opencode.json` 或 `opencode.jsonc` 中注册 `/better-code` 命令（参考 `config-example.jsonc`）

3. 重启 opencode

### 快速开始

1. 在 opencode 中进入你的项目目录
2. 输入 `/better-code`
3. 项目负责人会检查 `agents_files/` 是否存在：
   - **新项目**：输入 `请你先初始化` 初始化工作区
   - **已有项目**：从 `process.md` 上次断点继续
4. 回答初始化问题（技术栈、虚拟环境、工作范围）
5. 描述你的需求——流水线启动

### Git 提交规范

```
<type>(<scope>): <简短描述>

修改文件:
- path/to/file1.py (<新增/修改/删除>)
- path/to/file2.py

修改原因: <为什么做这个改动>

修改结果: <改动后达到什么效果>
```

类型：`feat` | `fix` | `refactor` | `style` | `docs` | `test` | `chore`

### 项目工作区

初始化后，项目将创建：

```
project_root/
├── agents_files/              # Agent 工作区（已加入 .gitignore）
│   ├── process.md             # 进度追踪总表
│   ├── requirements/          # 需求文档
│   ├── evaluations/           # 评估报告
│   ├── architecture/          # 架构设计
│   ├── code_reviews/          # 代码审核报告
│   ├── tests/                 # 测试报告及指南
│   └── agent_memory/          # Agent 状态持久化
├── .opencode/
│   └── agents/                # 7 个 Agent 定义文件
│       ├── requirement-analyst.md
│       ├── evaluation-analyst.md
│       ├── code-architect.md
│       ├── code-programmer.md
│       ├── code-reviewer.md
│       ├── frontend-designer.md
│       └── test-engineer.md
└── .gitignore                 # 已追加 agents_files/
```

### 权限管控

子 Agent 的所有权限需经项目负责人审核。若负责人无法判断权限请求是否在项目范围内，会先询问甲方：*"辅助团队的工作范围是什么？"*（如：能否访问外网安装依赖、能否修改项目目录外的文件等）

### 许可证

MIT — 详见 [LICENSE](LICENSE) 文件。
