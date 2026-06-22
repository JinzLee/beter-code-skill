# Phase 0: Initialization (初始化)

> 触发: Client 首次 invoke `/better-code` 或说 "请你先初始化"

## Step 1: Project Assessment (项目扫描)

Project Lead 扫描项目并评估规模：

| 检查项 | 方法 | 用于判定 |
|--------|------|----------|
| 现有代码量 | 统计源文件数、总行数 | 项目规模 |
| 技术栈 | 读 `package.json` / `pyproject.toml` 等 | 虚拟环境类型 |
| 是否有前端 | 查找 `.vue`/`.tsx`/`.html` 等 | 是否启用前端设计师 |
| 模块/子系统数 | 统计独立目录 | Tier 判定 |
| 现有 Git 状态 | `git status` / `git log` | 是否复用 |
| 现有文档 | 检查 README / docs 目录 | 文档需求 |

## Step 2: Tier Recommendation (规模推荐)

Project Lead 根据扫描结果，按 `SKILL.md` 中的 Project Scaling Matrix 推荐 tier：

```
根据项目扫描结果，建议采用以下团队配置：

📊 项目规模: <模块数> 模块 / <源文件数> 文件
🏷️ 推荐 Tier: Tier X (<Solo/Team/Enterprise>)

该 Tier 包含:
  ✅ <启用的角色列表>
  ✅ <启用的规则列表>
  ❌ <未启用的角色/规则>

是否确认？甲方可以增减任何角色和规则。
```

## Step 3: Client Confirmation

甲方确认以下信息：
1. 技术栈
2. 虚拟环境类型
3. 是否需要网页前端
4. 团队工作范围（外网访问等）
5. **Tier 配置** → 甲方可自定义增减

## Step 4: Directory Structure

在项目根目录创建：

```
project_root/
├── agents_files/
│   ├── process.md
│   ├── requirements/
│   ├── evaluations/
│   ├── architecture/
│   ├── code_reviews/
│   ├── tests/
│   └── agent_memory/
├── .opencode/agents/
│   └── <按 Tier 创建 role 定义文件>
```

## Step 5: Agent Definition Files

- 将启用的 role 文件内容写入 `.opencode/agents/<name>.md`
- 每个文件头部包含 frontmatter（`description` + `mode: subagent`）
- 文件数量 = 启用的角色数

## Step 6: Git & Environment

- Git: 无 repo → `git init` + 初始 commit；有 repo → 甲方确认后使用
- Virtual env: 按技术栈创建或确认使用现有

## Step 7: process.md

写入初始 process.md，包含空任务清单，格式见 `SKILL.md` Section 5。

## Step 8: Confirmation Output

```
✅ 辅助团队初始化完成
📁 agents_files/ — Agent 工作文件目录
📁 .opencode/agents/ — Agent 定义文件 (<N> 个角色)
📄 agents_files/process.md — 项目进度追踪
📜 .gitignore — 已添加 agents_files/
🔧 Git — <状态>
🐍 虚拟环境 — <状态>
🎯 技术栈 — <tech stack>
🏷️ Tier — <X> (<启用的角色名列表>)

请甲方说明需求，我们开始分析。
```
