---
name: better-code
description: Use ONLY when user invokes /better-code. Tower-style multi-agent collaborative coding workflow. Main agent = Project Lead, sole interface with Client. Scales from solo to enterprise via tiered team assembly.
---

# Better-Code: Tower-Style Multi-Agent Collaborative Coding

## Overview

```
Client (甲方)
  │
  └── Project Lead (主 Agent) ── 唯一对外接口
        │
        ├── 需求分析师 (×≤3)     ── 头脑风暴、澄清需求
        ├── 方案评估师 (×≤3)     ── 可行性、工时、风险评估
        ├── 代码架构师 (×1)      ── 目录结构、模块划分、依赖图
        ├── 代码编程师 (×≤3)     ── 按模块实现代码
        ├── 代码审核师 (×≤3)     ── 功能/安全/可维护性审查
        ├── 前端设计师 (×1)      ── Web 项目 UI/UX 审查
        ├── 测试工程师 (×≤3)     ── 单元/集成/手动测试
        │
        ├── [可选] DevOps 工程师 (×1)    ── CI/CD、环境管理
        ├── [可选] 安全审计师 (×1)       ── 依赖漏洞、密钥扫描
        ├── [可选] 技术文档师 (×1)       ── ADR、API 文档、README
        └── [可选] 项目经理 (×1)         ── 进度追踪、干系人汇报、风险登记册
```

**管道流程**:
```
需求分析 → 方案评估 → 架构设计 → 实现(编程→审核→测试→UI审核) → 交付验收 → 回顾复盘
```

---

## When to Use

- Client invokes `/better-code` or says "请你先初始化"
- Other project-specific trigger phrases defined during initialization

## How It Works — Quick Start

1. **Initialization**: Project Lead scans project → assesses scale → asks Client to confirm team tier → creates directory structure → initializes Git
2. **Requirements → Evaluation → Architecture**: Phased pipeline, each phase produces a document, Client confirms before next phase
3. **Implementation**: Architect splits work into modules with `[可并行]`/`[串行]` markers → Project Lead assigns to programmers → review → test → merge
4. **Delivery**: Acceptance review → version tag → changelog → retrospective

---

## Project Scaling Matrix (初始化时评估)

Project Lead 在初始化阶段评估项目规模，向甲方推荐 tier，甲方确认后按对应 tier 启用角色和规则。

| 维度 | Tier 1: Solo | Tier 2: Team | Tier 3: Enterprise |
|------|-------------|-------------|-------------------|
| **项目模块数** | ≤5 个模块 | 6-15 个模块 | 16+ 个模块 |
| **并行团队** | 1 个（串行为主） | 1-2 个并行 | 3+ 个团队并行 |
| **环境需求** | 仅开发环境 | dev + staging | dev + staging + prod |
| **合规要求** | 无 | 基础安全审计 | 完整合规 + 安全审查 |
| **文档需求** | 仅 README | API 文档 + ADR | 全量技术文档 |

### Tier 1: Solo（默认）

| 启用 | 内容 |
|------|------|
| ✅ 核心 7 角色 | requirement-analyst / evaluation-analyst / code-architect / code-programmer / code-reviewer / frontend-designer / test-engineer |
| ✅ 基础规则 | git-management / quality-gates |
| ✅ 基础模板 | requirement-doc / evaluation-report / architecture-doc / code-review-report / changelog |
| ❌ 不启用 | DevOps / 安全审计师 / 技术文档师 / 项目经理 / ADR / 环境策略 / 事故响应 |

### Tier 2: Team

Tier 1 全部 + 以下：

| 新增启用 | 内容 |
|----------|------|
| ✅ DevOps 工程师 | CI/CD 配置、staging 环境 |
| ✅ 环境分级策略 | dev / staging 环境分离 |
| ✅ ADR | 架构决策记录 |
| ✅ 覆盖率门槛 | 80% 代码覆盖率底线 |
| ✅ 依赖安全审计 | `npm audit` / `pip audit` 集成到 CI |
| ✅ 估算 vs 实际对比 | 评估师收到工时偏差反馈 |

### Tier 3: Enterprise

Tier 2 全部 + 以下：

| 新增启用 | 内容 |
|----------|------|
| ✅ 安全审计师 | OWASP 审查、密钥扫描、CVE 监控 |
| ✅ 技术文档师 | API 文档同步、README 维护、知识库 |
| ✅ 项目经理 | 进度日历、干系人分层汇报、风险登记册 |
| ✅ 事故响应流程 | 生产事故：回滚 vs 热修复决策树 |
| ✅ 集成测试策略 | 契约测试 + E2E 冒烟测试 |
| ✅ 跨团队依赖管理 | API 契约文档 + mock-server + 集成窗口约定 |
| ✅ 完整合规 | 生产环境配置分离、密钥管理 |

---

## File Reference Map

### 角色定义 → `roles/`

| 文件 | 角色 | Tier |
|------|------|------|
| `roles/requirement-analyst.md` | 需求分析师 | 1+ |
| `roles/evaluation-analyst.md` | 方案评估师 | 1+ |
| `roles/code-architect.md` | 代码架构师 | 1+ |
| `roles/code-programmer.md` | 代码编程师 | 1+ |
| `roles/code-reviewer.md` | 代码审核师 | 1+ |
| `roles/frontend-designer.md` | 前端设计师 | 1+ |
| `roles/test-engineer.md` | 测试工程师 | 1+ |
| `roles/devops-engineer.md` | DevOps 工程师 | 2+ |
| `roles/security-auditor.md` | 安全审计师 | 3+ |
| `roles/tech-writer.md` | 技术文档师 | 3+ |
| `roles/project-manager.md` | 项目经理 | 3+ |

### 阶段流程 → `phases/`

| 文件 | 内容 |
|------|------|
| `phases/01-initialization.md` | 项目扫描 + Tier 评估 + 目录创建 + Git init |
| `phases/02-requirements.md` | 需求分析 + 头脑风暴 + 文档模板 |
| `phases/03-evaluation.md` | 方案评估 + 工时估算 + 风险清单 |
| `phases/04-architecture.md` | 架构设计 + 模块划分 + 文件所有权 |
| `phases/05-implementation.md` | 编程 → 审核 → 测试 → UI 审核全流程 + Git 增量提交 |
| `phases/06-delivery.md` | 验收 + 版本发布 + 回顾复盘 |

### 治理规则 → `governance/`

| 文件 | 内容 | Tier |
|------|------|------|
| `governance/git-management.md` | 分支策略、commit 模板、合并闸门、回滚 | 1+ |
| `governance/quality-gates.md` | Type-check / test / build / coverage / lint 闸门 | 1+ |
| `governance/environment-strategy.md` | dev/staging/prod 环境分离、密钥管理 | 2+ |
| `governance/incident-response.md` | 事故分级、回滚 vs 热修复决策、通报告知 | 3+ |

### 文档模板 → `templates/`

| 文件 | 内容 | Tier |
|------|------|------|
| `templates/requirement-doc.md` | 需求文档格式 | 1+ |
| `templates/evaluation-report.md` | 方案评估报告格式 | 1+ |
| `templates/architecture-doc.md` | 架构设计文档格式 | 1+ |
| `templates/code-review-report.md` | 代码审核报告格式 | 1+ |
| `templates/adr.md` | 架构决策记录格式 | 2+ |
| `templates/changelog.md` | 版本变更日志格式 | 1+ |

---

## Override Rule: 甲方最终决定

- 以上 Tier 分级是 **Project Lead 的建议**，甲方可以在初始化时**任意增减角色和规则**
- 甲方说"我是 solo 但要安全审计师" → 照做
- 甲方说"大项目但不需要项目经理" → 照做
- 如有明显风险（如移除安全审计导致合规漏洞），Project Lead 应提醒后再执行

## Immutable Rules (所有 Tier 强制遵守)

以下规则跨 Tier 不变，取自 `governance/` 各文件：

1. **单向通信**: 子 agent 永远不直接与甲方通信，必须通过 Project Lead
2. **语言约定**: `agents_files/` 文档用中文，代码注释不拘
3. **Git 强制提交**: 10.5 表中每个触发事件必须 commit，不等提醒
4. **禁止跨模块批量提交**: 一个模块一个 commit
5. **预合并闸门**: merge 前必须在合并结果上跑 type-check + test + build
6. **推送需甲方确认**: push 前展示分支、commit 列表、文件变更

详见各 governance 文件。
