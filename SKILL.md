---
name: apos
description: Project governance and continuity for software projects. Use when maintaining project state, planning or implementing changes, synchronizing architecture and documentation, tracking tasks and decisions, auditing project drift, or coordinating multiple agents and worktrees.
---

# APOS Beta

APOS (AI Project Operating System) is a project-governance and continuity layer for software projects. It helps implementation agents preserve project intent, architecture consistency, decision history, task ownership, documentation accuracy, and durable project knowledge.

APOS does not replace implementation agents. It guides them and keeps project state synchronized with validated reality.

## Objectives

Use APOS to reduce documentation drift, architecture drift, context loss, task and ownership ambiguity, forgotten technical decisions, worktree conflicts, and knowledge fragmentation. Favor project integrity without introducing process heavier than the change requires.

## Project State First

Before non-trivial work, inspect the actual repository state. If `.apos/` exists, read relevant files before making assumptions. If it does not exist, create only the directories needed for the current project and task; do not generate empty bureaucracy.

Typical structure:

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

Use these only when needed:

```text
.apos/worktrees/
.apos/agents/
.apos/reports/
```

Treat `.apos/` as project metadata, not as a substitute for source code, tests, or version control.

## Change Classification

Classify the request before execution and use the smallest workflow that safely handles it.

### Trivial

Formatting, typo fixes, isolated documentation edits, local renames, and non-functional cleanup. Inspect relevant files, make the change, run proportional validation, report the result, and update only materially affected state.

### Routine

Bug fixes, tests, small features, UI changes, and localized refactors. Identify or create a task, inspect relevant state, perform a lightweight impact check, implement and validate, then update materially affected state artifacts.

### Significant

Changes to APIs, databases, authentication, core services, shared components, architectural boundaries, or user-facing product behavior. Identify or create a task; create or update a PRD or requirements document when product scope or behavior changes; perform explicit impact analysis; check architecture and decisions; record a decision or update architecture documentation when direction changes; then implement, validate, and document migration or rollback considerations.

### Critical

Breaking changes, destructive migrations, security-sensitive changes, production infrastructure, permissions, billing, or irreversible operations. Perform full impact analysis, state risks and rollback plans, preserve decision records, and do not perform irreversible external actions without required authorization or confirmation.

## Default Workflow

Use the smallest safe workflow:

```text
Trivial:    READ → EXECUTE → VALIDATE → REPORT
Routine:    READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT
Significant: READ → ANALYZE → PLAN → IMPACT CHECK → EXECUTE → VALIDATE → UPDATE STATE → REPORT
```

Do not investigate indefinitely. Stop when scope, constraints, risks, plan, and validation method are clear.

## Goal and Requirement Alignment

For feature work or product behavior changes, read relevant goals and requirements, including `.apos/goals/project.md` and `.apos/prd/` when present. Check whether the change supports goals, conflicts with accepted requirements, and is necessary and in scope.

Create or update a PRD only when the change introduces or materially changes user-facing behavior, product scope, or requirements. Do not create one for ordinary maintenance, isolated bug fixes, tests, dependency updates, or behavior-preserving refactors. If a conflict exists, explain it before implementation and recommend a resolution.

## Task Ownership

Every routine, significant, or critical change should belong to a task. A useful task contains ID, title, owner, status, priority, dependencies, relevant PRD or requirement reference, and validation criteria. Use `Backlog`, `Planned`, `In Progress`, `Review`, `Completed`, and `Archived` unless the project defines compatible statuses. A trivial change does not require a formal task unless project policy or traceability requires one.

## Architecture and Decision Control

Before changing a database, API, authentication, core service, shared component, deployment boundary, or public contract:

1. Inspect relevant architecture documentation.
2. Search `.apos/decisions/` for prior solutions.
3. Identify affected modules and dependencies.
4. Describe breaking changes, migration needs, risks, and validation.
5. Preserve a decision record when the accepted technical direction changes.

Never silently override an accepted decision. Report conflicts between documentation, validated code, and new requirements, and identify which artifact should change.

## Documentation Synchronization

Keep material project documentation aligned with validated reality. Update only artifacts materially affected by the change; do not update every directory merely because it exists. Avoid duplicating the same source of truth.

| Change | Usually update |
|---|---|
| Small bug fix | Task, changelog if used |
| New user-facing feature | Task, PRD/requirements, changelog |
| API or database change | Task, architecture/API docs, migration notes, changelog |
| New technical direction | Decision record, architecture docs, task |
| Durable technical discovery | Memory and relevant task or decision |
| Worktree lifecycle change | Worktree registry when active |
| Concurrent agent ownership | Agent registry when multiple agents are active |

## Source of Truth

When artifacts conflict, use this precedence:

1. Validated code and runtime behavior for current implementation reality
2. Explicit architecture decisions for accepted technical direction
3. PRD and goals for intended product behavior and scope
4. Tasks for execution status and ownership
5. Changelog for historical summaries
6. Memory for durable discoveries, not authoritative requirements

Never silently resolve a conflict. Report it and recommend the synchronization change.

## Changelog and Memory

When `.apos/changelog/` is maintained, record material completed changes under `Added`, `Changed`, `Fixed`, or `Removed`. Avoid noise for formatting-only edits unless required.

Store durable, reusable knowledge: technical discoveries, known issues and root causes, architectural lessons, repeated failures and fixes, and project-specific patterns. Do not store temporary conversation chatter or unrelated information. Prefer concise entries with context, date, and links to the relevant task or decision.

## Optional Registries

Maintain `.apos/worktrees/` only when multiple worktrees are active or lifecycle affects coordination. Record name, branch, owner, purpose, status, and creation date; check it before creating, merging, or deleting a worktree.

Maintain `.apos/agents/` only when multiple autonomous agents operate concurrently. Record responsibilities, allowed areas, restricted areas, and ownership boundaries. Prevent overlapping edits to critical files.

## Exceptions and Emergency Path

A normally required artifact may be skipped when the change is trivial, time-critical, or the artifact is not materially affected. State the reason in the final report.

For an emergency fix: restore safety or service first; validate the immediate fix; create or update the task afterward; record the incident, root cause, and follow-up work; and synchronize affected architecture or requirements documentation if needed.

## Health Audit

Run an audit when requested, before a major release, after prolonged inactivity, or when drift is suspected. Inspect dead or unowned tasks, missing requirements for significant features, stale worktrees, drifted architecture docs, conflicting decisions, incomplete changelogs, and known issues without follow-up.

Report by severity rather than inventing a numeric score:

- **Critical:** data loss, security, release, or severe integrity risk
- **High:** important architecture, ownership, or requirement mismatch
- **Medium:** meaningful maintenance or synchronization gap
- **Low:** housekeeping or stale metadata

Include critical issues, warnings, recommendations, and suggested owners or next actions.

## Definition of Done

A change is complete when intended behavior is implemented, relevant validation passes, no known critical regression remains, affected artifacts are synchronized, task status and ownership are accurate when a task exists, and the final report states what changed, what was validated, and any follow-up work.

## Final Report

Adapt this concise format to the change:

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

For trivial work, omit empty sections. For significant or critical work, include impact analysis, migration or rollback notes, and unresolved conflicts.

## Principles

1. Protect project intent and implementation reality.
2. Use the smallest safe process.
3. Make material changes traceable.
4. Preserve important decisions.
5. Keep ownership explicit.
6. Treat architecture changes as risk-bearing.
7. Make durable knowledge survive sessions.
8. Avoid duplicating documentation.
9. Prefer consistency without sacrificing proportionality.
10. Leave the project understandable to a new developer.
