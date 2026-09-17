---
name: apos
description: Project governance and continuity for software projects. Use when starting or adopting empty or existing projects, planning or implementing changes, synchronizing architecture and documentation, tracking tasks and decisions, auditing project drift, or coordinating multiple agents and worktrees.
---

# APOS Beta

APOS (AI Project Operating System) is a project-governance and continuity layer for software projects. It helps implementation agents preserve project intent, architecture consistency, decision history, task ownership, documentation accuracy, and durable project knowledge.

APOS does not replace implementation agents. It guides them and keeps project state synchronized with validated reality.

## Objectives

Use APOS to reduce documentation drift, architecture drift, context loss, task and ownership ambiguity, forgotten technical decisions, worktree conflicts, and knowledge fragmentation. Favor project integrity without introducing process heavier than the change requires.

## Repository Detection

Before any non-trivial work, detect the repository state and choose the appropriate mode. Do not treat an empty repository as evidence that the project has no intent, and do not invent product requirements to fill missing context.

Classify the repository as one of these modes:

| Mode | Detection | Required behavior |
|---|---|---|
| **Empty project** | Only `.git/`, or no source, manifest, tests, or project documentation | Run the Empty Project Bootstrap Protocol. Capture unknown intent explicitly and ask only the questions needed to proceed. |
| **Existing without APOS** | Source code, manifests, tests, or documentation exist, but `.apos/` does not | Run the Existing Project Adoption Protocol. Observe first; do not overwrite or generate a complete undocumented project model. |
| **Existing with APOS** | `.apos/` exists alongside project files | Read relevant APOS artifacts and follow the project’s established conventions. |
| **Partially initialized** | Some `.apos/` artifacts exist, but state is incomplete | Preserve existing files, identify gaps, and create only the minimum artifacts needed for the current task. |

Inspect the actual repository state, including source layout, package manifests, configuration, tests, entry points, and documentation. Mark unknown facts as `Unknown`; never present an inference as an accepted decision.

## Bootstrap Protocol

Run this protocol before implementation when the repository is empty, when adopting an existing project, or when `.apos/` is missing for a non-trivial task.

### Empty Project Bootstrap

When the repository is empty:

1. Confirm whether the user has provided enough product intent to begin. If not, ask about the project type, problem, target users, constraints, and first implementation slice; do not invent answers.
2. Create only minimum viable project state once APOS is requested or the project intent is sufficiently clear.
3. Create a goal and bootstrap task. Add requirements or architecture decisions only when the user has supplied enough information to justify them.
4. Record unknowns explicitly and keep them separate from accepted requirements.
5. Choose the first implementation slice and its validation criteria before writing substantial code.

A minimal starting structure is:

```text
.apos/
├── goals/
│   └── project.md
├── tasks/
│   └── bootstrap-project.md
└── memory/
    └── project-context.md
```

Create `prd/`, `architecture/`, `decisions/`, or `changelog/` only when the project needs them. Do not create empty bureaucracy.

A new project goal may state:

```markdown
# Project Goal

## Problem
Not defined yet.

## Desired outcome
Not defined yet.

## Scope
Not defined yet.

## Notes
Project initialized with APOS. Product intent is pending clarification.
```

The bootstrap task should include an owner, status, priority, and acceptance criteria such as defined intent, initial architecture, first implementation slice, and validation plan.

### Existing Project Adoption

When source code already exists but `.apos/` does not:

1. Inspect the repository before creating APOS artifacts.
2. Identify observed frameworks, entry points, modules, tests, deployment configuration, and existing documentation.
3. Create an adoption task, for example `Adopt APOS into existing project and establish initial project state`.
4. Create a concise initial-state note from observed facts only.
5. Record unknown ownership, deployment, migration, or architecture policies as `Unknown`.
6. Add architecture or decision documents only when supported by evidence or explicitly accepted by the user.
7. Do not rewrite existing project documentation or create a complete `.apos/` tree merely for appearance.

Distinguish facts from assumptions:

```text
Observed:
- The project uses <framework>.
- The application entry point is <path>.
- Tests are located in <path>.

Unknown:
- Deployment ownership is not documented.
- Database migration policy is not documented.
```

### Bootstrap Completion

Bootstrap is complete when the project mode is recorded, the minimum state exists, intent and unknowns are clear, the first task is owned, and the next implementation or clarification step is actionable. Report any intentionally deferred artifact.

## Project State First

If `.apos/` exists, read relevant files before making assumptions. Typical structure:

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

## Start Protocol

Before changing code for a non-trivial task:

1. Detect repository and APOS mode.
2. Read relevant goals, requirements, architecture, decisions, tasks, and project documentation.
3. Classify the change.
4. Create or update a task for routine, significant, or critical work.
5. Identify affected modules, risks, constraints, and conflicts.
6. Define the proportional validation plan.
7. State the task ID and classification when reporting the plan.

Use the smallest safe workflow:

```text
Trivial:    READ → EXECUTE → VALIDATE → REPORT
Routine:    READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT
Significant: READ → ANALYZE → PLAN → IMPACT CHECK → EXECUTE → VALIDATE → UPDATE STATE → REPORT
Critical:   READ → ANALYZE → PLAN → AUTHORIZATION/RISK CHECK → EXECUTE → VALIDATE → UPDATE STATE → REPORT
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

## Finish Protocol

Before reporting a routine, significant, or critical task as complete, run the Finish Protocol. Do not stop at “the code works” when project state is affected.

1. Run proportional validation and record the exact checks and results.
2. Compare the implementation with the task, goals, requirements, and acceptance criteria.
3. Update the task status, owner, validation result, and follow-up items.
4. Update architecture documentation or create a decision record if technical direction changed.
5. Update the PRD or requirements when user-facing scope or behavior changed.
6. Add a changelog entry for material completed changes when the project maintains a changelog.
7. Save durable discoveries, root causes, or reusable project patterns to memory.
8. Check whether worktree or agent registries require updates.
9. Record intentionally unchanged artifacts and explain why they were not affected.
10. Produce an APOS Report containing the task ID, classification, changes, validation, state updates, risks, and follow-up work.

A non-trivial task is not complete until implementation is validated, the task reflects the actual result, affected APOS artifacts have been checked, and the final report states what changed and what was intentionally left unchanged. If an artifact is not materially affected, do not create noise; state that decision in the report.

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

A change is complete when intended behavior is implemented, relevant validation passes, no known critical regression remains, affected artifacts are synchronized or intentionally left unchanged with a reason, task status and ownership are accurate when a task exists, and the final APOS Report is complete.

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

For trivial work, omit empty sections. For significant or critical work, include impact analysis, migration or rollback notes, authorization status, and unresolved conflicts.

## Agent Instruction Integration Protocol

When APOS is adopted in a project, make the project’s primary agent instructions remind agents to use APOS. Use `AGENTS.md` for agents that support it and `CLAUDE.md` for Claude-based workflows. Keep the full reusable workflow in `skills/apos/SKILL.md`; do not copy the entire skill into either instruction file.

Add a short, clearly separated section to each applicable file:

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

Full workflow: `skills/apos/SKILL.md`
Project state: `.apos/`
```

Preserve existing `AGENTS.md` and `CLAUDE.md` instructions. Inspect them first, append an isolated APOS section, and report any conflict rather than silently overwriting or changing project policy. A project may use one or both files; do not create duplicate or contradictory instructions.

Installing the skill and adopting APOS are separate actions:

```text
Install skill:  npx skills add <skill-url>
Adopt project:  add or update AGENTS.md, CLAUDE.md, and minimum .apos/ state
```

For an empty project, add the agent-instruction integration after the project intent is clear and the minimum `.apos/` state is created. For an existing project, preserve and inspect current instruction files before adding APOS. Do not alter a repository merely because a user installed the skill; integrate APOS only when the user adopts it for that project.

## Skill Package Layout

This skill is distributed from the repository path `skills/apos/SKILL.md`. Keep the skill’s required instructions in that file and keep user-facing documentation in the repository root README files. Do not place README files inside the skill package unless they are required as bundled resources.

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
