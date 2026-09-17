# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/b/ahmdd4vd/apos)

**APOS (AI Project Operating System)** es una skill de gobernanza de proyectos que ayuda a los coding agents a mantener la continuidad de los proyectos de software. APOS mantiene alineados entre sesiones el contexto del proyecto, los requisitos, la arquitectura, las decisiones técnicas, los responsables de las tareas, el changelog y el conocimiento duradero.

> APOS es una capa de orientación y sincronización. No reemplaza al coding agent ni ejecuta acciones irreversibles sin la autorización correspondiente.

## Funciones principales

- Inspecciona el estado del repositorio antes del trabajo no trivial.
- Clasifica los cambios por riesgo: trivial, routine, significant o critical.
- Conecta goals, requirements, tasks, architecture, decisions, changelog y memory.
- Aplica un flujo proporcional: `READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`.
- Ayuda a evitar que la arquitectura y la documentación se separen de la implementación validada.
- Admite auditorías de salud del proyecto e informes de drift por severidad.
- Proporciona un formato de informe final breve y accionable.

## Instalación mediante skills.sh

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos
```

El comando instala la skill para el proyecto actual. Para instalarla globalmente:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -g
```

Para seleccionar un agente concreto, usa `--agent`, por ejemplo para Claude Code:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --agent claude-code
```

Para listar skills o instalar sin interacción:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --list
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -y
```

Consulta la [documentación de skills.sh](https://www.skills.sh/docs) para la referencia completa.

## Guía de uso

### Cuándo usar APOS

Usa APOS para desarrollar funcionalidades, corregir bugs que afecten a varios componentes, cambiar API, base de datos o autenticación, modificar la arquitectura, coordinar varios agentes o worktrees, auditar drift y preparar releases o migraciones importantes. Para cambios de formato o correcciones de texto simples se utiliza un flujo ligero.

### Iniciar una tarea

Pide explícitamente al agente que use APOS:

```text
Use APOS for this task. Inspect the repository state first, classify the change, identify relevant tasks and architecture decisions, implement the smallest safe change, validate it, update affected artifacts, and provide an APOS Report.
```

Para una nueva funcionalidad, pide que revise primero goals, requirements, architecture y decisions antes de crear o actualizar la task.

### Flujo según el riesgo

| Tipo | Ejemplos | Flujo |
| --- | --- | --- |
| **Trivial** | Textos, formato, documentación aislada | Read, execute, validate, report |
| **Routine** | Bugs, tests, funcionalidades pequeñas, refactor local | Read, analyze, plan, execute, validate, update state, report |
| **Significant** | API, base de datos, autenticación, componentes compartidos | Read, analyze, impact check, execute, validate, update state, report |
| **Critical** | Breaking changes, migraciones destructivas, seguridad, permisos | Análisis completo de impacto, riesgos y rollback, autorización, ejecución y validación |

### Estado del proyecto y documentación

Si `.apos/` ya existe, lee los archivos relevantes antes de trabajar. Si no existe, crea solo los directorios necesarios:

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

Una task normalmente debe incluir ID estable, título, responsable, estado, prioridad, dependencias, referencias a requirements o decisions y criterios de validación. Actualiza solo los artifacts afectados de forma material.

### Validación, auditoría e informe

La validación debe corresponder al riesgo: tests, type checking, linting, build, migration check, security check o smoke test. Una auditoría debe buscar tasks obsoletas o sin responsable, requirements ausentes, decisions en conflicto, architecture drift, changelog incompleto, projections obsoletas e issues sin seguimiento.

Usa **Critical**, **High**, **Medium** y **Low** en lugar de una puntuación numérica opaca. Formato del informe:

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

## Principios de diseño

1. Protege la intención del proyecto y la realidad de la implementación.
2. Usa el proceso mínimo que siga siendo seguro.
3. Haz trazables los cambios materiales.
4. Conserva las decisiones importantes.
5. Explicita responsables y límites de responsabilidad.
6. Trata los cambios de arquitectura como cambios con riesgo.
7. Conserva el conocimiento importante entre sesiones.
8. Evita duplicar las fuentes de verdad.
9. Prefiere la coherencia sin burocracia innecesaria.
10. Deja el proyecto comprensible para un nuevo desarrollador.

## Integración de instrucciones del agente

Instalar la skill de APOS no modifica automáticamente el proyecto. Para que los agentes recuerden APOS de forma consistente, intégralo en `AGENTS.md` y/o `CLAUDE.md`.

El flujo completo permanece en [`skill/apos/SKILL.md`](./skill/apos/SKILL.md). En los archivos de instrucciones añade un recordatorio breve y obligatorio para revisar `.apos/`, clasificar la tarea, validar, ejecutar el Finish Protocol, sincronizar el estado y entregar un APOS Report. No copies toda la skill y conserva las instrucciones existentes.

Instalar la skill y adoptar APOS en el proyecto son acciones distintas: primero ejecuta `npx skills add` y después añade o actualiza `AGENTS.md`, `CLAUDE.md` y el estado mínimo de `.apos/`. En un proyecto vacío, hazlo después de aclarar la intención; en uno existente, revisa y conserva primero las instrucciones actuales.

## Vídeo general

Mira el [vídeo general de APOS](./docs/videos/apos-overview.mp4) para conocer el flujo de repository detection, Bootstrap Protocol, Start Protocol, Finish Protocol e integración del agente.

[![Ver el vídeo general de APOS](./docs/images/apos-overview-thumbnail.png)](./docs/videos/apos-overview.mp4)

## Archivos

- [`skill/apos/SKILL.md`](./skill/apos/SKILL.md) — instrucciones principales para el coding agent.
- [`README.md`](./README.md) — documentación en inglés.

## Estado y licencia

APOS está actualmente en la etapa **Beta / governance skill**. La API, consola, CLI, MCP adapter y base de datos se planifican por separado. Este repositorio todavía no tiene una licencia especificada.
