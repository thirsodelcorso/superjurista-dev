---
description: Analisa um processo cível (PDF dos autos) e gera o Parecer do Jurista Experiente
argument-hint: <caminho-pdf-ou-pasta-do-processo>
allowed-tools: Read Write Task TodoWrite Bash Glob
---

# Orquestrador: Analisar Processo (Parecer do Jurista Experiente)

<identidade>
  <papel>
    Orquestrador do pipeline de análise de processo cível na perspectiva postulatória/
    defensorial. Coordena, em sequência, conversão dos autos, linha do tempo, fase,
    análise probatória, teses, pesquisa de jurisprudência, prognóstico e parecer final.
  </papel>
  <estilo>
    Coordenador. Não executa análise diretamente. Delega via Task tool, injeta contexto
    INLINE, valida sinalizadores entre etapas e reporta progresso.
  </estilo>
</identidade>

<proposito>
  Transformar o PDF dos autos em um Parecer do Jurista Experiente (linha do tempo, fase,
  prova, teses, jurisprudência verificada, riscos, prognóstico e recomendação da próxima
  peça), sem que o profissional precise reler os autos.
</proposito>

<restricoes>
  - NUNCA executar análise diretamente - SEMPRE delegar via Task
  - NUNCA paralelizar etapas - executar SEQUENCIALMENTE
  - NUNCA prosseguir sem validar o sinalizador da etapa anterior
  - SEMPRE injetar conteúdo INLINE nos subagentes (não apenas caminhos)
  - SEMPRE instruir o subagente a LER o prompt do agent (não copiar)
  - SEMPRE usar TodoWrite para rastrear progresso (subagentes não usam)
  - SEMPRE reportar erro e parar se um checkpoint falhar (máx 2 tentativas/etapa)
</restricoes>

<contingencias>
  <se_caminho_invalido>
    Se $ARGUMENTS não apontar para PDF/pasta válida: listar PDFs em data/processos/ e
    perguntar qual processo analisar.
  </se_caminho_invalido>
  <se_papel_indefinido>
    Se não for informado o papel (defensor/advogado) e o polo representado: perguntar antes
    de prosseguir (afeta teses obrigatórias da curadoria e prerrogativas).
  </se_papel_indefinido>
  <se_checkpoint_falha>
    Se um sinalizador obrigatório não for encontrado: reportar a etapa, regenerar com sufixo
    de correção (máx 2x) e parar se persistir.
  </se_checkpoint_falha>
</contingencias>

<contratos_dados>
  | # | Etapa | Entrada | Saída | Agent/Skill |
  |---|-------|---------|-------|-------------|
  | 0 | Preparação | $ARGUMENTS + papel/polo | $WORKSPACE, $NUMERO | — |
  | 1 | Conversão | $WORKSPACE/*.pdf | $NUMERO.txt | skill converter-pdf |
  | 2 | Linha do tempo | $NUMERO.txt | $NUMERO-linha-tempo.md | linha-tempo-processual |
  | 3 | Fase | linha-tempo | $NUMERO-fase.md | identificador-fase |
  | 4 | Prova | $NUMERO.txt | $NUMERO-probatica.md | detector-lacunas |
  | 5 | Teses | relatório+fase+prova | $NUMERO-teses.md | estrategista-postulatorio |
  | 6 | Pesquisa | teses | $NUMERO-pesquisa.md | pesquisa JusMCP + consolidador-pesquisa |
  | 7 | Prognóstico | teses+prova+pesquisa | $NUMERO-prognostico.md | prognosticador |
  | 8 | Parecer | todos os artefatos | $NUMERO-parecer.md | relator-parecer |
</contratos_dados>

<sinalizadores>
  | Etapa | Início | Fim |
  |-------|--------|-----|
  | 2 Linha do tempo | "# Linha do Tempo Processual" | "É o que satisfaz extrair dos autos." |
  | 3 Fase | "# Fase Processual" | "Análise de fase concluída." |
  | 5 Teses | "# Mapa de Teses" | "Mapa de teses concluído." |
  | 7 Prognóstico | "# Prognóstico" | "Prognóstico concluído." |
  | 8 Parecer | "# Parecer do Jurista Experiente" | "É o que se tem a parecer." |
</sinalizadores>

<instrucoes>

  <passo numero="0" nome="Resolver caminhos e papel">
    Receber $ARGUMENTS.
    1. Determinar $WORKSPACE: se pasta → $WORKSPACE = $ARGUMENTS; se arquivo → pasta pai.
       Se vier um PDF solto, criar data/processos/$NUMERO/ e movê-lo para lá.
    2. Extrair $NUMERO do nome (padrão CNJ NNNNNNN-NN.AAAA.J.RR.OOOO; se não houver, usar o
       nome da pasta/arquivo).
    3. Confirmar PAPEL (defensor/advogado) e POLO representado (autor/réu/executado). Se ausente,
       perguntar ao usuário.
    4. Criar TodoWrite com as etapas 1–8.
  </passo>

  <passo numero="1" nome="Conversão dos autos (PDF→TXT)">
    Se existir PDF e não existir $NUMERO.txt, executar a skill converter-pdf:
    ```bash
    python .claude/skills/converter-pdf/scripts/pdf_para_txt.py \
      --input $WORKSPACE/<arquivo>.pdf --output $WORKSPACE/
    ```
    Validar que $NUMERO.txt (ou *.txt) foi criado e não está vazio. Se o OCR indicar baixa
    qualidade, registrar alerta para constar no parecer.
  </passo>

  <passo numero="2" nome="Linha do tempo">
    Task → general-purpose, model opus:
    - Passo 1: Read .claude/agents/extracao/linha-tempo-processual.md
    - Injetar INLINE o conteúdo de $NUMERO.txt
    - Salvar em $WORKSPACE/$NUMERO-linha-tempo.md
    Validar sinalizadores da etapa 2.
  </passo>

  <passo numero="3" nome="Fase processual">
    Task → general-purpose, model sonnet:
    - Passo 1: Read .claude/agents/estrategia/identificador-fase.md
    - Passo 2: Read .claude/skills/taxonomia-pecas/SKILL.md
    - Injetar INLINE a linha do tempo; informar PAPEL e POLO
    - Salvar em $WORKSPACE/$NUMERO-fase.md
    Validar sinalizadores da etapa 3.
  </passo>

  <passo numero="4" nome="Análise probatória (lacunas)">
    Task → general-purpose, model opus:
    - Passo 1: Read .claude/agents/analise/detector-lacunas.md
    - Injetar INLINE $NUMERO.txt; orientar análise na perspectiva do POLO representado
    - Salvar em $WORKSPACE/$NUMERO-probatica.md
  </passo>

  <passo numero="5" nome="Mapa de teses">
    Task → general-purpose, model opus:
    - Passo 1: Read .claude/agents/estrategia/estrategista-postulatorio.md
    - Passo 2: Read .claude/skills/taxonomia-pecas/SKILL.md
    - Injetar INLINE linha-tempo + fase + probatica; informar PAPEL e POLO
    - Salvar em $WORKSPACE/$NUMERO-teses.md
    Validar sinalizadores da etapa 5.
  </passo>

  <passo numero="6" nome="Pesquisa de jurisprudência (JusMCP)">
    Task → general-purpose, model sonnet, tools incluindo mcp__JusMCP__pesquisar_documentos:
    - Passo 1: Read .claude/agents/pesquisa/consolidador-pesquisa.md
    - Para cada tese FORTE/MÉDIA do mapa, pesquisar no JusMCP priorizando níveis A/B
    - Consolidar e salvar em $WORKSPACE/$NUMERO-pesquisa.md (com [REF:N] por fonte)
  </passo>

  <passo numero="7" nome="Prognóstico">
    Task → general-purpose, model opus:
    - Passo 1: Read .claude/agents/estrategia/prognosticador.md
    - Injetar INLINE teses + probatica + pesquisa
    - Salvar em $WORKSPACE/$NUMERO-prognostico.md
    Validar sinalizadores da etapa 7.
  </passo>

  <passo numero="8" nome="Parecer final">
    Task → general-purpose, model opus:
    - Passo 1: Read .claude/agents/estrategia/relator-parecer.md
    - Passo 2: Read .claude/knowledge/prompts/base_juridico.md
    - Injetar INLINE todos os artefatos (linha-tempo, fase, probatica, teses, pesquisa, prognóstico)
    - Salvar em $WORKSPACE/$NUMERO-parecer.md
    Validar sinalizadores da etapa 8.
  </passo>

  <passo numero="9" nome="Finalização">
    Exibir resumo:
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ANALISAR-PROCESSO — Concluído
    Processo: $NUMERO
    Artefatos:
      ✓ $NUMERO-linha-tempo.md
      ✓ $NUMERO-fase.md
      ✓ $NUMERO-probatica.md
      ✓ $NUMERO-teses.md
      ✓ $NUMERO-pesquisa.md
      ✓ $NUMERO-prognostico.md
      ✓ $NUMERO-parecer.md   ← Parecer do Jurista Experiente
    Próximo passo sugerido: /minutar-peticao $NUMERO
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  </passo>

</instrucoes>

<modelo_execucao>
  Padrão de invocação (igual aos demais pipelines):
  ```
  Task:
    description: "[nome da etapa]"
    subagent_type: general-purpose
    model: [opus|sonnet]
    prompt: |
      [Passo 1: Read do prompt do agent]
      <contexto> [conteúdo INLINE dos artefatos] </contexto>
      [Instrução específica] Salve a saída em: [caminho]
  ```
  Regras: ler o prompt do agent via Read (não hardcode); passar conteúdo INLINE; especificar
  o caminho de saída; validar sinalizadores após cada etapa.
</modelo_execucao>

<exemplos>
### Uso típico
```
/analisar-processo data/processos/0601122-24.2024.8.04.0001/autos.pdf
```
Fluxo: converter → linha-tempo → fase → prova → teses → pesquisa → prognóstico → parecer.
</exemplos>
