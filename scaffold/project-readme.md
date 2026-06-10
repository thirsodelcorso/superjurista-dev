# SuperJurista

## O que e o SuperJurista

O SuperJurista e um sistema de inteligencia aumentada que auxilia magistrados, Defensores Publicos e advogados no processamento de demandas judiciais. Ele funciona como um assistente especializado dentro do Claude Code, capaz de ler processos, pesquisar precedentes, analisar provas, redigir minutas de sentenca e -- na perspectiva de quem peticiona -- gerar pareceres completos e minutas de pecas processuais, tudo a partir de comandos simples.

O sistema automatiza as tarefas mais repetitivas do trabalho juridico: extrair informacoes dos autos, organizar linhas do tempo, identificar a fase processual e a peca cabivel, buscar jurisprudencia relevante em bancos como o BNP (STF/STJ), CJF, JULIA (TRF5) e JusMCP (STF/STJ/TJs), e produzir analises juridicas estruturadas. O que antes exigia horas de leitura e pesquisa pode ser feito em minutos.

E importante destacar: o SuperJurista assiste, nao substitui. O profissional mantem o controle integral sobre o resultado. Toda minuta gerada e uma proposta que deve ser revisada, ajustada e validada pelo magistrado, Defensor ou advogado responsavel antes de qualquer utilizacao. O sistema e uma ferramenta de apoio -- a decisao final e sempre humana.

## Pre-requisitos

Antes de comecar, certifique-se de que os seguintes programas estao instalados no seu computador:

1. **Claude Code** -- a interface de linha de comando da Anthropic. Consulte a documentacao oficial em https://docs.anthropic.com para instrucoes de instalacao.

2. **Python 3.8 ou superior** -- necessario para os scripts de download e conversao de documentos. Disponivel em https://www.python.org/downloads/.

3. **Tesseract OCR** -- software de reconhecimento optico de caracteres, usado para extrair texto de documentos digitalizados. Instale com o pacote de idioma portugues (`por`).

4. **Poppler** -- utilitario necessario para converter arquivos PDF em imagens durante o processo de OCR. No Windows, extraia os arquivos para a pasta `~/poppler/`.

Apos instalar o Python, execute o seguinte comando para instalar as bibliotecas necessarias:

```
pip install requests beautifulsoup4 pdfplumber PyPDF2 pdf2image pytesseract
```

5. **JusMCP** (apenas para o Jurista Experiente) -- servidor MCP remoto de pesquisa
   jurisprudencial (https://jusratio.com.br), usado por `/analisar-processo` e
   `/minutar-peticao` para pesquisar precedentes e verificar cada citacao. Configure-o
   no seu Claude Code. Sem ele, os dois comandos continuam funcionando, mas toda
   jurisprudencia sai marcada `[VERIFY]` (nao verificada) e exige conferencia manual.

## Primeiros passos

Siga este roteiro para processar seu primeiro caso:

1. **Capture sua sessao do PJE.** No Claude Code, execute o comando:
   ```
   /capturar-sessao-pje
   ```
   Isso abrira o navegador para que voce faca login no PJE. As credenciais de sessao serao salvas localmente (nunca sao enviadas para fora do seu computador).

2. **Baixe processos do PJE.** Para baixar 5 processos pendentes de sentenca e ja converter os PDFs em texto:
   ```
   /baixar-converter 5 sentenca
   ```
   Os processos serao salvos na pasta `data/sentenca/`, cada um em sua propria subpasta.

3. **Execute a analise completa de um processo.** Escolha um dos processos baixados e execute:
   ```
   /pipeline-sentenca data/sentenca/<numero-do-processo>/processo.txt
   ```
   Substitua `<numero-do-processo>` pelo numero real (exemplo: `0814624-28.2019.4.05.8100`).

4. **Encontre o resultado.** Os arquivos gerados estarao na mesma pasta do processo:
   ```
   data/sentenca/<numero-do-processo>/
   ```
   Voce encontrara o relatorio, a analise juridica e a minuta de fundamentacao, cada um em seu proprio arquivo.

## Comandos disponiveis

Todos os comandos sao executados dentro do Claude Code, digitando `/` seguido do nome do comando.

### Jurista Experiente -- parecer e peticao (Defensoria/advocacia civel)

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/analisar-processo` | Analisa o PDF dos autos e gera o Parecer do Jurista Experiente: linha do tempo, fase e prazos, provas e lacunas, teses cabiveis, jurisprudencia com nivel de autoridade, riscos e prognostico | `/analisar-processo data/processos/0601122-24.2024.8.04.0001/autos.pdf` |
| `/minutar-peticao` | Gera a minuta da peca cabivel a fase (contestacao, embargos, impugnacao, apelacao etc.), com teses selecionadas, citacoes verificadas e score de confianca | `/minutar-peticao 0601122-24.2024.8.04.0001 --teses nulidade_citacao_edital,negativa_geral` |

O fluxo recomendado e: rodar `/analisar-processo`, ler o parecer, escolher as teses e
entao rodar `/minutar-peticao`. Quando o usuario atua como Defensor Publico em curadoria
especial, o sistema inclui automaticamente a negativa geral (art. 341, paragrafo unico,
CPC), o pedido de gratuidade quando documentado e o fechamento com as prerrogativas da
LC 80/94. A minuta sai com cabecalho de produto de trabalho de IA e aviso de revisao
obrigatoria -- o sistema nunca protocola nada.

Atencao (LGPD/segredo de justica): o texto dos autos e enviado ao modelo de linguagem
para analise. Nao use estes comandos em processos sob segredo de justica sem avaliar a
politica de tratamento de dados aplicavel.

### Processamento de sentencas

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/pipeline-sentenca` | Analisa um processo e gera relatorio, analise e minuta de fundamentacao | `/pipeline-sentenca data/sentenca/0814624-28.2019.4.05.8100/processo.txt` |
| `/pipeline-sentenca-team` | Mesmo que o anterior, mas executa as pesquisas de precedentes em paralelo (mais rapido) | `/pipeline-sentenca-team data/sentenca/0814624-28.2019.4.05.8100/processo.txt` |
| `/pipeline-minutar-pdf` | Recebe um PDF e gera a sentenca completa, sem necessidade de conversao previa | `/pipeline-minutar-pdf documento.pdf` |

### Embargos de declaracao

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/pipeline-embargos` | Analisa embargos de declaracao e gera minuta de decisao | `/pipeline-embargos data/sentenca/<numero>/processo.txt` |

### Pesquisa de precedentes

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/pipeline-pesquisa` | Pesquisa jurisprudencia em multiplos bancos (BNP, CJF, JULIA) simultaneamente | `/pipeline-pesquisa "dano moral overbooking transporte aereo"` |
| `/planejamento-epistemico` | Monta um plano estruturado de pesquisa antes de executar as buscas | `/planejamento-epistemico "responsabilidade civil medica"` |

### Analise de provas

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/pipeline-probatica` | Analise probabilistica completa do conjunto probatorio do processo | `/pipeline-probatica data/sentenca/<numero>/processo.txt` |
| `/skill-probatica` | Analise probatoria simplificada, focada em um aspecto especifico | `/skill-probatica data/sentenca/<numero>/processo.txt` |
| `/tribunal-probatico` | Simula debate entre analistas sobre a forca das provas | `/tribunal-probatico data/sentenca/<numero>/processo.txt` |

### Revisao

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/pipeline-revisao-minuta` | Revisa uma minuta ja gerada, verificando calculos, honorarios, citacoes e remessa | `/pipeline-revisao-minuta data/sentenca/<numero>/<numero>-fundamentacao.md` |

### Download e conversao

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/baixar-pje` | Baixa processos do PJE (somente os PDFs) | `/baixar-pje 10 sentenca` |
| `/baixar-converter` | Baixa processos do PJE e ja converte os PDFs em texto | `/baixar-converter 5 sentenca` |
| `/baixar-inteligente` | Baixa processos selecionando automaticamente os documentos mais relevantes | `/baixar-inteligente 3 sentenca` |

### Utilidades

| Comando | O que faz | Exemplo de uso |
|---------|-----------|----------------|
| `/relatar-processo` | Gera linha do tempo e relatorio de um processo | `/relatar-processo data/sentenca/<numero>/processo.txt` |
| `/analisar-lista` | Analisa uma lista de julgamento do TRF5, gerando resumo de cada processo | `/analisar-lista lista-julgamento.pdf` |
| `/fork-terminal` | Executa multiplas tarefas em paralelo, cada uma em seu proprio terminal | `/fork-terminal "tarefa1" "tarefa2"` |

## Onde ficam os arquivos

Todos os arquivos do SuperJurista ficam organizados dentro da pasta `data/` no seu projeto:

```
data/
├── sentenca/                              # Processos aguardando sentenca
│   ├── 0814624-28.2019.4.05.8100/        # Pasta de um processo (numero CNJ)
│   │   ├── processo.txt                   # Texto extraido dos autos
│   │   ├── 0814624-(...)-relatorio.md     # Relatorio gerado
│   │   ├── 0814624-(...)-analise.md       # Analise juridica
│   │   └── 0814624-(...)-fundamentacao.md # Minuta de fundamentacao
│   └── 0005144-15.2026.4.05.8100/        # Outro processo
│       └── ...
├── decisao/                               # Processos aguardando decisao
│   └── ...                                # Mesma estrutura
└── processos/                             # Jurista Experiente (parecer e peticao)
    └── 0601122-24.2024.8.04.0001/
        ├── autos.pdf                      # PDF dos autos (entrada)
        ├── 0601122-(...).txt              # Texto extraido (OCR)
        ├── 0601122-(...)-linha-tempo.md   # Cronologia processual
        ├── 0601122-(...)-fase.md          # Fase e pecas cabiveis
        ├── 0601122-(...)-teses.md         # Mapa de teses
        ├── 0601122-(...)-pesquisa.md      # Jurisprudencia (JusMCP)
        ├── 0601122-(...)-prognostico.md   # Cenarios e probabilidades
        ├── 0601122-(...)-parecer.md       # PARECER consolidado
        ├── 0601122-(...)-minuta.md        # MINUTA da peca (revisao obrigatoria)
        └── 0601122-(...)-verificacao.md   # Verificacao de citacoes + score
```

Cada processo recebe sua propria pasta, nomeada pelo numero CNJ completo. Dentro dela, o arquivo `processo.txt` contem o texto extraido dos autos (gerado automaticamente na etapa de download e conversao). Os demais arquivos sao os artefatos gerados pelo sistema: relatorio, analise e fundamentacao.

Voce pode abrir qualquer um desses arquivos em um editor de texto ou no proprio Claude Code para revisar e ajustar o conteudo.

## Precisa de ajuda?

O SuperJurista esta em desenvolvimento ativo. Se encontrar algum problema ou tiver sugestoes:

- Abra um chamado em: https://github.com/thirsodelcorso/superjurista-dev/issues
- Descreva o problema encontrado, incluindo o comando executado e a mensagem de erro (se houver)

Novas funcionalidades e melhorias sao adicionadas regularmente. Mantenha o sistema atualizado para ter acesso as versoes mais recentes.
