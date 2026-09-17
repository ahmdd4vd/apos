# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/b/ahmdd4vd/apos)

[![Watch the APOS overview video](./docs/images/apos-overview-thumbnail.png)](https://youtu.be/JedSiPIMITA)

**APOS (AI Project Operating System)** — это навык управления проектами, который помогает coding agent сохранять непрерывность разработки программного обеспечения. APOS поддерживает согласованность контекста проекта, требований, архитектуры, технических решений, владельцев задач, changelog и долгосрочных знаний между сессиями.

> APOS — это слой рекомендаций и синхронизации. Он не заменяет coding agent и не выполняет необратимые действия без соответствующих полномочий.

## Основные возможности

- Проверяет состояние репозитория перед нетривиальной работой.
- Классифицирует изменения по риску: trivial, routine, significant и critical.
- Связывает goals, requirements, tasks, architecture, decisions, changelog и memory.
- Использует пропорциональный процесс: `READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`.
- Помогает предотвращать расхождение архитектуры и документации с проверенной реализацией.
- Поддерживает аудит состояния проекта и отчёты о drift по уровням серьёзности.
- Предоставляет краткий и практичный формат итогового отчёта.

## Установка через skills.sh

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skills/apos
```

Команда устанавливает навык для текущего проекта. Для глобальной установки:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skills/apos -g
```

Чтобы выбрать конкретного агента, используйте `--agent`, например для Claude Code:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skills/apos --agent claude-code
```

Просмотр доступных навыков и неинтерактивная установка:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skills/apos --list
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skills/apos -y
```

Полная информация доступна в [документации skills.sh](https://www.skills.sh/docs).

## Руководство по использованию

### Когда использовать APOS

Используйте APOS для новых функций, исправлений, затрагивающих несколько компонентов, изменений API, базы данных или аутентификации, архитектурных изменений, координации нескольких агентов или worktree, аудита drift, а также крупных релизов и миграций. Для исправления опечаток и простого форматирования применяется облегчённый процесс.

### Начало задачи

Явно попросите агента использовать APOS:

```text
Use APOS for this task. Inspect the repository state first, classify the change, identify relevant tasks and architecture decisions, implement the smallest safe change, validate it, update affected artifacts, and provide an APOS Report.
```

Для новой функции попросите сначала изучить goals, requirements, architecture и decisions, затем создать или обновить задачу.

### Процесс по уровню риска

| Тип | Примеры | Процесс |
| --- | --- | --- |
| **Trivial** | Опечатки, форматирование, небольшие изменения документации | Read, execute, validate, report |
| **Routine** | Исправления ошибок, тесты, небольшие функции, локальный рефакторинг | Read, analyze, plan, execute, validate, update state, report |
| **Significant** | API, база данных, аутентификация, общие компоненты | Read, analyze, impact check, execute, validate, update state, report |
| **Critical** | Breaking changes, разрушительные миграции, security, permissions | Полный impact analysis, риски и rollback, authorization, выполнение и проверка |

### Состояние проекта и документация

Если `.apos/` уже существует, прочитайте относящиеся к задаче файлы. Если его нет, создавайте только необходимые каталоги:

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

Запись задачи обычно должна содержать стабильный ID, название, владельца, статус, приоритет, зависимости, ссылки на requirements или decisions и критерии проверки. Обновляйте только существенно затронутые артефакты.

### Проверка, аудит и отчёт

Проверка должна соответствовать риску: tests, type checking, linting, build, migration check, security check или smoke test. Аудит должен искать устаревшие или бесхозные задачи, отсутствующие требования, конфликтующие решения, архитектурный drift, неполный changelog, устаревшие projections и issues без follow-up.

Используйте уровни **Critical**, **High**, **Medium** и **Low**, а не непрозрачный числовой score. Итоговый отчёт:

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

## Принципы

1. Защищайте намерение проекта и реальность реализации.
2. Используйте минимальный безопасный процесс.
3. Делайте существенные изменения отслеживаемыми.
4. Сохраняйте важные решения.
5. Явно фиксируйте владельцев и границы ответственности.
6. Считайте архитектурные изменения рискованными.
7. Сохраняйте важные знания между сессиями.
8. Избегайте дублирования источников истины.
9. Поддерживайте согласованность без лишней бюрократии.
10. Оставляйте проект понятным для нового разработчика.

## Интеграция инструкций агента

Установка навыка APOS сама по себе не изменяет проект. Чтобы агенты не забывали использовать APOS, добавьте его в `AGENTS.md` и/или `CLAUDE.md`.

Полный процесс остаётся в [`skills/apos/SKILL.md`](./skills/apos/SKILL.md). В файлы инструкций добавьте краткое обязательное напоминание о проверке `.apos/`, классификации задачи, валидации, Finish Protocol, синхронизации состояния и APOS Report. Не копируйте весь skill и сохраните существующие инструкции.

Установка skill и принятие APOS в проекте — разные действия: сначала выполните `npx skills add`, затем добавьте или обновите `AGENTS.md`, `CLAUDE.md` и минимальное состояние `.apos/`. Для пустого проекта сделайте это после определения намерения проекта; для существующего сначала сохраните и проверьте текущие инструкции.

## Video overview

Watch the [APOS overview video](https://youtu.be/JedSiPIMITA) to see the repository detection, Bootstrap Protocol, Start Protocol, Finish Protocol, and agent integration flow.



## Файлы

- [`skills/apos/SKILL.md`](./skills/apos/SKILL.md) — основные инструкции для coding agent.
- [`README.md`](./README.md) — документация на английском языке.

## Статус и лицензия

APOS находится на этапе **Beta / governance skill**. API, console, CLI, MCP adapter и database планируются отдельно. Лицензия репозитория пока не указана.
