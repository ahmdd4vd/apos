# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/b/ahmdd4vd/apos)

[![Watch the APOS overview video](./docs/images/apos-overview-thumbnail.png)](https://youtu.be/JedSiPIMITA)

**APOS (AI Project Operating System)** est un skill de gouvernance de projet qui aide les coding agents à préserver la continuité des projets logiciels. APOS maintient la cohérence du contexte du projet, des exigences, de l’architecture, des décisions techniques, des responsables de tâches, du changelog et des connaissances durables entre les sessions.

> APOS est une couche d’orientation et de synchronisation. Il ne remplace pas le coding agent et n’effectue pas d’actions irréversibles sans l’autorisation appropriée.

## Fonctionnalités principales

- Inspecte l’état réel du dépôt avant tout travail non trivial.
- Classe les changements selon leur risque : trivial, routine, significant ou critical.
- Relie les goals, requirements, tasks, architecture, decisions, changelog et memory.
- Applique un processus proportionné : `READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`.
- Aide à éviter que l’architecture et la documentation ne divergent de l’implémentation validée.
- Prend en charge les audits de santé du projet et les rapports de drift par niveau de gravité.
- Fournit un format de rapport final court et exploitable.

## Installation via skills.sh

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos
```

Cette commande installe le skill dans le projet actuel. Pour l’installer globalement :

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -g
```

Pour cibler un agent particulier, utilisez `--agent`, par exemple pour Claude Code :

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --agent claude-code
```

Pour afficher les skills disponibles ou installer sans interaction :

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --list
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -y
```

Consultez la [documentation de skills.sh](https://www.skills.sh/docs) pour la référence complète de la CLI.

## Guide d’utilisation

### Quand utiliser APOS

Utilisez APOS pour les nouvelles fonctionnalités, les corrections qui touchent plusieurs composants, les changements d’API, de base de données ou d’authentification, les modifications d’architecture, la coordination de plusieurs agents ou worktrees, les audits de drift ainsi que les releases ou migrations importantes. Pour les corrections de texte et de formatage simples, un processus léger suffit.

### Démarrer une tâche

Demandez explicitement à l’agent d’utiliser APOS :

```text
Use APOS for this task. Inspect the repository state first, classify the change, identify relevant tasks and architecture decisions, implement the smallest safe change, validate it, update affected artifacts, and provide an APOS Report.
```

Pour une nouvelle fonctionnalité, demandez à l’agent de consulter d’abord les goals, requirements, architecture et decisions avant de créer ou de mettre à jour la task.

### Processus selon le niveau de risque

| Type | Exemples | Processus |
| --- | --- | --- |
| **Trivial** | Texte, formatage, documentation isolée | Read, execute, validate, report |
| **Routine** | Corrections, tests, petites fonctionnalités, refactorisation locale | Read, analyze, plan, execute, validate, update state, report |
| **Significant** | API, base de données, authentification, composants partagés | Read, analyze, impact check, execute, validate, update state, report |
| **Critical** | Breaking changes, migrations destructives, sécurité, permissions | Analyse d’impact complète, risques et rollback, autorisation, exécution et validation |

### État du projet et documentation

Si `.apos/` existe déjà, lisez les fichiers pertinents avant de commencer. Sinon, créez uniquement les répertoires nécessaires :

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

Une task doit généralement contenir un ID stable, un titre, un responsable, un statut, une priorité, des dépendances, des références vers les requirements ou decisions et des critères de validation. Ne mettez à jour que les artifacts réellement concernés.

### Validation, audit et rapport

La validation doit être proportionnelle au risque : tests, type checking, linting, build, migration check, security check ou smoke test. Un audit doit rechercher les tasks obsolètes ou sans responsable, les requirements manquants, les decisions contradictoires, l’architecture drift, les changelogs incomplets, les projections obsolètes et les issues sans suivi.

Utilisez **Critical**, **High**, **Medium** et **Low** plutôt qu’un score numérique opaque. Format du rapport :

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

## Principes de conception

1. Protégez l’intention du projet et la réalité de l’implémentation.
2. Utilisez le plus petit processus qui reste sûr.
3. Rendez les changements importants traçables.
4. Préservez les décisions importantes.
5. Rendez explicites les responsables et les limites de responsabilité.
6. Traitez les changements d’architecture comme des changements à risque.
7. Faites survivre les connaissances importantes entre les sessions.
8. Évitez de dupliquer les sources de vérité.
9. Privilégiez la cohérence sans bureaucratie inutile.
10. Laissez un projet compréhensible pour un nouveau développeur.

## Intégration des instructions de l’agent

L’installation du skill APOS ne modifie pas automatiquement le projet. Pour que les agents se souviennent systématiquement d’APOS, intégrez-le dans `AGENTS.md` et/ou `CLAUDE.md`.

Le workflow complet reste dans [`skill/apos/SKILL.md`](./skill/apos/SKILL.md). Dans les fichiers d’instructions, ajoutez seulement un rappel obligatoire et concis : vérifier `.apos/`, classer la tâche, valider, exécuter le Finish Protocol, synchroniser l’état et produire un APOS Report. Ne copiez pas tout le skill et conservez les instructions existantes.

L’installation du skill et l’adoption d’APOS dans le projet sont deux actions différentes : exécutez d’abord `npx skills add`, puis ajoutez ou mettez à jour `AGENTS.md`, `CLAUDE.md` et l’état `.apos/` minimal. Pour un projet vide, clarifiez d’abord son intention ; pour un projet existant, examinez et préservez d’abord les instructions présentes.

## Video overview

Watch the [APOS overview video](https://youtu.be/JedSiPIMITA) to see the repository detection, Bootstrap Protocol, Start Protocol, Finish Protocol, and agent integration flow.



## Fichiers

- [`skill/apos/SKILL.md`](./skill/apos/SKILL.md) — instructions principales pour le coding agent.
- [`README.md`](./README.md) — documentation en anglais.

## Statut et licence

APOS est actuellement au stade **Beta / governance skill**. L’API, la console, la CLI, le MCP adapter et la base de données sont planifiés séparément. Aucune licence n’a encore été définie pour ce dépôt.
