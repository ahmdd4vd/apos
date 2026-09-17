# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/b/ahmdd4vd/apos)

[![Watch the APOS overview video](./docs/images/apos-overview-thumbnail.png)](https://youtu.be/JedSiPIMITA)

**APOS (AI Project Operating System)** ist ein Skill für die Projekt-Governance, der Coding Agents dabei unterstützt, die Kontinuität von Softwareprojekten zu bewahren. APOS hält Projektkontext, Anforderungen, Architektur, technische Entscheidungen, Task-Verantwortung, Changelog und dauerhaftes Wissen über mehrere Sitzungen hinweg konsistent.

> APOS ist eine Ebene für Anleitung und Synchronisierung. Es ersetzt den Coding Agent nicht und führt ohne entsprechende Berechtigung keine irreversiblen Aktionen aus.

## Hauptfunktionen

- Prüft den tatsächlichen Repository-Zustand vor nicht trivialen Arbeiten.
- Klassifiziert Änderungen nach Risiko: trivial, routine, significant oder critical.
- Verknüpft goals, requirements, tasks, architecture, decisions, changelog und memory.
- Verwendet einen verhältnismäßigen Ablauf: `READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`.
- Hilft zu verhindern, dass Architektur und Dokumentation von der validierten Implementierung abweichen.
- Unterstützt Projektgesundheitsprüfungen und Drift-Berichte nach Schweregrad.
- Bietet ein kurzes und umsetzbares Format für Abschlussberichte.

## Installation über skills.sh

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos
```

Damit wird der Skill im aktuellen Projekt installiert. Für eine globale Installation:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -g
```

Um einen bestimmten Agent auszuwählen, verwenden Sie `--agent`, zum Beispiel für Claude Code:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --agent claude-code
```

Verfügbare Skills anzeigen oder nicht interaktiv installieren:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --list
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -y
```

Die vollständige CLI-Referenz finden Sie in der [skills.sh-Dokumentation](https://www.skills.sh/docs).

## Verwendung

### Wann APOS verwendet werden sollte

Verwenden Sie APOS für neue Funktionen, Fehlerbehebungen über mehrere Komponenten hinweg, Änderungen an API, Datenbank oder Authentifizierung, Architekturänderungen, die Koordination mehrerer Agents oder Worktrees, Drift-Audits sowie wichtige Releases oder Migrationen. Für einfache Text- und Formatierungsänderungen genügt ein leichter Ablauf.

### Eine Aufgabe starten

Fordern Sie den Agent ausdrücklich auf, APOS zu verwenden:

```text
Use APOS for this task. Inspect the repository state first, classify the change, identify relevant tasks and architecture decisions, implement the smallest safe change, validate it, update affected artifacts, and provide an APOS Report.
```

Bei einer neuen Funktion sollte der Agent zuerst goals, requirements, architecture und decisions prüfen, bevor er den Task erstellt oder aktualisiert.

### Ablauf nach Risikostufe

| Typ | Beispiele | Ablauf |
| --- | --- | --- |
| **Trivial** | Text, Formatierung, isolierte Dokumentation | Read, execute, validate, report |
| **Routine** | Fehlerbehebungen, Tests, kleine Features, lokales Refactoring | Read, analyze, plan, execute, validate, update state, report |
| **Significant** | API, Datenbank, Authentifizierung, gemeinsam genutzte Komponenten | Read, analyze, impact check, execute, validate, update state, report |
| **Critical** | Breaking Changes, destruktive Migrationen, Sicherheit, Berechtigungen | Vollständige Auswirkungsanalyse, Risiken und Rollback, Autorisierung, Ausführung und Validierung |

### Projektstatus und Dokumentation

Wenn `.apos/` bereits vorhanden ist, lesen Sie vor Beginn die relevanten Dateien. Falls nicht, erstellen Sie nur die benötigten Verzeichnisse:

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

Ein Task sollte normalerweise eine stabile ID, einen Titel, einen Verantwortlichen, Status, Priorität, Abhängigkeiten, Verweise auf requirements oder decisions sowie Validierungskriterien enthalten. Aktualisieren Sie nur materiell betroffene Artefakte.

### Validierung, Audit und Bericht

Die Validierung muss dem Risiko entsprechen: tests, type checking, linting, build, migration check, security check oder smoke test. Ein Audit sollte veraltete oder nicht zugewiesene Tasks, fehlende requirements, widersprüchliche decisions, architecture drift, unvollständige changelogs, veraltete projections und issues ohne Folgeaufgaben prüfen.

Verwenden Sie **Critical**, **High**, **Medium** und **Low** statt einer undurchsichtigen numerischen Bewertung. Berichtsvorlage:

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

## Designprinzipien

1. Schützen Sie die Projektabsicht und die Realität der Implementierung.
2. Verwenden Sie den kleinsten weiterhin sicheren Prozess.
3. Machen Sie materielle Änderungen nachvollziehbar.
4. Bewahren Sie wichtige Entscheidungen.
5. Machen Sie Verantwortliche und Zuständigkeitsgrenzen explizit.
6. Behandeln Sie Architekturänderungen als risikobehaftete Änderungen.
7. Lassen Sie wichtiges Wissen sitzungsübergreifend bestehen.
8. Vermeiden Sie doppelte Quellen der Wahrheit.
9. Bevorzugen Sie Konsistenz ohne unnötige Bürokratie.
10. Hinterlassen Sie ein für neue Entwickler verständliches Projekt.

## Integration der Agent-Anweisungen

Die Installation des APOS-Skills ändert das Projekt nicht automatisch. Damit Agents APOS zuverlässig beachten, integrieren Sie APOS in `AGENTS.md` und/oder `CLAUDE.md`.

Der vollständige Ablauf bleibt in [`skill/apos/SKILL.md`](./skill/apos/SKILL.md). Ergänzen Sie in den Anweisungsdateien nur eine kurze verpflichtende Erinnerung: `.apos/` prüfen, die Aufgabe klassifizieren, validieren, das Finish Protocol ausführen, den Zustand synchronisieren und einen APOS Report erstellen. Kopieren Sie nicht den gesamten Skill und bewahren Sie bestehende Anweisungen.

Die Skill-Installation und die Einführung von APOS im Projekt sind getrennte Schritte: zuerst `npx skills add` ausführen, danach `AGENTS.md`, `CLAUDE.md` und den minimalen `.apos/`-Zustand hinzufügen oder aktualisieren. Klären Sie bei einem leeren Projekt zuerst die Projektabsicht; prüfen und bewahren Sie bei einem bestehenden Projekt zuerst die vorhandenen Anweisungen.

## Video overview

Watch the [APOS overview video](https://youtu.be/JedSiPIMITA) to see the repository detection, Bootstrap Protocol, Start Protocol, Finish Protocol, and agent integration flow.



## Dateien

- [`skill/apos/SKILL.md`](./skill/apos/SKILL.md) — zentrale Anweisungen für den Coding Agent.
- [`README.md`](./README.md) — englische Dokumentation.

## Status und Lizenz

APOS befindet sich derzeit in der Phase **Beta / governance skill**. API, Console, CLI, MCP Adapter und Datenbank werden separat geplant. Für dieses Repository wurde noch keine Lizenz festgelegt.
