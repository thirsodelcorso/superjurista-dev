# superjurista-dev

Plugin de meta-ferramentas para criar e customizar sistemas agenticos judiciais com Claude Code. Inclui ferramentas para criar agentes, orquestradores, skills, teams e um scaffold completo do sistema SuperJurista -- um sistema de inteligencia aumentada para processamento de processos judiciais, construido com arquitetura de pipelines deterministicos.

**Novidade desta versao:** o agente **Jurista Experiente** -- dois pipelines para a atuacao postulatoria/defensorial (Defensoria Publica e advocacia civel estadual): `/analisar-processo` gera um parecer completo dos autos (fase, provas, teses, jurisprudencia, prognostico) e `/minutar-peticao` gera a minuta da peca cabivel a fase, com verificacao citacao-a-citacao via JusMCP e score de confianca. Inclui 25 modelos reais de pecas (DPE/AM + genericos) e taxonomia deterministica fase-peca-tese.

## Instalacao

### Opcao 1: Dentro de uma sessao do Claude Code (recomendado)

Tres comandos e o sistema inteiro esta funcionando:

```
/plugin marketplace add thirsodelcorso/superjurista-marketplace
/plugin install superjurista-dev@thirsodelcorso-superjurista-marketplace
/instalar-superjurista
```

**Passo a passo:**

1. **Adicionar o marketplace** -- registra o repositorio de plugins do SuperJurista:
   ```
   /plugin marketplace add thirsodelcorso/superjurista-marketplace
   ```

2. **Instalar o plugin** -- baixa as meta-ferramentas e o scaffold:
   ```
   /plugin install superjurista-dev@thirsodelcorso-superjurista-marketplace
   ```

3. **Instalar o sistema no projeto** -- copia agentes, pipelines, skills e MCPs para `.claude/`:
   ```
   /instalar-superjurista
   ```

### Opcao 2: Via interface interativa

Digite `/plugin` para abrir o gerenciador visual com abas (Discover, Installed, Marketplaces, Errors). Navegue com Tab/Shift+Tab.

### Opcao 3: Desenvolvimento local

```bash
git clone https://github.com/thirsodelcorso/superjurista-dev.git
claude --plugin-dir ./superjurista-dev
```

Depois, dentro da sessao: `/instalar-superjurista`

## Gerenciamento do plugin

| Acao | Comando |
|------|---------|
| Abrir gerenciador interativo | `/plugin` |
| Adicionar marketplace | `/plugin marketplace add owner/repo` |
| Instalar plugin | `/plugin install plugin@marketplace` |
| Listar instalados | `/plugin` > aba Installed |
| Desinstalar | `/plugin uninstall superjurista-dev@thirsodelcorso-superjurista-marketplace` |
| Desativar (sem remover) | `/plugin disable superjurista-dev@thirsodelcorso-superjurista-marketplace` |
| Reativar | `/plugin enable superjurista-dev@thirsodelcorso-superjurista-marketplace` |
| Recarregar apos instalar | `/reload-plugins` |

### Escopo da instalacao

Ao instalar, voce pode escolher o escopo:

- **user** -- funciona em todos os seus projetos (padrao)
- **project** -- salva em `.claude/settings.json` do repo (todos que clonarem terao o plugin)
- **local** -- so voce, so neste repositorio

### Pre-requisito

Versao minima do Claude Code: **1.0.33+**. Se `/plugin` nao aparecer:

```bash
npm update -g @anthropic-ai/claude-code
```

## Comandos

| Comando | Descricao |
|---------|-----------|
| `/instalar-superjurista` | Instala o sistema SuperJurista completo no projeto atual |
| `/criar-agente` | Cria agentes modulares seguindo as SPECs v2.0 |
| `/criar-orquestrador` | Cria orquestradores (commands) com injecao de contexto |
| `/criar-skill` | Cria skills com TDD e CSO (Claude Search Optimization) |
| `/criar-team` | Cria Agent Teams (paralelo ou debate) |
| `/planejar-sistema` | Gera blueprint arquitetural antes de criar artefatos |

## Skills

- **criar-skill**: Workflow TDD para criacao de skills -- garante que a skill ensina o comportamento correto ao Claude, com testes de conformidade e otimizacao para busca interna.
- **criar-mcp-precedente**: Guia para criar servidores MCP de jurisprudencia com scraping de tribunais, incluindo padroes de extracao e configuracao de endpoints.
- **criar-pje-download**: Cria skills de download do PJE para qualquer tribunal via engenharia reversa de arquivos HAR -- identifica endpoints, cookies, headers e gera scripts Python parametrizados.

## O que o /instalar-superjurista cria

O comando `/instalar-superjurista` gera um sistema judicial completo no projeto atual:

- **18 pipelines e comandos** para processamento judicial (sentenca, embargos, pesquisa, revisao, analise de processo, minuta de peticao, etc.)
- **~56 agentes especializados** em 8 categorias (extracao, analise, estrategia, pesquisa, redacao, revisao, lista-trf, tribunal)
- **7 skills de dominio** (download PJE, conversao PDF, taxonomia de pecas, analise probatoria, captura de sessao, etc.)
- **Base de conhecimento juridico** (`knowledge/`): prompt-base de citacao/IRAC e 25 templates de pecas (modelos DPE/AM de curadoria especial + genericos)
- **2 servidores MCP locais** (TJSC eProc, TCU)
- **Estrutura de dados pronta** (`data/sentenca/`, `data/decisao/`, `data/processos/`)
- **CLAUDE.md e README.md** configurados para o projeto

### Estrutura gerada

```
projeto/
├── .claude/
│   ├── commands/           # 18 pipelines e comandos
│   ├── agents/
│   │   ├── analise/        # Marmelstein, Haack, Pearl, embargos, probatoria
│   │   ├── estrategia/     # Identificador de fase, estrategista, prognosticador, relator de parecer
│   │   ├── extracao/       # Linha do tempo, relator, conversor
│   │   ├── lista-trf/      # 9 agentes para listas de julgamento
│   │   ├── pesquisa/       # BNP, CJF, JULIA, JusMCP, consolidador
│   │   ├── redacao/        # Redator de minutas, redator de peticoes
│   │   ├── revisao/        # Verificadores (calculos, honorarios, fontes, citacoes)
│   │   └── tribunal/       # Acusador, defensor, juiz mediador
│   ├── skills/
│   │   ├── pje-download/   # API REST do PJE (10 scripts Python)
│   │   ├── converter-pdf/  # Conversao PDF para TXT com OCR hibrido
│   │   ├── taxonomia-pecas/     # Tabela fase-peca-tese + checklists (deterministico)
│   │   ├── analise-probatoria/  # Checklists por tipo de prova
│   │   ├── capturar-sessao-pje/ # Captura sessao via Chrome MCP
│   │   ├── analisador-erro-medico/ # Analise de erro medico
│   │   └── fork-terminal/  # Execucao paralela em terminais
│   ├── knowledge/
│   │   ├── prompts/        # base_juridico.md (citacao, IRAC, prerrogativas)
│   │   └── templates/      # 25 modelos de pecas (DPE/AM + genericos)
│   └── mcp-servers/
│       ├── tjsc-eproc/     # Jurisprudencia TJSC
│       └── tcu-jurisprudencia/ # Jurisprudencia TCU
├── data/
│   ├── sentenca/           # Processos para sentenca
│   ├── decisao/            # Processos para decisao
│   └── processos/          # Processos para analise/peticao (Jurista Experiente)
├── CLAUDE.md               # Configuracao do projeto
└── README.md               # Documentacao do projeto
```

## Jurista Experiente (atuacao postulatoria/defensorial)

Dois comandos transformam o PDF dos autos em parecer e minuta, na perspectiva de quem
peticiona (Defensor Publico ou advogado), em causas civeis estaduais:

| Comando | O que faz |
|---------|-----------|
| `/analisar-processo` | Gera o **Parecer do Jurista Experiente**: linha do tempo, fase processual e prazos, analise probatoria, teses com condicao de cabimento verificada, jurisprudencia (niveis de autoridade A-E), riscos, prognostico por cenarios e recomendacao da proxima peca |
| `/minutar-peticao` | Gera a **minuta da peca cabivel a fase** (a escolha da peca e deterministica, por tabela fase-peca-tese), ancorada em 25 templates reais, com fundamentacao IRAC, citacoes verificadas `[REF:N]`, tags `[VERIFY]` no nao confirmado e score de confianca |

Fluxo tipico (exemplo de curadoria especial):

```
/analisar-processo data/processos/0601122-24.2024.8.04.0001/autos.pdf
# le o parecer, escolhe as teses
/minutar-peticao 0601122-24.2024.8.04.0001 --teses nulidade_citacao_edital,negativa_geral
```

**Pre-requisito (verificacao de citacoes):** os dois comandos usam o **JusMCP**
(servidor MCP remoto de jurisprudencia, https://jusratio.com.br) para pesquisa e
verificacao citacao-a-citacao. Ele NAO e instalado por este plugin -- configure-o no seu
Claude Code. Sem ele, os comandos continuam funcionando, mas degradam com transparencia:
toda jurisprudencia sai marcada `[VERIFY]` (nao verificada) e o score de confianca reflete isso.

**Aviso (LGPD/segredo de justica):** o texto dos autos e injetado no contexto do modelo
de linguagem para analise e redacao. Nao use os pipelines em processos sob segredo de
justica sem avaliar a politica de tratamento de dados aplicavel. Toda minuta gerada e
produto de trabalho de IA: revisao por profissional habilitado e obrigatoria antes do protocolo.

## Framework

O sistema baseia-se no framework v2.7 de orquestracao agentica com padrao "Orquestrador Cego" e injecao de contexto. Neste padrao, commands (orquestradores) delegam tarefas via Task tool para subagentes que possuem contexto isolado -- cada agente le seu proprio prompt e recebe apenas os caminhos de workspace necessarios. Templates e referencias completas estao disponiveis em `spec/`.

## Dependencias

**Python 3.8+:**
```bash
pip install requests beautifulsoup4 pdfplumber PyPDF2 pdf2image pytesseract
```

**Sistema (para OCR):**
- Tesseract OCR com pacote de idioma portugues
- Poppler (Windows: extrair para `~/poppler/`)

**Servicos externos (opcionais por funcionalidade):**
- **JusMCP** -- pre-requisito de `/analisar-processo` e `/minutar-peticao` para
  verificacao de citacoes (sem ele, degradam para `[VERIFY]`)
- Sessao PJE capturada (`/capturar-sessao-pje`) -- apenas para os comandos de download

## Licenca

[MIT](LICENSE)
