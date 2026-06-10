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

## 1.0.0 (2026-03-20)

### Adicionado

- 6 comandos: criar-agente, criar-orquestrador, criar-skill, criar-team, planejar-sistema, instalar-superjurista
- 2 skills: criar-skill (TDD), criar-mcp-precedente
- Framework spec v2.7 completo (templates, referências, checklists)
- Scaffold judicial com 16 commands, ~52 agents, 6 skills, 2 MCPs
- README para juristas (scaffold)
- CLAUDE.md template (scaffold)
