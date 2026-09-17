# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/b/ahmdd4vd/apos)

[![Watch the APOS overview video](./docs/images/apos-overview-thumbnail.png)](https://youtu.be/JedSiPIMITA)

**APOS（AI Project Operating System）** 是一个项目治理 skill，帮助 coding agent 在软件项目中保持连续性。APOS 用于在不同会话之间同步项目上下文、需求、架构、技术决策、任务负责人、变更日志和长期知识。

> APOS 是指导与同步层。它不取代 coding agent，也不会在没有适当授权的情况下执行不可逆操作。

## 主要功能

- 在开始非简单任务前检查仓库的实际状态。
- 按风险将变更分为 trivial、routine、significant 和 critical。
- 关联 goals、requirements、tasks、architecture、decisions、changelog 和 memory。
- 使用与风险匹配的流程：`READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`。
- 防止架构和文档悄悄偏离已经验证的实现。
- 支持项目健康审计和按严重级别报告 drift。
- 提供简洁、可执行的最终报告格式。

## 通过 skills.sh 安装

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos
```

上述命令会为当前项目安装 skill。若要全局安装：

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -g
```

若要指定 agent，可以使用 `--agent`，例如 Claude Code：

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --agent claude-code
```

查看可用 skill 或进行非交互安装：

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --list
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -y
```

完整 CLI 参考请查看 [skills.sh 文档](https://www.skills.sh/docs)。

## 使用指南

### 何时使用 APOS

以下情况建议使用 APOS：开发新功能、跨组件修复 bug、修改 API/数据库/认证流程、调整架构、协调多个 agent 或 worktree、审计项目 drift，以及准备重要发布或迁移。对于拼写修正和简单格式调整，APOS 会使用轻量流程。

### 开始任务

可以在 prompt 中明确要求 agent 使用 APOS：

```text
Use APOS for this task. Inspect the repository state first, classify the change, identify relevant tasks and architecture decisions, implement the smallest safe change, validate it, update affected artifacts, and provide an APOS Report.
```

开发新功能时，应要求 agent 先阅读 goals、requirements、architecture 和 decisions，再创建或更新 task。

### 按风险执行流程

| 类型 | 示例 | 流程 |
| --- | --- | --- |
| **Trivial** | 拼写、格式、局部文档修改 | Read, execute, validate, report |
| **Routine** | Bug 修复、测试、小功能、局部重构 | Read, analyze, plan, execute, validate, update state, report |
| **Significant** | API、数据库、认证、共享组件 | Read, analyze, impact check, execute, validate, update state, report |
| **Critical** | Breaking change、破坏性迁移、安全、权限 | 完整影响分析、风险与 rollback、授权检查、执行并验证 |

### 项目状态与文档

如果仓库中已有 `.apos/`，先读取与任务相关的文件。如果没有，只创建当前任务需要的目录：

```text
.apos/
├── goals/
├── prd/
├── architecture/
├── decisions/
├── tasks/
├── changelog/
└── memory/
```

Task 通常应包含稳定 ID、标题、负责人、状态、优先级、依赖、requirements 或 decisions 的引用，以及验证标准。只更新受到实质影响的 artifact，不要默认创建空的管理目录。

### 验证、审计与报告

验证应与变更风险匹配，可以包括 tests、type checking、linting、build、migration check、security check 或 smoke test。审计应检查过期或无负责人的 task、缺失的 requirements、冲突的 decisions、架构 drift、不完整的 changelog、过期 projection，以及没有后续任务的 issue。

使用 **Critical**、**High**、**Medium** 和 **Low** 表示严重程度，不要使用无法解释的数字评分。报告格式：

```markdown
## APOS Report

### Change
- Summary:
- Classification:
- Task:

### Validation
- Checks run:
- Result:

### State updates
- Updated artifacts:
- Intentionally unchanged artifacts:

### Risks and follow-up
- Known risks:
- Follow-up work:
```

## 设计原则

1. 保护项目意图和实现现实。
2. 使用仍然安全的最小流程。
3. 让重要变更可追踪。
4. 保存重要决策。
5. 明确负责人和责任边界。
6. 将架构变更视为高风险变更。
7. 让重要知识跨会话保留。
8. 避免重复 source of truth。
9. 保持一致性，同时避免不必要的官僚流程。
10. 让新开发者也能理解项目。

## Agent 指令集成

安装 APOS skill 不会自动修改项目。为了让 agent 始终记住 APOS，请在项目的 `AGENTS.md` 和/或 `CLAUDE.md` 中加入 APOS 集成说明。

完整流程保留在 [`skill/apos/SKILL.md`](./skill/apos/SKILL.md) 中。指令文件只需要包含简短的强制提醒：检查 `.apos/`、分类任务、执行验证、完成前运行 Finish Protocol、同步受影响的状态并提供 APOS Report。不要复制整个 skill，并保留已有指令。

安装 skill 与在项目中采用 APOS 是两个不同的动作：先运行 `npx skills add`，再添加或更新 `AGENTS.md`、`CLAUDE.md` 和最小 `.apos/` 状态。空项目应在明确项目意图后执行；已有项目应先检查并保留现有指令。

## Video overview

Watch the [APOS overview video](https://youtu.be/JedSiPIMITA) to see the repository detection, Bootstrap Protocol, Start Protocol, Finish Protocol, and agent integration flow.



## 文件

- [`skill/apos/SKILL.md`](./skill/apos/SKILL.md) — coding agent 使用的主要指令。
- [`README.md`](./README.md) — 英文文档。

## 状态与许可证

APOS 当前处于 **Beta / governance skill** 阶段。API、console、CLI、MCP adapter 和 database 将另行规划。仓库尚未指定许可证。
