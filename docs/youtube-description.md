# APOS — YouTube Video Description

## Title

APOS — AI Project Operating System for AI-Assisted Software Development

## Description

Watch the video: https://youtu.be/JedSiPIMITA

AI coding agents can write code quickly—but without project continuity, context gets lost, technical decisions drift, tasks become unclear, and documentation falls behind.

**APOS (AI Project Operating System)** adds a lightweight governance and continuity layer to AI-assisted software projects. It helps agents understand the repository state, preserve project intent, track technical decisions, synchronize documentation, and leave behind durable project knowledge.

APOS is designed for both **empty projects** and **existing projects**.

For an empty project, APOS helps the agent clarify intent, create minimum project state, define a bootstrap task, and choose the first implementation slice without inventing requirements.

For an existing project, APOS inspects the repository, records observed facts and unknowns, creates an adoption task, and establishes project state based on evidence rather than assumptions.

The core APOS lifecycle is:

```text
Repository Detection
        ↓
Bootstrap / Adoption
        ↓
Start Protocol
        ↓
Implementation
        ↓
Validation
        ↓
Finish Protocol
        ↓
State Synchronization
        ↓
APOS Report
```

The **Finish Protocol** is especially important: code being complete does not automatically mean the task is complete. Before reporting a non-trivial task as finished, the agent validates the implementation, updates the task, synchronizes affected decisions and documentation, records durable knowledge, and produces an APOS Report.

## What APOS provides

- Repository detection for empty, existing, and partially initialized projects.
- Empty Project Bootstrap Protocol.
- Existing Project Adoption Protocol.
- Start Protocol for planning work safely.
- Finish Protocol for synchronizing project state before completion.
- Risk-based change classification.
- Task ownership and validation criteria.
- Architecture and technical decision control.
- Changelog and durable-memory synchronization.
- Health audits and severity-based drift reporting.
- Integration guidance for `AGENTS.md` and `CLAUDE.md`.
- A portable `.apos/` project-state structure.

## Video chapters

00:00 — APOS introduction
00:07 — The project continuity problem
00:14 — APOS governance layer
00:21 — Empty project vs. existing project
00:28 — Bootstrap and Start Protocol
00:35 — Finish Protocol
00:42 — Agent integration with `AGENTS.md` and `CLAUDE.md`
00:49 — The APOS operating loop

## Repository

GitHub: https://github.com/ahmdd4vd/apos

## Install through skills.sh

Install APOS directly into your agent workflow:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos
```

Install globally:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -g
```

Install for a specific agent:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --agent claude-code
```

## Documentation

- Main README: https://github.com/ahmdd4vd/apos#readme
- APOS skill instructions: https://github.com/ahmdd4vd/apos/blob/main/skill/apos/SKILL.md
- Project state structure: https://github.com/ahmdd4vd/apos#supported-project-structure
- Agent integration guide: https://github.com/ahmdd4vd/apos#agent-instruction-integration
- APOS installation page on skills.sh: https://skills.sh/ahmdd4vd/apos

## Multilingual documentation

- Indonesian: https://github.com/ahmdd4vd/apos/blob/main/README.id.md
- Russian: https://github.com/ahmdd4vd/apos/blob/main/README.ru.md
- Simplified Chinese: https://github.com/ahmdd4vd/apos/blob/main/README.zh-CN.md
- Spanish: https://github.com/ahmdd4vd/apos/blob/main/README.es.md
- Brazilian Portuguese: https://github.com/ahmdd4vd/apos/blob/main/README.pt-BR.md
- German: https://github.com/ahmdd4vd/apos/blob/main/README.de.md
- French: https://github.com/ahmdd4vd/apos/blob/main/README.fr.md

## Agent integration

To make agents consistently remember APOS, add a short APOS governance section to `AGENTS.md` and/or `CLAUDE.md` in the project. Keep the complete reusable workflow in:

```text
skill/apos/SKILL.md
```

Keep project-specific state in:

```text
.apos/
```

Installing the skill and adopting APOS into a project are separate actions. Install the skill through `skills.sh`, then integrate the project instruction files and minimum `.apos/` state when the project adopts APOS governance.

## Contributing

Issues, improvements, documentation updates, and workflow feedback are welcome in the GitHub repository.

## License

A license has not yet been specified for the repository.

#APOS #AICodingAgents #SoftwareDevelopment #DeveloperTools #ProjectGovernance #OpenSource
