# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/ahmdd4vd/apos)

[![Watch the APOS overview video](./docs/images/apos-overview-thumbnail.png)](https://youtu.be/JedSiPIMITA)

**APOS (AI Project Operating System)** is a project-governance skill that helps coding agents maintain continuity across software projects. APOS keeps project context, requirements, architecture, technical decisions, task ownership, changelogs, and durable knowledge aligned across sessions.

> APOS is a guidance and synchronization layer. It does not replace the coding agent or perform irreversible actions without the appropriate authorization.

## Key features

- Inspects repository state before starting non-trivial work.
- Classifies changes by risk: trivial, routine, significant, or critical.
- Keeps goals, requirements, tasks, architecture, decisions, changelogs, and memory connected.
- Applies a proportional workflow: `READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`.
- Helps prevent architecture and documentation from silently drifting away from validated implementation reality.
- Supports project health audits and severity-based drift reporting.
- Provides a concise, actionable final-report format.

## Install through skills.sh

Use `npx` to run the `skills` CLI and add APOS from this repository:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos
```

The command installs the skill for the current project. To install it globally so it is available across projects, add the `-g` flag:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -g
```

To target a specific agent, use the `--agent` option. For example, for Claude Code:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --agent claude-code
```

To list the skills available in this repository without installing them:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --list
```

For non-interactive installation:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -y
```

See the [skills.sh documentation](https://www.skills.sh/docs) for the complete CLI reference.

## Usage

### When to use APOS

Use APOS whenever a task can affect project intent, implementation structure, ownership, or durable project knowledge. Typical examples include:

- planning or implementing a new feature;
- fixing a bug that touches more than one file or component;
- changing an API, database, authentication flow, deployment boundary, or shared component;
- updating requirements, architecture documentation, or technical decisions;
- coordinating multiple agents, branches, or worktrees;
- investigating suspected documentation or project-state drift; or
- preparing a release or major migration.

For isolated typo fixes, formatting changes, or other low-risk edits, APOS uses a lightweight process rather than requiring a full project record.

### Start a task with APOS

After installing the skill, ask your coding agent to use APOS explicitly when you want predictable governance. For example:

```text
Use APOS for this task. Inspect the repository state first, classify the change, identify the relevant task and architecture decisions, implement the smallest safe change, run proportional validation, update affected project artifacts, and provide an APOS Report.
```

For a feature request, provide the desired outcome and ask APOS to preserve traceability:

```text
Use APOS to add email-based password reset. Review the existing goals, requirements, architecture, and decisions first. Create or update the relevant task, explain the impact and security considerations, implement the feature, run tests, update documentation and changelog entries that are materially affected, and report any follow-up work.
```

For an audit:

```text
Run an APOS health audit. Check for stale or unowned tasks, missing requirements, conflicting decisions, architecture drift, incomplete changelog entries, stale projections, and known issues without follow-up tasks. Report findings by severity with recommended next actions.
```

### APOS workflow

APOS selects the smallest workflow that is safe for the change:

| Change type | Typical examples | Workflow |
| --- | --- | --- |
| **Trivial** | Formatting, typo fixes, isolated documentation edits | Read, execute, validate, report |
| **Routine** | Bug fixes, tests, small features, localized refactors | Read, analyze, plan, execute, validate, update state, report |
| **Significant** | API, database, authentication, shared component, or product-behavior changes | Read, analyze, plan, impact check, execute, validate, update state, report |
| **Critical** | Breaking changes, destructive migrations, security, permissions, billing, irreversible operations | Full impact analysis, explicit risks and rollback plan, authorization checks, execution, validation, and state updates |

Do not skip repository inspection for non-trivial work. The agent should first check the current code, tests, configuration, and any existing `.apos/` artifacts before making assumptions.

### Initialize project state

If the repository already contains `.apos/`, read only the relevant files before changing the project. If it does not exist, create only the directories needed by the current task:

```bash
mkdir -p .apos/goals .apos/tasks .apos/decisions
```

Do not create every possible directory by default. A small project may begin with only a goal and a task; additional directories can be added when they become useful.

A task record should normally include:

- a stable ID and clear title;
- owner, status, and priority;
- dependencies and affected areas;
- links to requirements, goals, or decisions; and
- validation or acceptance criteria.

Use the project’s existing conventions when they differ. APOS commonly recognizes the statuses `Backlog`, `Planned`, `In Progress`, `Review`, `Completed`, and `Archived`.

### Keep documentation synchronized

Update only the artifacts materially affected by the change. Avoid copying the same information into multiple files. As a default guide:

| Change | Usually update |
| --- | --- |
| Small bug fix | Task and changelog, if the project maintains one |
| New user-facing feature | Task, PRD or requirements, and changelog |
| API or database change | Task, architecture/API documentation, migration notes, and changelog |
| New technical direction | Decision record, architecture documentation, and task |
| Durable technical discovery | Memory plus the relevant task or decision |
| Worktree lifecycle change | Worktree registry, when one is active |
| Concurrent agent ownership | Agent registry, when multiple agents are active |

When implementation, documentation, and requirements disagree, do not silently choose one. APOS uses this order of precedence for current work:

1. Validated code and runtime behavior;
2. explicit architecture decisions;
3. PRD and goals;
4. tasks;
5. changelog entries; and
6. memory and durable discoveries.

The conflict should be reported and the appropriate source document should be synchronized.

### Review architecture and decisions

Before changing a database, API, authentication flow, core service, shared component, deployment boundary, or public contract:

1. Read the relevant architecture documentation.
2. Search `.apos/decisions/` for prior solutions.
3. Identify affected modules and dependencies.
4. Describe breaking changes, migration needs, risks, and validation steps.
5. Record a decision when the accepted technical direction changes.

Do not silently override an accepted decision. If a new requirement conflicts with an existing decision, explain the conflict and recommend whether the decision, requirement, or implementation should change.

### Validate and report

Validation should match the risk of the change. Depending on the project, this may include unit tests, integration tests, type checking, linting, build verification, migration checks, security checks, or a manual smoke test.

A normal APOS final report should contain:

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

For trivial changes, omit empty sections. For significant or critical changes, include impact analysis, migration or rollback notes, unresolved conflicts, and any required authorization or follow-up work.

### Run a health audit

Request an audit before a major release, after a long period of inactivity, or whenever project drift is suspected. APOS should inspect:

- stale, blocked, or unowned tasks;
- significant features without requirements;
- decisions without affected scope;
- architecture references to missing modules;
- incomplete changelog entries;
- stale or missing projections;
- conflicting status or ownership metadata; and
- known issues without follow-up tasks.

Report findings by severity instead of inventing an opaque numeric score:

- **Critical:** data loss, security, release, or severe integrity risk;
- **High:** important architecture, ownership, or requirement mismatch;
- **Medium:** meaningful maintenance or synchronization gap; and
- **Low:** housekeeping or stale metadata.

## Supported project structure

When needed, APOS uses an `.apos/` directory at the repository root:

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

Additional directories such as `worktrees/`, `agents/`, and `reports/` are created only when they are actually needed. APOS does not recommend creating empty administrative structures.

## Design principles

1. Protect project intent and implementation reality.
2. Use the smallest process that remains safe.
3. Make material changes traceable.
4. Preserve important decisions.
5. Make ownership and responsibility boundaries explicit.
6. Treat architecture changes as risk-bearing changes.
7. Make important knowledge survive across sessions.
8. Avoid duplicating sources of truth.
9. Prefer consistency without unnecessary bureaucracy.
10. Leave the project understandable to a new developer.

## Agent instruction integration

Installing the APOS skill does not automatically change a project. To make agents consistently remember APOS, adopt it in the project’s agent instruction files.

Keep the full workflow in [`skill/apos/SKILL.md`](./skill/apos/SKILL.md), and add a short mandatory reminder to `AGENTS.md` and/or `CLAUDE.md`:

```markdown
## APOS Governance

This project uses APOS for project governance and continuity.

For every non-trivial task:
1. Inspect the repository and relevant `.apos/` state.
2. Classify the change and create or update a task when required.
3. Run proportional validation.
4. Run the APOS Finish Protocol before reporting completion.
5. Synchronize affected tasks, decisions, architecture, changelog, or memory.
6. Provide an APOS Report.

Full workflow: `skill/apos/SKILL.md`
Project state: `.apos/`
```

Do not copy the entire skill into `AGENTS.md` or `CLAUDE.md`. Preserve existing instructions and append a clearly separated APOS section. For an empty project, create the minimum `.apos/` state and the agent-instruction integration after the project intent is clear. For an existing project, inspect and preserve existing instruction files before adding APOS.

Installing the skill and adopting APOS are separate actions:

```text
Install skill:  npx skills add <skill-url>
Adopt project:  add/update AGENTS.md, CLAUDE.md, and minimum .apos/ state
```

## Video overview

Watch the [APOS overview video](https://youtu.be/JedSiPIMITA) to see the repository detection, Bootstrap Protocol, Start Protocol, Finish Protocol, and agent integration flow. Click the thumbnail above to watch it on YouTube.



## Files

- [`skill/apos/SKILL.md`](./skill/apos/SKILL.md) — the main instructions loaded by coding agents.

## Status

APOS is currently in the **Beta / governance skill** stage. This repository contains the skill that can be installed through skills.sh. The APOS API, console, CLI, MCP adapter, and database implementation are planned separately.

## Contributing

Before changing `skill/apos/SKILL.md`, keep the instructions concise, broadly applicable across repositories, and free of unnecessary documentation duplication. For material workflow changes, document the relevant technical decision and update the project documentation as needed.

## License

A license has not yet been specified for this repository.
