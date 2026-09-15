# APOS Implementation Plan

**Project:** APOS — AI Project Operating System  
**Version:** Beta roadmap v0.1  
**Status:** Planning  
**Repository:** `ahmdd4vd/apos`

## 1. Vision

APOS is a project-governance and continuity platform for AI-assisted software development. It keeps project intent, requirements, architecture, decisions, tasks, ownership, and durable knowledge synchronized with validated implementation reality.

The target architecture is:

```text
Web Console
    ↓
APOS API and Governance Engine
    ↓
Structured Database
    ↓
Local CLI / Daemon / MCP Adapter
    ↓
Coding Agent and Repository
    ↓
Portable .apos/ Projection
```

The database is the operational source of truth. The web console is the human control plane. The CLI and MCP adapter are the agent integration layer. Markdown is a Git-friendly, portable projection and backup—not the primary operational store.

## 2. Product Principles

1. Use the smallest safe workflow for each change.
2. Keep governance risk-based rather than bureaucratic.
3. Query only relevant project context; never dump the entire project into an agent prompt.
4. Preserve an audit trail for material state changes.
5. Keep project data portable through repository projections and export.
6. Make conflicts visible; never silently overwrite project knowledge.
7. Start with a modular monolith before introducing distributed complexity.
8. Keep every phase independently usable and demonstrable.

## 3. Scope Boundaries

### In scope

- Project registration and workspace identity
- Goals, requirements, tasks, decisions, architecture notes, memory, and changelog
- Risk-based change classification
- Relevant-context retrieval for agents
- Web dashboard and entity views
- Local CLI and MCP integration
- Git-friendly `.apos/` projection
- Audit trail and drift detection
- Authentication and project-level access control when multi-user support begins

### Out of scope until explicitly planned

- Fully autonomous code changes without agent/user authorization
- Full bidirectional database/Markdown sync in the first release
- Real-time distributed locking between many agents
- Automatic inference of the entire architecture from a repository
- A numeric AI-generated health score without a transparent rubric
- Billing, marketplace, or broad third-party plugin ecosystem

## 4. Delivery Strategy

Build a vertical slice first, then expand. Every phase must produce:

- A runnable or inspectable artifact
- Tests or validation proportional to the risk
- Updated documentation
- A short demo scenario
- A decision on whether to proceed, revise, or stop

Do not begin the next phase when the current phase has unresolved critical defects or an unclear source-of-truth model.

---

## Phase 0 — Foundation and Decisions

**Goal:** Establish the technical baseline and prevent early architectural drift.

### Deliverables

- Repository structure and contribution conventions
- Architecture decision record for database-first ownership
- Initial domain model and entity relationships
- API contract conventions
- Local development and test setup
- Environment and secrets policy
- Basic threat model and data-privacy assumptions
- Seed/demo project definition

### Decisions to lock

- TypeScript monorepo or single application structure
- PostgreSQL versus SQLite-first strategy
- API style: REST, tRPC, or equivalent
- ORM and migration tool
- Web framework and UI component strategy
- MCP implementation boundary
- Projection format and stable entity IDs

### Exit criteria

- A new developer can run the project locally from documented instructions.
- Architecture decisions are recorded with alternatives and consequences.
- The initial schema can represent one complete demo project.
- CI runs formatting, type checking, and a minimal test suite.

---

## Phase 1 — Core APOS Domain and API

**Goal:** Build the database-backed project state engine without a web UI dependency.

### Initial entities

- `projects`
- `goals`
- `requirements`
- `tasks`
- `decisions`
- `architecture_documents` or `architecture_nodes`
- `memories`
- `changelog_entries`
- `project_events`

Every entity should have a stable ID, project scope, timestamps, creator/updater metadata, and an explicit lifecycle status where applicable.

### Deliverables

- Database schema and migrations
- CRUD and search APIs for core entities
- Relationship validation between projects, tasks, requirements, and decisions
- Append-only event recording for material mutations
- Project context endpoint with scoped filters
- Seed data for a realistic demo project
- API tests for authorization boundaries and core workflows

### Exit criteria

- A project, task, requirement, decision, and memory can be created and linked through the API.
- A query can return only context relevant to a requested change.
- Material changes produce auditable project events.
- Invalid cross-project references are rejected.

---

## Phase 2 — Web Console MVP

**Goal:** Make APOS useful to a human without requiring direct database access.

### Deliverables

- Project dashboard
- Task list and board view
- Requirements and goals views
- Decision and architecture views
- Memory and changelog views
- Global project search
- Entity detail pages with relationships and history
- Basic activity timeline
- Empty, loading, error, and permission states

### UX principles

- Show project health signals as concrete findings, not unexplained scores.
- Make relationships and “why does this exist?” context easy to navigate.
- Keep editing forms small and focused.
- Display source, version, timestamps, and last updater.

### Exit criteria

- A user can create a project and manage its core entities from the console.
- A user can navigate from a task to its requirement, decision, and affected architecture.
- Event history is visible for material changes.
- The demo project can be managed without API calls.

---

## Phase 3 — Local CLI and `apos init`

**Goal:** Connect a repository to APOS and make project context available locally.

### Commands

```text
apos init
apos status
apos context [query]
apos task list|create|update
apos decision search|create
apos audit
apos export
apos sync status
```

### `apos init` behavior

1. Detect the repository root.
2. Register or link the project.
3. Create `.apos/manifest.json` with project ID and integration mode.
4. Generate a short agent instruction file only when requested or supported.
5. Create an initial project snapshot.
6. Never overwrite existing project files without reporting the change.

### Deliverables

- Authenticated local CLI
- Repository/project manifest
- Context retrieval command
- Task and decision mutation commands
- Human-readable and JSON output modes
- Safe error messages when the APOS server is unavailable

### Exit criteria

- `apos init` is repeatable and safe.
- An agent or developer can retrieve relevant context in one command.
- CLI mutations appear in the web console and event timeline.
- CLI output is concise by default and machine-readable with `--json`.

---

## Phase 4 — MCP Agent Adapter

**Goal:** Give compatible agents native, query-based access to APOS without injecting the entire project state into prompts.

### Initial MCP tools

- `apos_get_project`
- `apos_get_relevant_context`
- `apos_classify_change`
- `apos_list_tasks`
- `apos_get_decisions`
- `apos_record_decision`
- `apos_update_task`
- `apos_record_validation`
- `apos_run_audit`

### Context policy

The adapter should return a compact context packet containing only relevant goals, requirements, decisions, architecture constraints, active tasks, risks, and required updates. It must support project scoping and should expose why each result was selected.

### Deliverables

- Local or remote MCP server
- Tool schemas and validation
- Project-scoped authentication
- Context relevance query with deterministic filters first
- Optional semantic search only after baseline search is reliable
- Generated integration instructions for supported agent clients

### Exit criteria

- A compatible agent can inspect project context before a change.
- The agent can classify a change and update a task after validation.
- Tool calls are auditable.
- Large unrelated project documents are not returned by default.

---

## Phase 5 — Markdown Projection and Git Integration

**Goal:** Keep APOS portable and reviewable inside the repository without making Markdown the operational source of truth.

### Projection layout

```text
.apos/
├── manifest.json
├── goals/
├── prd/
├── architecture/
├── decisions/
├── tasks/
├── changelog/
└── memory/
```

Generated Markdown should contain stable APOS metadata:

```yaml
---
apos_id: task_123
project_id: project_001
version: 7
managed_by: apos
last_synced_at: 2026-09-15T20:00:00Z
---
```

### Deliverables

- Deterministic database-to-Markdown exporter
- Stable filenames and IDs
- Manifest and projection version
- Export preview and dry-run mode
- Git-friendly formatting
- Projection status showing stale or missing files
- Documented policy: database-first for Beta

### Beta sync policy

Database changes generate projections. Direct Markdown edits are detected and reported as potential drift; they are not silently imported. Two-way conflict resolution is a later phase.

### Exit criteria

- A project can be exported and reviewed as a Git diff.
- Export is repeatable without unnecessary churn.
- Deletions and stale files are reported safely.
- A repository remains understandable when the APOS service is unavailable.

---

## Phase 6 — Audit and Drift Engine

**Goal:** Detect project-state problems and turn them into actionable findings.

### Checks

- Unowned or stale tasks
- Significant features without requirements
- Decisions without affected scope
- Architecture references to missing modules
- Incomplete changelog entries
- Missing or stale projections
- Conflicting status or ownership metadata
- Known issues without follow-up tasks

### Deliverables

- On-demand audit API, CLI command, and web report
- Severity classification: Critical, High, Medium, Low
- Finding lifecycle: Open, Acknowledged, Resolved, Dismissed
- Suggested remediation actions
- Audit event history

### Exit criteria

- Findings are reproducible from project state.
- Each finding points to affected entities.
- The system does not claim code/documentation drift without evidence.
- A user can assign or resolve findings.

---

## Phase 7 — Multi-User, Security, and Collaboration

**Goal:** Safely support teams, multiple projects, and concurrent agents.

### Deliverables

- User authentication
- Organization and project membership
- Role-based permissions
- API key or agent identity management
- Audit log access controls
- Ownership and concurrency rules
- Worktree registry when needed
- Agent registry when multiple autonomous agents are active
- Backup and restore procedure

### Exit criteria

- Users cannot access projects outside their authorization.
- Agent actions are attributable to an identity.
- Sensitive data is not exposed through broad context queries.
- Destructive operations have safeguards and clear audit records.

---

## Phase 8 — Advanced Search, Analytics, and Optional Sync

**Goal:** Improve retrieval and coordination only after the core workflow is proven.

### Candidate deliverables

- Semantic search and embeddings
- Dependency graph visualization
- Decision timeline and project analytics
- Release-readiness views
- Optional Markdown-to-database import
- Explicit two-way conflict resolution
- Webhooks and integrations
- Agent coordination and reservations

### Entry criteria

Begin this phase only when real usage demonstrates that deterministic search, the web console, CLI, MCP, and export cannot meet the need. Measure retrieval quality, latency, conflict frequency, and user adoption before adding complexity.

---

## 5. Cross-Phase Requirements

### Reliability

- Use migrations for schema changes.
- Make mutations idempotent where practical.
- Return actionable errors.
- Keep exports deterministic.
- Add backups before production use.

### Security

- Scope every query by project and authorization.
- Never place secrets in Markdown projections.
- Validate all agent-provided mutations.
- Log material actions without storing unnecessary sensitive content.

### Performance

- Prefer indexed structured queries.
- Paginate lists and timelines.
- Limit context packets by relevance and size.
- Do not add embeddings before measuring baseline search.

### Observability

- Record request IDs and actor identity.
- Track API errors, latency, export failures, and audit failures.
- Provide a diagnostic command for local adapter connectivity.

## 6. Definition of Done for Each Phase

A phase is complete only when:

- Its scope is implemented without hidden manual steps.
- Tests and validation appropriate to the risk pass.
- The demo scenario works from a clean setup.
- Documentation and architecture decisions are updated.
- Known limitations are recorded.
- The phase has a reproducible acceptance checklist.
- The next phase has not been started merely to hide unfinished work.

## 7. First Implementation Slice

Start with the smallest end-to-end slice rather than building every entity at once:

1. Create a project.
2. Create one goal.
3. Create one task linked to the goal.
4. Retrieve task context through an API.
5. Update task status.
6. Record an event.
7. Display the result in a minimal web page.
8. Export the project to `.apos/`.

This slice proves the central APOS loop:

```text
Structured state → relevant context → agent/developer action → validated update → portable projection
```

## 8. Open Questions Before Phase 0 Implementation

- Is APOS intended to be local-first, hosted, or both?
- Is the first target a single developer, a small team, or multiple organizations?
- Which agent clients must be supported first through MCP or generated instructions?
- Should the first database be PostgreSQL, SQLite, or a compatibility layer supporting both?
- Which project files and frameworks must `apos init` recognize initially?
- What information is allowed to leave the repository when using a hosted APOS service?

Resolve these questions in architecture decisions before committing to the corresponding implementation details.

## 9. Immediate Next Actions

1. Resolve the Phase 0 open questions.
2. Create the initial application scaffold and CI.
3. Record the database-first and projection architecture decisions.
4. Implement the first end-to-end slice.
5. Demo it against a real repository before expanding the domain model.
