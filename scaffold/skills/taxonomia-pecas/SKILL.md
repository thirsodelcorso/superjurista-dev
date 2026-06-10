---
name: taxonomia-pecas
description: >
  Use when you need to determine, deterministically, the procedural phase of a
  civil case and the petition that is admissible (cabível) at that phase, with the
  available defense/postulatory theses and the mandatory section/request checklist
  for each piece. Maps fase processual → peça cabível → teses → template DPE, and
  provides the completeness checklist used to validate a generated draft. Civil
  state-court (cível estadual), postulatory/defensorial perspective. Keywords:
  fase processual, peça cabível, tese, contestação, embargos, monitória, execução,
  cumprimento de sentença, impugnação, apelação, agravo, embargos de declaração,
  curadoria especial, negativa geral, checklist de peça, taxonomia.
metadata:
  author: super-jurista
  version: "1.0.0"
---

<identidade>
  <papel>Especialista em direito processual civil que mapeia, de forma determinística, a fase do processo, a peça cabível e as teses disponíveis na perspectiva postulatória/defensorial</papel>
  <dominio>Direito Processual Civil brasileiro (CPC/15), atuação da Defensoria Pública (curadoria especial, prerrogativas LC 80/94) e advocacia cível</dominio>
  <estilo>Determinístico e conservador — o cabimento de peça NÃO é decisão criativa; decorre de uma tabela e de condições verificadas nos autos. Erro de cabimento custa preclusão.</estilo>
</identidade>

<proposito>
  <objetivo>Fornecer a tabela de decisão fase→peça→teses→template e os checklists de seções/pedidos obrigatórios por peça, para que os agentes de estratégia e redação selecionem a peça correta e validem a completude da minuta</objetivo>
  <razao>A escolha da peça errada para a fase (ex.: embargos quando já cabe apelação) gera preclusão e perda de prazo. Por isso o cabimento é tratado como conhecimento determinístico, não como geração livre do LLM</razao>
  <resultado>Identificação da fase, lista de peças cabíveis, teses disponíveis por peça com suas condições de cabimento, template DPE correspondente e checklist de completude</resultado>
</proposito>

<quando_usar>
  <ativar_quando>
    - Um agente precisa decidir qual peça é cabível dada a fase do processo
    - Um agente de redação precisa do checklist de seções/pedidos obrigatórios de uma peça
    - É preciso listar as teses disponíveis e suas condições de cabimento
    - É preciso localizar o template DPE/genérico correspondente a uma peça/tese
  </ativar_quando>
</quando_usar>

<deteccao_fase>
  ## Detecção de fase por movimentos (heurística determinística)

  Examine os últimos movimentos/peças dos autos. Aplique a PRIMEIRA regra que casar,
  de baixo para cima na linha do tempo (a fase mais avançada prevalece):

  | Sinais nos movimentos/peças | Fase | Observação |
  |------------------------------|------|------------|
  | Cumprimento de sentença / penhora / avaliação / execução de título | execucao | fase mais avançada |
  | Sentença publicada + prazo recursal aberto | recursal | conferir tempestividade |
  | Acórdão publicado | recursal_superior | cabe ED, REsp/RE, contrarrazões |
  | Saneamento / especificação de provas / despacho "digam as partes" / laudo juntado | instrutoria | |
  | Citação + prazo de resposta aberto (sem sentença) | postulatoria | |
  | Apenas petição inicial / distribuição | postulatoria | autor; ou aguardando citação |

  ALERTAS de cabimento (sempre emitir quando aplicável):
  - Se já houve sentença → contestação/embargos à monitória NÃO cabem mais; verificar prazo de apelação.
  - Se já houve acórdão → apelação não cabe; verificar ED, REsp/RE.
  - Se citação por edital/hora certa ou réu incapaz sem representante → atuação por CURADORIA ESPECIAL (art. 72, II, CPC).
</deteccao_fase>

<tabela_fase_peca>
  ## Tabela fase → peça cabível → template

  | Fase | Peça cabível | Quem | Template |
  |------|--------------|------|----------|
  | postulatoria | Petição inicial (cobrança) | autor | peticao-inicial-cobranca.md |
  | postulatoria | Petição inicial (consumidor) | autor | peticao-inicial-consumidor.md |
  | postulatoria | Contestação (negativa geral) | réu / curador | contestacao-negativa-geral.md |
  | postulatoria | Contestação (novo endereço localizado) | curador | contestacao-novo-endereco.md |
  | postulatoria | Contestação genérica | réu | generico-contestacao.md |
  | postulatoria | Embargos à ação monitória | réu / curador | embargos-monitoria.md |
  | instrutoria | Manifestação / especificação de provas | qualquer | generico-manifestacao.md |
  | instrutoria | Manifestação "sem provas a produzir" (curadoria) | curador | sem-provas-curadoria.md |
  | recursal | Apelação (curadoria) | réu / curador | apelacao-curadoria.md |
  | recursal | Contrarrazões à apelação (curadoria) | réu / curador | contrarrazoes-apelacao-curadoria.md |
  | recursal | Agravo de instrumento (curadoria) | qualquer | agravo-instrumento-curadoria.md |
  | recursal | Informação de interposição de agravo | qualquer | info-interposicao-agravo-curadoria.md |
  | recursal | Embargos de declaração (curadoria) | qualquer | embargos-declaracao-curadoria.md |
  | recursal | Contrarrazões a embargos de declaração | qualquer | contrarrazoes-ed-curadoria.md |
  | recursal_superior | (decisão de não recorrer) Não contrarrazoar apelação | curador | nao-contrarrazoar-apelacao-curadoria.md |
  | execucao | Impugnação ao cumprimento de sentença (negativa geral) | executado / curador | impugnacao-cumprimento-negativa-geral.md |
  | execucao | Impugnação ao cumprimento (novo endereço) | curador | impugnacao-cumprimento-novo-endereco.md |
  | execucao | Embargos à execução (negativa geral) | executado / curador | embargos-execucao-negativa-geral.md |
  | execucao | Embargos à execução (novo endereço) | curador | embargos-execucao-novo-endereco.md |
  | execucao | (decisão de não embargar) Não embargar execução extrajudicial | curador | nao-embargar-execucao-extrajudicial-curadoria.md |
  | execucao | (decisão de não embargar) Não embargar — justiça gratuita CadÚnico | curador | nao-embargar-justica-gratuita-cadunico-curadoria.md |
  | execucao | Impugnação à penhora (curatela) | executado / curador | impugnacao-penhora-curatela.md |
  | execucao | Impugnação à penhora (impenhorabilidade 40 salários) | executado / curador | impugnacao-penhora-40-salarios-curatela.md |
  | execucao | Suspensão do cumprimento de sentença (curadoria) | curador | suspensao-cumprimento-sentenca-curadoria.md |
  | qualquer | Intimação por edital | — (ato) | intimacao-por-edital.md |

  Os arquivos ficam em `knowledge/templates/` (no projeto instalado: `.claude/skills/taxonomia-pecas/../../knowledge/templates/` ou conforme o orquestrador injetar).
</tabela_fase_peca>

<tabela_teses>
  ## Teses disponíveis, condições de cabimento e fundamento

  | Tese (id) | Peça | Condição de cabimento (verificar nos autos) | Fundamento legal |
  |-----------|------|----------------------------------------------|------------------|
  | nulidade_citacao_edital | embargos/contestação | citação por edital sem esgotamento de diligências de localização | art. 256, §§1º-3º, e art. 282, CPC |
  | prescricao_lei_14195 | embargos/contestação | crédito sujeito a prazo trienal; lapso temporal ultrapassado | art. 206-A, CC (Lei 14.195/2021); art. 487, II, CPC |
  | prescricao_cpc15 | embargos/contestação | obrigação pessoal; >5 anos sem interrupção válida | art. 206, §5º, I, CC; art. 487, II, CPC |
  | prescricao_cpc73 | embargos/contestação | obrigação anterior; aplicável regime pretérito | arts. 205-206, CC; art. 487, II, CPC |
  | justica_gratuita_cadunico | qualquer | hipossuficiência; inscrição no CadÚnico nos autos | arts. 98-99, §3º, CPC; art. 5º, LXXIV, CF |
  | negativa_geral | contestação/impugnação (curadoria) | atuação como curador especial | art. 341, parágrafo único, e art. 72, II, CPC |
  | impugnacao_documental | embargos à monitória | insuficiência/iliquidez do documento da inicial | art. 700, §§1º-2º, e art. 702, CPC |
  | impenhorabilidade_40_salarios | impugnação à penhora | penhora de poupança/valores até 40 salários mínimos | art. 833, X, CPC |
  | excesso_execucao | impugnação/embargos à execução | valor cobrado superior ao devido | art. 525, §1º, V, e art. 917, III, CPC |
  | novo_endereco_localizado | contestação/impugnação (curadoria) | curador localiza endereço atual do assistido | dever de comunicação; art. 72, II, CPC |

  REGRA DE OURO (curadoria especial): a tese `negativa_geral` é SEMPRE incluída como fechamento,
  ainda que se sustentem teses específicas. A `justica_gratuita_cadunico` é incluída sempre que houver
  indício de hipossuficiência nos autos.
</tabela_teses>

<checklists_completude>
  ## Checklists de seções/pedidos obrigatórios por peça (validação de completude)

  Toda peça (seções comuns):
  - [ ] Endereçamento ao juízo competente (vara + comarca/UF)
  - [ ] Identificação do processo (número CNJ) e das partes
  - [ ] Qualificação da atuação (parte / curador especial — art. 72, II, CPC)
  - [ ] Fundamentação por IRAC para cada tese
  - [ ] Pedidos específicos correspondentes a cada tese sustentada
  - [ ] Data e assinatura
  - [ ] (Defensoria) Fechamento com prerrogativas LC 80/94

  Petição inicial — adicional (art. 319, CPC):
  - [ ] Fatos / Direito / Pedidos / Valor da causa / Provas
  - [ ] Requerimento de citação do réu

  Contestação / Embargos à monitória — adicional:
  - [ ] Preliminares (se houver) antes do mérito (art. 337, CPC)
  - [ ] (curadoria) negativa geral (art. 341, parágrafo único, CPC)
  - [ ] Pedido de improcedência + sucumbência

  Apelação / Agravo — adicional:
  - [ ] Demonstração de tempestividade e cabimento
  - [ ] Pedido de reforma/anulação com o que se pretende
  - [ ] (apelação) razões recursais separadas da petição de interposição

  Impugnação ao cumprimento / Embargos à execução — adicional:
  - [ ] Matéria admissível na via (art. 525, §1º / art. 917, CPC)
  - [ ] (excesso) memória de cálculo do valor que entende devido (art. 525, §4º-5º, CPC)
  - [ ] (impenhorabilidade) indicação do bem/valor e fundamento (art. 833, CPC)
</checklists_completude>

<restricoes>
  - NUNCA recomendar peça incompatível com a fase detectada — emitir o alerta de cabimento.
  - NUNCA afirmar que uma tese é cabível sem verificar sua condição nos autos.
  - SEMPRE incluir negativa geral e gratuidade quando o usuário atua em curadoria especial.
  - SEMPRE usar português com acentos corretos.
</restricoes>
