# Changelog

## Não lançado

### Adicionado — Agente "Jurista Experiente" (atuação postulatória/defensorial, cível estadual)

- PRD em `docs/PRD-agente-jurista-experiente.md`
- 2 orquestradores no scaffold: `/analisar-processo` (Parecer do Jurista Experiente) e `/minutar-peticao` (minuta da peça cabível à fase)
- 6 agentes novos: `estrategia/identificador-fase`, `estrategia/estrategista-postulatorio`, `estrategia/prognosticador`, `estrategia/relator-parecer`, `redacao/redator-peticao`, `revisao/verificador-citacoes`
- Skill `taxonomia-pecas` (tabela determinística fase→peça→tese + checklists de completude)
- Base de conhecimento `knowledge/`: `prompts/base_juridico.md` e 25 templates de peças (modelos DPE/AM + genéricos, incorporados do projeto JurisAI)
- Integração com JusMCP para pesquisa e verificação citação-a-citação (níveis de autoridade A–E)
- `/instalar-superjurista` atualizado para copiar `knowledge/` e criar `data/processos/`

### Corrigido — revisão externa (pós-implementação v1)

- Agente `pesquisa/pesquisador-jusmcp` com tools MCP explícitas, pré-check de disponibilidade e degradação formal (substitui uso de general-purpose com MCP implícito nas etapas de pesquisa)
- JusMCP documentado como pré-requisito (project-claude.md, instalador, PRD); sem ele os pipelines degradam com transparência: jurisprudência sai `[VERIFY]` e componente de citações do score zera
- `AskUserQuestion` adicionado ao `allowed-tools` de `/analisar-processo` e `/minutar-peticao`
- Templates sanitizados: imagens base64 removidas (2 arquivos, ~27KB), nome de defensora → `{NOME_DEFENSOR}` (22 ocorrências), conta/agência FUNDEP → `{CONTA_FUNDEP}`/`{AGENCIA_FUNDEP}`
- `redator-peticao`: citações herdadas do template entram na tabela `[REF:N]` e passam pela verificação; placeholders sem dado verificado são preservados, nunca preenchidos por suposição
- `/analisar-processo` agora COPIA (não move) PDF solto para `data/processos/`
- Aviso operacional LGPD/segredo de justiça nos dois comandos
- Manifest e READMEs apontam para `thirsodelcorso/superjurista-dev` (autoria original de George Marmelstein preservada no campo `author`)

## 1.0.0 (2026-03-20)

### Adicionado

- 6 comandos: criar-agente, criar-orquestrador, criar-skill, criar-team, planejar-sistema, instalar-superjurista
- 2 skills: criar-skill (TDD), criar-mcp-precedente
- Framework spec v2.7 completo (templates, referências, checklists)
- Scaffold judicial com 16 commands, ~52 agents, 6 skills, 2 MCPs
- README para juristas (scaffold)
- CLAUDE.md template (scaffold)
