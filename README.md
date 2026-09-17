# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/ahmdd4vd/apos)

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
npx skills add ahmdd4vd/apos --skill apos
```

The command installs the skill for the current project. To install it globally so it is available across projects, add the `-g` flag:

```bash
npx skills add ahmdd4vd/apos --skill apos -g
```

To target a specific agent, use the `--agent` option. For example, for Claude Code:

```bash
npx skills add ahmdd4vd/apos --skill apos --agent claude-code
```

To list the skills available in this repository without installing them:

```bash
npx skills add ahmdd4vd/apos --list
```

For non-interactive installation:

```bash
npx skills add ahmdd4vd/apos --skill apos -y
```

See the [skills.sh documentation](https://www.skills.sh/docs) for the complete CLI reference.

## Usage

Once installed, an agent can load APOS when a task involves project governance and continuity, such as:

- planning or implementing software changes;
- synchronizing documentation with validated code;
- tracking tasks, ownership, and technical decisions;
- auditing project drift; or
- coordinating multiple agents or worktrees.

For manual use, mention APOS or ask the agent to follow the APOS workflow in your prompt. The skill guides the agent to inspect the actual project state, choose a risk-appropriate process, validate the result, update affected artifacts, and produce a final report.

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

## Files

- [`SKILL.md`](./SKILL.md) — the main instructions loaded by coding agents.

## Status

APOS is currently in the **Beta / governance skill** stage. This repository contains the skill that can be installed through skills.sh. The APOS API, console, CLI, MCP adapter, and database implementation are planned separately.

## Contributing

Before changing `SKILL.md`, keep the instructions concise, broadly applicable across repositories, and free of unnecessary documentation duplication. For material workflow changes, document the relevant technical decision and update the project documentation as needed.

## License

A license has not yet been specified for this repository.
