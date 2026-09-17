# APOS

[![skills.sh](https://skills.sh/b/ahmdd4vd/apos)](https://skills.sh/b/ahmdd4vd/apos)

**APOS (AI Project Operating System)** é uma skill de governança de projetos que ajuda coding agents a manter a continuidade de projetos de software. O APOS mantém alinhados, entre sessões, o contexto do projeto, requisitos, arquitetura, decisões técnicas, responsáveis por tasks, changelog e conhecimento duradouro.

> O APOS é uma camada de orientação e sincronização. Ele não substitui o coding agent nem executa ações irreversíveis sem a autorização adequada.

## Principais recursos

- Inspeciona o estado do repositório antes de trabalhos não triviais.
- Classifica mudanças por risco: trivial, routine, significant ou critical.
- Conecta goals, requirements, tasks, architecture, decisions, changelog e memory.
- Aplica um fluxo proporcional: `READ → ANALYZE → PLAN → EXECUTE → VALIDATE → UPDATE STATE → REPORT`.
- Ajuda a evitar que arquitetura e documentação se afastem da implementação validada.
- Oferece auditorias de saúde do projeto e relatórios de drift por severidade.
- Fornece um formato de relatório final curto e acionável.

## Instalação pelo skills.sh

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos
```

Esse comando instala a skill no projeto atual. Para instalar globalmente:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -g
```

Para escolher um agent específico, use `--agent`, por exemplo para o Claude Code:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --agent claude-code
```

Para listar skills ou instalar sem interação:

```bash
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos --list
npx skills add https://github.com/ahmdd4vd/apos/tree/main/skill/apos -y
```

Consulte a [documentação do skills.sh](https://www.skills.sh/docs) para a referência completa.

## Guia de uso

### Quando usar o APOS

Use o APOS para novas funcionalidades, correções que afetam vários componentes, mudanças em API, banco de dados ou autenticação, alterações arquiteturais, coordenação de vários agents ou worktrees, auditorias de drift e releases ou migrações importantes. Para correções de texto e formatação simples, use um fluxo leve.

### Iniciar uma tarefa

Peça explicitamente ao agent para usar o APOS:

```text
Use APOS for this task. Inspect the repository state first, classify the change, identify relevant tasks and architecture decisions, implement the smallest safe change, validate it, update affected artifacts, and provide an APOS Report.
```

Para uma nova funcionalidade, peça ao agent para revisar goals, requirements, architecture e decisions antes de criar ou atualizar a task.

### Fluxo por nível de risco

| Tipo | Exemplos | Fluxo |
| --- | --- | --- |
| **Trivial** | Texto, formatação, documentação isolada | Read, execute, validate, report |
| **Routine** | Correções, testes, pequenas features, refatoração local | Read, analyze, plan, execute, validate, update state, report |
| **Significant** | API, banco de dados, autenticação, componentes compartilhados | Read, analyze, impact check, execute, validate, update state, report |
| **Critical** | Breaking changes, migrações destrutivas, segurança, permissões | Análise completa de impacto, riscos e rollback, autorização, execução e validação |

### Estado do projeto e documentação

Se `.apos/` já existir, leia os arquivos relevantes antes de trabalhar. Caso não exista, crie apenas os diretórios necessários:

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

Uma task normalmente deve conter ID estável, título, responsável, status, prioridade, dependências, referências a requirements ou decisions e critérios de validação. Atualize somente os artifacts afetados materialmente.

### Validação, auditoria e relatório

A validação deve ser proporcional ao risco: tests, type checking, linting, build, migration check, security check ou smoke test. Uma auditoria deve procurar tasks antigas ou sem responsável, requirements ausentes, decisions conflitantes, architecture drift, changelog incompleto, projections desatualizadas e issues sem acompanhamento.

Use **Critical**, **High**, **Medium** e **Low**, em vez de uma pontuação numérica opaca. Formato do relatório:

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

## Princípios de design

1. Proteja a intenção do projeto e a realidade da implementação.
2. Use o menor processo que continue seguro.
3. Torne mudanças materiais rastreáveis.
4. Preserve decisões importantes.
5. Deixe explícitos os responsáveis e limites de responsabilidade.
6. Trate mudanças arquiteturais como mudanças de risco.
7. Faça o conhecimento importante sobreviver entre sessões.
8. Evite duplicar fontes de verdade.
9. Prefira consistência sem burocracia desnecessária.
10. Deixe o projeto compreensível para um novo desenvolvedor.

## Integração das instruções do agent

Instalar a skill do APOS não altera o projeto automaticamente. Para que os agents se lembrem do APOS de forma consistente, integre-o ao `AGENTS.md` e/ou `CLAUDE.md`.

O fluxo completo permanece em [`skill/apos/SKILL.md`](./skill/apos/SKILL.md). Nos arquivos de instruções, adicione um lembrete curto e obrigatório para verificar `.apos/`, classificar a tarefa, validar, executar o Finish Protocol, sincronizar o estado e fornecer um APOS Report. Não copie a skill inteira e preserve as instruções existentes.

Instalar a skill e adotar o APOS no projeto são ações diferentes: primeiro execute `npx skills add` e depois adicione ou atualize `AGENTS.md`, `CLAUDE.md` e o estado mínimo de `.apos/`. Em um projeto vazio, faça isso depois de esclarecer a intenção; em um projeto existente, revise e preserve primeiro as instruções atuais.

## Arquivos

- [`skill/apos/SKILL.md`](./skill/apos/SKILL.md) — instruções principais carregadas pelo coding agent.
- [`README.md`](./README.md) — documentação em inglês.

## Status e licença

O APOS está atualmente na etapa **Beta / governance skill**. A API, o console, a CLI, o MCP adapter e o banco de dados serão planejados separadamente. Este repositório ainda não possui uma licença especificada.
