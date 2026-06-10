# PRD — Agente de IA "Jurista Experiente"

| Campo | Valor |
|---|---|
| **Produto** | Agente de IA que analisa um processo judicial cível como um jurista experiente |
| **Plataforma v1** | Plugin Claude Code (evolução do scaffold SuperJurista) |
| **Usuários-alvo** | Defensor Público (1ª classe) e Advogado cível |
| **Status** | Proposta aprovada — pronta para implementação |
| **Data** | Junho/2026 |
| **Autor** | Thirso del Corso (com pesquisa e consolidação assistidas por IA) |

---

## 1. Visão e contexto

### 1.1 O problema

Defensores Públicos e advogados cíveis enfrentam três gargalos crônicos:

1. **Volume**: a Defensoria cível (especialmente em curadoria especial) recebe lotes de processos semelhantes — monitórias, execuções, cumprimentos de sentença — em que o tempo disponível por processo é de minutos, não de horas. Cada processo exige ler os autos, identificar a fase, escolher a peça cabível, selecionar teses e redigir.
2. **Profundidade**: a análise de qualidade (linha do tempo, lacunas probatórias, teses disponíveis com condições de cabimento, jurisprudência atual, prognóstico) é trabalho de parecerista sênior — escassa e cara.
3. **Risco de IA mal usada**: o uso ingênuo de LLMs já gerou sanções reais no Brasil — TJSC multou recorrente em 10% do valor da causa por jurisprudência inexistente gerada por ChatGPT; o TST condenou empresa e advogado por citações falsas; a Justiça Federal/PR aplicou multas por dispositivos legais inexistentes. LLMs puros alucinam de 58% a 82% em perguntas jurídicas, e mesmo ferramentas comerciais com RAG ainda erram (estudo de Stanford: Lexis+ AI ~17%, Westlaw AI ~33% de alucinação).

### 1.2 A oportunidade

A pesquisa de mercado (jun/2026) mostra uma lacuna clara: **nenhuma ferramenta brasileira entrega, em conjunto, parecer completo + prognóstico de risco + minuta da peça cabível com citações verificadas**:

| Player | O que entrega | O que falta |
|---|---|---|
| Jus IA (Jusbrasil) | Pesquisa, peças, validação de citações ("Análise de Referências") | Análise estratégica profunda dos autos; perspectiva defensorial |
| EscavAI (Escavador) | Panorama do processo, chat sobre o caso | Parecer com teses/riscos/prognóstico; minuta por fase |
| Turivius (GPTuri) | Jurisprudência + jurimetria | Análise dos autos; redação de peças |
| CoCounsel / Lexis+ AI / Harvey (internacional) | Workflows, linha do tempo, citações ancoradas | Direito brasileiro; atuação defensorial |

### 1.3 Por que agora

- **Resolução CNJ 615/2025** estabeleceu o padrão regulatório de IA no Judiciário (abordagem baseada em risco, supervisão humana obrigatória, explicabilidade, LGPD/segredo de justiça). Ainda que dirigida ao Judiciário, é o benchmark de compliance que este produto espelha por desenho.
- **Ativos prontos**: o scaffold SuperJurista (este repositório) e o projeto JurisAI (branch RAG) já contêm a maior parte da engenharia de domínio necessária — o que falta é recombiná-los na perspectiva postulatória/defensorial (ver §5 e Apêndice A).

### 1.4 Tese do produto

> **"Dado o PDF dos autos, o agente entrega em minutos o que um parecerista sênior somado a um excelente assessor entregariam em dias: parecer completo, prognóstico honesto e a minuta da peça cabível — com cada citação verificada na fonte."**

---

## 2. Personas e jobs-to-be-done

### 2.1 Persona primária — Defensor(a) Público(a) cível

- Atua em curadoria especial (réu revel citado por edital/hora certa), pautas típicas da DPE (saúde, moradia/despejo, família, consumidor, vulneráveis) e alto volume de processos padronizáveis.
- Tem prerrogativas próprias (LC 80/94 e LC estadual): prazo em dobro, intimação pessoal com integralidade dos autos, atuação independente de mandato, gratuidade da justiça para o assistido.
- **JTBD**: "Quando recebo a intimação de um processo de curadoria, preciso saber em minutos qual peça cabe, quais teses tenho e receber uma minuta defensável — sem risco de citar jurisprudência falsa."

### 2.2 Persona secundária — Advogado(a) cível

- Recebe autos volumosos (cível estadual) e precisa de avaliação estratégica do caso: teses, riscos, chances de êxito e a próxima peça.
- **JTBD**: "Quando assumo um processo em andamento, preciso de um parecer completo dos autos e de um prognóstico honesto antes de definir a estratégia com o cliente."

### 2.3 Princípio transversal

O agente **assiste, o jurista decide**: toda saída é um produto de trabalho revisável, com aviso de revisão obrigatória, score de confiança e citações rastreáveis até a fonte (autos ou jurisprudência verificada). Nunca atua de forma autônoma perante o cliente final ou o juízo.

---

## 3. Escopo

### 3.1 Dentro do escopo (v1)

- **Plataforma**: plugin Claude Code instalado sobre o scaffold SuperJurista deste repositório.
- **Entrada**: PDF dos autos fornecido manualmente pelo usuário (agnóstico de tribunal — funciona para qualquer TJ; OCR híbrido já existente no scaffold).
- **Área**: cível estadual (processo civil — CPC/15, CC, CDC, CF/88).
- **Fases processuais cobertas na geração de minutas**:
  1. **Postulatória** — petição inicial, contestação, reconvenção, réplica
  2. **Instrutória/saneamento** — especificação de provas, manifestação sobre laudos, alegações finais
  3. **Recursal** — apelação, contrarrazões, agravo de instrumento, embargos de declaração
  4. **Execução/cumprimento** — impugnação ao cumprimento de sentença, embargos à execução, impugnação à penhora, exceção de pré-executividade
- **Kit hipossuficiência como requisito de primeira classe**: gratuidade da justiça (arts. 98–99 CPC, CadÚnico), prerrogativas da Defensoria (LC 80/94: prazo em dobro, intimação pessoal, atuação independente de mandato) e curadoria especial (art. 72, II CPC; negativa geral do art. 341, parágrafo único) — verificados e invocados automaticamente nas peças quando o usuário atua como Defensor.
- **Jurisprudência**: JusMCP (STF/STJ/TJs, com níveis de autoridade A–E) como fonte de pesquisa **e** de verificação citação-a-citação.

### 3.2 Fora do escopo (v1)

| Item | Destino |
|---|---|
| App web multi-tenant (frontend, banco de dados, Clerk, Celery) | v2 — reaproveitando o backend JurisAI |
| Download automático de autos (Projudi/e-SAJ/eproc/PJe estadual) | v1.x — scraper Projudi do JurisAI empacotado como MCP |
| DataJud para metadados/movimentações | v1.x |
| Áreas penal, trabalhista, tributária | Backlog |
| Atendimento direto ao assistido / triagem pré-processual | Backlog |

---

## 4. Especificação funcional

### 4.1 Comando `/analisar-processo` → Parecer do Jurista Experiente

**Entrada**: caminho do(s) PDF(s) dos autos + papel do usuário (Defensor/Advogado) + polo que representa.

**Saída**: arquivo `[NUMERO_CNJ]-parecer.md` (convenção de nomenclatura do scaffold) contendo:

1. **Identificação** — número CNJ, juízo, classe, assuntos, valor da causa
2. **Partes e representação** — polos, qualificação, situação (revelia? citação por edital? curadoria?)
3. **Linha do tempo processual** — cronologia com marcos, alertas e prazos correntes
4. **Fase processual detectada** — com a justificativa (movimentos que a evidenciam)
5. **Síntese dos fatos e pedidos** — teoria do caso de cada polo, fato pivô
6. **Análise probatória** — inventário das provas, qualidade, lacunas (metodologias Haack/Pearl/detector de lacunas já existentes no scaffold, operando em modo postulatório: "o que falta provar para a minha tese / o que falta ao adversário")
7. **Teses disponíveis** — cada tese com: condição de cabimento verificada nos autos, fundamento legal, jurisprudência de suporte (com nível de autoridade A–E), força relativa
8. **Riscos e pontos de atenção** — prescrição/decadência, preclusões, nulidades, sucumbência, prazos em curso
9. **Prognóstico fundamentado** — cenários (êxito total/parcial/improcedência), probabilidade qualitativa (provável/possível/remota) com a justificativa de cada cenário, impacto econômico estimado
10. **Recomendação** — próxima peça cabível, teses recomendadas em ordem de força, estratégia sugerida

**Regra de ouro (herdada das Iron Laws do scaffold)**: toda afirmação do parecer referencia o documento e a página dos autos de onde foi extraída; toda jurisprudência citada foi verificada via JusMCP.

### 4.2 Comando `/minutar-peticao` → Minuta da peça cabível

**Entrada**: parecer gerado (ou os autos diretamente) + peça desejada (ou "a cabível") + teses selecionadas pelo usuário (ou recomendadas pelo parecer).

**Pipeline de geração** (adaptado do agente LangGraph de 7 nós do JurisAI):

1. **Plano determinístico da peça** — tabela fase→peça cabível→teses (taxonomia herdada do JurisAI, ver §5.3) gera um checklist de seções e pedidos obrigatórios. Decisão de cabimento é **determinística** (tabela + condições verificadas nos autos), não criativa — o LLM não escolhe a peça sozinho.
2. **Seleção de template** — se houver modelo DPE aplicável (20 modelos reais migrados do JurisAI, ver Apêndice A.2), ele ancora estrutura, cabeçalho e fechamento; senão, usa template genérico por tipo de peça.
3. **Pesquisa dirigida** — JusMCP por tese, priorizando autoridade A/B (súmula vinculante, repetitivos, repercussão geral).
4. **Geração** — método IRAC por tese (Issue-Rule-Application-Conclusion), citações marcadas `[REF:N]`, linguagem forense brasileira (regras do `base_juridico.md` do JurisAI).
5. **Verificação citação-a-citação** — cada `[REF:N]` é confrontado com a fonte (JusMCP para jurisprudência; texto legal para artigos; autos para fatos). Não verificado ⇒ tag `[VERIFY]` visível + listagem em seção "⚠️ Revisão necessária". Até 2 ciclos de re-pesquisa antes de marcar como não verificado.
6. **Score de confiança** — fórmula herdada do JurisAI: `0.55 × taxa de citações verificadas + 0.30 × completude estrutural (checklist) + 0.15 × adequação de extensão`, com detalhamento dos componentes.
7. **Revisão humana (gate)** — a minuta sai com cabeçalho de produto de trabalho e aviso obrigatório: gerada por IA, revisão por Defensor/advogado(a) inscrito(a) na OAB é condição para uso.

**Requisitos defensoriais embutidos** (quando papel = Defensor):
- Fechamento obrigatório com prerrogativas (intimação pessoal com integralidade dos autos, prazo em dobro, atuação independente de mandato — arts. 44/89/128 LC 80/94 + LC estadual)
- Em curadoria especial: defesa por negativa geral (art. 341, parágrafo único CPC) sempre incluída como tese de fechamento
- Verificação de gratuidade da justiça (arts. 98–99 CPC; CadÚnico quando documentado nos autos)

### 4.3 Q&A sobre os autos (camada de exploração)

Após o parecer, o usuário conversa livremente com o agente sobre o processo ("qual o teor da decisão do dia X?", "o autor juntou o contrato?"). Toda resposta cita documento + página. Sem comando dedicado: é o comportamento padrão do chat com o contexto do processo carregado.

### 4.4 Fluxo típico (Defensor, curadoria especial)

```
1. Defensor baixa o PDF dos autos e salva em data/processos/0601122-24.2024.8.04.0001/
2. /analisar-processo data/processos/0601122-24.2024.8.04.0001/autos.pdf --papel defensor --polo reu
   → OCR → classificação de peças → linha do tempo → fase: "monitória, prazo de embargos correndo"
   → parecer com 4 teses (nulidade citação por edital, prescrição Lei 14.195/2021,
     impugnação documental, negativa geral) + prognóstico
3. Defensor lê o parecer (5 min), seleciona teses 1, 2 e negativa geral
4. /minutar-peticao --teses nulidade_citacao_edital,prescricao_lei_14195,negativa_geral
   → minuta de Embargos à Monitória sobre o modelo DPE, com citações verificadas,
     score de confiança 0.91, prerrogativas no fechamento
5. Defensor revisa no editor, ajusta e protocola
```

---

## 5. Arquitetura

### 5.1 Princípios

- **Orquestrador Cego com pipelines determinísticos** (framework v2.7 deste repositório): o orquestrador injeta contexto e delega para subagentes especializados; etapas e artefatos intermediários são previsíveis e auditáveis (cada etapa grava `[NUMERO_CNJ]-<etapa>.md`).
- **Determinístico onde o erro é caro** (cabimento de peça, checklist estrutural, verificação de citação), **generativo onde agrega valor** (análise, argumentação, redação).
- **File-based na v1** (padrão do scaffold): sem banco de dados; cada processo vive em sua pasta. Persistência estruturada fica para a v2.

### 5.2 Pipeline `/analisar-processo`

```
PDF dos autos
  │
  ├─ [skill: converter-pdf]            OCR híbrido (Tesseract + pdfplumber)     — EXISTENTE
  ├─ [agente: seletor-documentos]      classificação das peças dos autos        — EXISTENTE
  ├─ [agente: linha-tempo-processual]  cronologia + marcos + alertas            — EXISTENTE
  ├─ [agente: identificador-fase]      fase processual + peças cabíveis + prazos — NOVO
  ├─ [agentes: probatica-haack / probatica-pearl / detector-lacunas]
  │                                    análise probatória em modo postulatório  — EXISTENTES (variante)
  ├─ [agente: estrategista-postulatorio] teses + condições de cabimento + força — NOVO
  ├─ [pesquisa: JusMCP]                jurisprudência por tese (autoridade A–E) — MCP JÁ DISPONÍVEL
  ├─ [agente: consolidador-pesquisa]   síntese das fontes                       — EXISTENTE
  ├─ [agente: prognosticador]          cenários + probabilidade qualitativa     — NOVO
  └─ [agente: relator-parecer]         parecer final consolidado                — NOVO (derivado do relator-marmelstein)
```

### 5.3 Pipeline `/minutar-peticao`

```
Parecer (ou autos)
  │
  ├─ [planner determinístico]          tabela fase→peça→teses + checklist        — NOVO (porte do piece_planner do JurisAI)
  ├─ [seleção de template]             20 modelos DPE + genéricos                — MIGRADO do JurisAI
  ├─ [pesquisa: JusMCP]                jurisprudência dirigida por tese          — MCP JÁ DISPONÍVEL
  ├─ [agente: redator-peticao]         IRAC + [REF:N] + linguagem forense        — NOVO (prompts do JurisAI)
  ├─ [agente: verificador-citacoes]    citação-a-citação vs JusMCP/lei/autos     — NOVO (evolução do verificador-fontes)
  │     └─ não verificado & ciclos < 2 → re-pesquisa dirigida (loop)
  ├─ [revisores existentes]            verificador-calculos, analista-embargabilidade — EXISTENTES
  └─ [score + gate de revisão humana]  confiança + [VERIFY] tags + aviso OAB     — NOVO (fórmula do JurisAI)
```

### 5.4 Componentes a criar (estrutura no scaffold)

| Componente | Caminho proposto | Origem do conteúdo |
|---|---|---|
| Orquestrador `/analisar-processo` | `scaffold/commands/analisar-processo.md` | novo (padrão `spec/templates/orquestrador.md`) |
| Orquestrador `/minutar-peticao` | `scaffold/commands/minutar-peticao.md` | novo |
| Agente `identificador-fase` | `scaffold/agents/estrategia/identificador-fase.md` | lógica de detecção de fase do JurisAI |
| Agente `estrategista-postulatorio` | `scaffold/agents/estrategia/estrategista-postulatorio.md` | `TESE_INSTRUCOES` do JurisAI + novo |
| Agente `prognosticador` | `scaffold/agents/estrategia/prognosticador.md` | novo |
| Agente `relator-parecer` | `scaffold/agents/estrategia/relator-parecer.md` | derivado de `scaffold/agents/extracao/relator-marmelstein.md` |
| Agente `redator-peticao` | `scaffold/agents/redacao/redator-peticao.md` | prompts `system.py` + `base_juridico.md` do JurisAI |
| Agente `verificador-citacoes` | `scaffold/agents/revisao/verificador-citacoes.md` | evolução de `verificador-fontes.md` + node validate do JurisAI |
| Skill `taxonomia-pecas` (tabela fase→peça→tese + checklists) | `scaffold/skills/taxonomia-pecas/` | `_TESE_TEMPLATE` + piece_planner do JurisAI |
| Templates de peças | `scaffold/knowledge/templates/` (20 DPE + 5 genéricos) | `Modelos_DPE/` do JurisAI |
| Prompt-base jurídico | `scaffold/knowledge/prompts/base_juridico.md` | migrado integralmente do JurisAI |

### 5.5 Modelos LLM

Seguindo a convenção do scaffold: **Opus/Sonnet para análise e redação; Haiku para verificação e extração rápida** (no JurisAI, a verificação citação-a-citação com modelo rápido custou ~US$0,005/citação). O plugin roda nativamente no Claude Code — sem necessidade de infraestrutura própria de LLM na v1.

---

## 6. Requisitos não-funcionais

| Requisito | Especificação |
|---|---|
| **Grounding verificável** | Nenhuma citação de lei/súmula/jurisprudência sai sem verificação contra fonte; o não verificado sai com tag `[VERIFY]` explícita e listado em seção destacada. Princípio: *é melhor admitir "não localizei fundamento" do que inventar* (regra 3 do base_juridico). |
| **Supervisão humana** | Toda minuta carrega cabeçalho de produto de trabalho + aviso de revisão obrigatória por profissional habilitado. O agente nunca protocola, envia ou comunica nada externamente. Espelha a Res. CNJ 615/2025 (funções de alto risco exigem revisão humana efetiva). |
| **Explicabilidade** | Toda conclusão do parecer referencia documento/página dos autos; todo cenário do prognóstico expõe sua justificativa; o score de confiança expõe seus componentes. |
| **LGPD / segredo de justiça** | Os autos permanecem na máquina do usuário (file-based). Tráfego externo limita-se ao LLM (Claude) e à consulta de jurisprudência (JusMCP) — nunca com upload integral dos autos para serviços de terceiros além do LLM. Processos sob segredo: responsabilidade de uso informada na documentação. |
| **Desempenho (alvo)** | Parecer completo de autos de ~500 páginas em ≤ 15 min; minuta em ≤ 5 min após o parecer. |
| **Custo (alvo)** | ≤ US$ 2,00 por análise completa (referência JurisAI: ~US$0,15/peça gerada + ~US$0,07/análise em modelos de 2025). |
| **Auditabilidade** | Artefatos intermediários de cada etapa gravados em disco com convenção `[NUMERO_CNJ]-<etapa>.md`. |

---

## 7. Métricas de sucesso e avaliação

### 7.1 Métricas de produto

- **Taxa de citação verificada = 100%** nas minutas entregues (citações não verificadas só saem com tag).
- **Acurácia de fase processual ≥ 95%** e **acurácia de peça cabível ≥ 95%** em conjunto de teste.
- **Recall de teses ≥ 90%**: das teses que um Defensor experiente identificaria, o agente captura ao menos 90%.
- **Tempo economizado**: baseline manual vs. com agente, medido com usuários reais (alvo: redução ≥ 70% no tempo até a minuta revisada).
- **Taxa de aproveitamento**: % de minutas protocoladas com edição leve (alvo: ≥ 60% na v1).

### 7.2 Suíte de avaliação (inspirada no LegalBench, adaptada ao processo civil brasileiro)

Conjunto de processos reais **anonimizados** (começando pelos casos de curadoria do próprio usuário), com gabarito por eixo de raciocínio:

| Eixo (LegalBench) | Tarefa adaptada |
|---|---|
| Issue-spotting | Identificar teses disponíveis nos autos |
| Rule-recall | Citar a norma correta (artigo/súmula) por tese |
| Rule-application | Aplicar a norma aos fatos do caso (IRAC) |
| Rule-conclusion | Cabimento da peça por fase |
| Interpretation | Extração fiel de fatos/decisões dos autos (Q&A com gabarito) |
| Rhetorical-understanding | Qualidade argumentativa da minuta (rubrica avaliada por Defensor) |

A suíte roda a cada mudança relevante de prompts/agentes (regressão de qualidade), com resultados registrados em `docs/avaliacoes/`.

---

## 8. Riscos e mitigações

| Risco | Impacto | Mitigação |
|---|---|---|
| Alucinação de jurisprudência/lei | Sanção processual, dano reputacional | Verificação citação-a-citação via JusMCP + tags `[VERIFY]` + regra "admitir ausência de fundamento" |
| OCR ruim em autos digitalizados | Análise baseada em texto corrompido | Score de qualidade do OCR; abaixo do limiar, alerta no parecer + reprocessamento página a página (Vision) |
| Peça/tese errada para a fase | Preclusão, perda de prazo | Cabimento decidido por tabela determinística + condições verificadas nos autos; LLM não decide cabimento sozinho |
| Excesso de confiança do usuário | Protocolo sem revisão | Gate de revisão humana, score de confiança visível, seção de pontos não verificados sempre no topo |
| Prazos calculados errados | Perda de prazo | v1 *não* calcula prazos de forma conclusiva: aponta o prazo legal + termo inicial provável e manda conferir a intimação; cálculo automático fica para v1.x |
| Vazamento de dados sensíveis | Violação LGPD/segredo | Processamento local; documentação clara sobre o que trafega para o LLM; anonimização na suíte de avaliação |
| Dependência do JusMCP (cota/cobertura) | Verificação degradada | Fallback: marcar como `[VERIFY]` e degradar com transparência, nunca afirmar sem fonte |

---

## 9. Roadmap

### v1 — Plugin "Jurista Experiente" (este PRD)
1. Migrar ativos do JurisAI para o scaffold (templates, base_juridico, taxonomia, checklists)
2. Criar agentes novos (`identificador-fase`, `estrategista-postulatorio`, `prognosticador`, `relator-parecer`, `redator-peticao`, `verificador-citacoes`) usando os meta-comandos do repositório (`/criar-agente`, `/criar-orquestrador`)
3. Criar orquestradores `/analisar-processo` e `/minutar-peticao`
4. Montar suíte de avaliação com 10–20 processos anonimizados e validar métricas (§7)
5. Dogfooding pelo usuário em casos reais de curadoria

### v1.x — Integrações
- Scraper Projudi (TJAM) do JurisAI empacotado como servidor MCP (busca + download de autos pelo número CNJ)
- DataJud (API pública CNJ) para capa/movimentações e detecção de fase sem precisar do PDF
- MCP de jurisprudência do TJ local (skill `criar-mcp-precedente` já existente no repositório)
- Cálculo assistido de prazos (com prerrogativa de prazo em dobro)

### v2 — Produto web (SaaS)
- Reaproveitar o backend JurisAI (FastAPI + LangGraph + Qdrant + Postgres multi-tenant + frontend Next.js com editor TipTap, painel de citações e badge de confiança), substituindo seus prompts/taxonomias pelos validados na v1
- Os entregáveis e regras deste PRD permanecem; muda apenas a plataforma

---

## Apêndice A — Inventário de ativos reaproveitáveis

### A.1 Deste repositório (superjurista-dev)

| Ativo | Caminho | Uso no novo agente |
|---|---|---|
| OCR híbrido | `scaffold/skills/converter-pdf/` | Ingestão dos autos (etapa 1) |
| Classificador de peças | `scaffold/agents/extracao/seletor-documentos.md` | Mapa dos autos |
| Linha do tempo | `scaffold/agents/extracao/linha-tempo-processual.md` | Parecer §3 |
| Relator estruturado | `scaffold/agents/extracao/relator-marmelstein.md` | Base do `relator-parecer` |
| Análise probatória | `scaffold/agents/analise/probatica-haack.md`, `probatica-pearl.md`, `detector-lacunas.md` | Parecer §6 (modo postulatório) |
| Consolidação de pesquisa | `scaffold/agents/pesquisa/consolidador-pesquisa.md` | Síntese de jurisprudência |
| Revisores | `scaffold/agents/revisao/verificador-fontes.md`, `verificador-calculos.md`, `analista-embargabilidade.md` | Revisão da minuta |
| Meta-comandos | `commands/criar-agente.md`, `commands/criar-orquestrador.md` | Criação dos componentes novos |
| Framework/templates | `spec/README.md`, `spec/templates/` | Padrão Orquestrador Cego v2.7 |
| Iron Laws | `scaffold/project-claude.md` | Regras de citação e verificação |

### A.2 Do JurisAI (branch RAG — zip analisado em jun/2026)

| Ativo | Caminho no JurisAI | Uso no novo agente |
|---|---|---|
| Prompt-base jurídico (regras de citação + IRAC + formatação) | `knowledge/prompts/base_juridico.md` | Migrar integralmente |
| 20 modelos reais DPE/AM (curadoria, negativa geral, novo endereço, penhora, gratuidade CadÚnico etc.) | `Modelos_DPE/` | Templates do `redator-peticao` |
| 5 templates genéricos (inicial cobrança/consumidor, contestação, embargos à monitória com 6 teses, manifestação) | `knowledge/templates/` | Templates complementares |
| Taxonomia tese→template + instruções por tese | `apps/api/app/api/v1/pecas.py` (`_TESE_TEMPLATE`, `TESE_INSTRUCOES`) | Skill `taxonomia-pecas` |
| Detecção de fase por movimentos + alerta de cabimento | `apps/api/app/services/pipeline_processo.py` | Agente `identificador-fase` |
| Planner determinístico (checklists de seções/pedidos obrigatórios) | `apps/api/app/services/piece_planner.py` | Validação estrutural da minuta |
| Validação citação-a-citação + loop de re-pesquisa | `apps/api/app/agent/nodes/validate.py` + `graph.py` | Agente `verificador-citacoes` |
| Fórmula de score de confiança (0.55/0.30/0.15) | node `score_confidence` | Score da minuta |
| Padrão de revisão humana obrigatória (compliance OAB) | `apps/web/src/components/pieces/review-banner.tsx` | Gate de revisão (texto/fluxo) |
| Scraper Projudi/TJAM (Selenium + CDP) | `apps/scraper/projudi_service.py` | v1.x — empacotar como MCP |
| Arquitetura RAG 5 coleções + retrieval híbrido (BGE-M3 + BM25 + RRF + reranker) | `apps/api/app/services/rag*.py` | v2 (web) |

### A.3 Da sessão / ecossistema

- **JusMCP** — pesquisa jurisprudencial STF/STJ/TJs com níveis de autoridade (A: vinculante forte → E: editorial); fonte de pesquisa e verificação na v1.

## Apêndice B — Fontes da pesquisa externa

- Resolução CNJ 615/2025 — https://atos.cnj.jus.br/atos/detalhar/6001
- DataJud API Pública — https://www.cnj.jus.br/sistemas/datajud/api-publica/
- Stanford, *Hallucination-Free? Assessing the Reliability of Leading AI Legal Research Tools* — https://arxiv.org/pdf/2405.20362
- LegalBench — https://hazyresearch.stanford.edu/legalbench/
- Jus IA (Jusbrasil) — https://ia.jusbrasil.com.br/ ; EscavAI — https://escavai.escavador.com/ ; Turivius — https://turivius.com/
- TJSC: multa por jurisprudência falsa via IA — https://www.tjsc.jus.br/web/imprensa/-/tjsc-multa-autor-de-recurso-por-jurisprudencia-falsa-gerada-por-ia
- TST: condenação por citações falsas — https://www.tst.jus.br/en/-/empresa-e-advogado-sao-condenados-por-possivel-uso-de-ia-com-citacoes-falsas-de-jurisprudencia
- Thomson Reuters, *Legal AI Benchmarking: Long Context* — https://www.thomsonreuters.com/en-us/posts/innovation/legal-ai-benchmarking-evaluating-long-context-performance-for-llms/
- Caso Victor (STF/UnB) — https://www.scielo.br/j/rinc/a/YKZfQPLJqT7F3P445KkmwnC/?lang=pt

## Apêndice C — Glossário

| Termo | Significado |
|---|---|
| **Curadoria especial** | Atuação da Defensoria em favor de réu revel citado por edital/hora certa ou incapaz sem representante (art. 72, II CPC) |
| **Negativa geral** | Prerrogativa do curador especial de contestar por negação geral dos fatos (art. 341, parágrafo único CPC) |
| **IRAC** | Issue-Rule-Application-Conclusion — método de estruturação de argumento jurídico |
| **Iron Laws** | Regras invioláveis do scaffold: nenhuma citação sem verificação, nenhuma afirmação sem referência |
| **Orquestrador Cego** | Padrão arquitetural do scaffold: o orquestrador delega a subagentes injetando contexto, sem interpretar o conteúdo |
| **JusMCP** | Servidor MCP de pesquisa jurisprudencial brasileira com ranking por autoridade (A–E) |
| **`[REF:N]` / `[VERIFY]`** | Marcação de citação ancorada na fonte N / marcação de afirmação não verificada que exige conferência humana |
