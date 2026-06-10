# Template: Embargos à Ação Monitória — 17ª DPCív / DPEAM

## Metadados
- **Tipo**: embargos_monitoria
- **Órgão**: Defensoria Pública do Estado do Amazonas — 17ª Defensoria Pública Cível (17ª DPCív)
- **Comarca**: Manaus — AM
- **Legitimidade**: Curador Especial nomeado nos termos do art. 72, II, do CPC
- **Prazo**: 15 dias úteis da nomeação (art. 700, § 6º c/c art. 72, II, do CPC)
- **Legislação principal**: CPC/15 (Lei 13.105/2015) arts. 72 II, 341 par. único, 700–702,
  921–924; LC 80/1994; LCE 01/1990

---

## CABEÇALHO PADRÃO (fixo — sempre igual)

```
AO JUÍZO DA {vara} DA COMARCA DE MANAUS - AMAZONAS

Processo nº {numero_processo}

Curadoria Especial

A DEFENSORIA PÚBLICA DO ESTADO DO AMAZONAS, através do órgão de atuação signatário,
exercendo a curadoria especial de {nome_assistido}, vem, nos termos do art. 72 do CPC,
apresentar EMBARGOS À AÇÃO MONITÓRIA POR NEGATIVA GERAL.
```

---

## TESES DISPONÍVEIS

O agente analisa os dados extraídos do processo e inclui apenas as teses aplicáveis,
na ordem abaixo. A TESE 6 (Negativa Geral) é sempre incluída.

---

### TESE 1 — NULIDADE DA CITAÇÃO POR EDITAL
**Condição de aplicação**: INFOSEG ou outro sistema público (Receita Federal, TSE,
DETRAN) indicou endereço diverso daquele utilizado nas diligências do oficial de justiça,
demonstrando que o réu era localizável e a citação ficta foi prematura.

**Variável**: `{novo_endereco}`

---

**PRELIMINAR — DA NULIDADE DA CITAÇÃO POR EDITAL**

A citação por edital constitui modalidade ficta e excepcional de comunicação processual,
somente cabível quando esgotados todos os meios ordinários de localização do réu, conforme
expressamente exige o art. 256 do Código de Processo Civil:

> *"Art. 256. A citação por edital será feita quando desconhecido ou incerto o citando,
> quando ignorado, incerto ou inacessível o lugar em que se encontrar ou nos casos
> expressos em lei."*

A excepcionalidade da medida impõe ao juízo e ao autor rigoroso dever de diligência prévia:
não basta a mera afirmação de desconhecimento do paradeiro do réu; é necessária a
demonstração concreta e documentada de que foram realizadas buscas nos cadastros públicos
disponíveis — Receita Federal, DETRAN, cartórios de registro de imóveis, sistemas de
segurança pública (INFOSEG/SINESP) — antes de se recorrer à citação editalícia.

Ocorre que, consultado o sistema INFOSEG/SINESP, localizou-se endereço atualizado do
assistido desta Curadoria, não coincidente com os locais onde as diligências foram realizadas,
a saber: **{novo_endereco}**.

A existência de endereço localizável por meio de sistema público de acesso franqueado ao
Poder Judiciário demonstra, de forma inequívoca, que o réu não era incerto, desconhecido
nem inacessível. Restou configurada, portanto, a nulidade da citação por edital realizada
nos presentes autos, nos termos do art. 280 do CPC, que determina a nulidade de todos os
atos processuais subsequentes ao ato viciado.

Nesse sentido, o Superior Tribunal de Justiça consolidou entendimento de que a citação
por edital pressupõe o esgotamento efetivo das diligências de localização, sendo nula
quando realizada sem que o exequente tenha se valido dos meios disponíveis para encontrar
o réu (STJ, REsp 1.277.551/RJ; AgInt no AREsp 1.489.401/SP).

Requer-se, portanto, a declaração de nulidade da citação por edital e de todos os atos
processuais a ela subsequentes, com a determinação de citação pessoal do réu no endereço
encontrado: **{novo_endereco}**.

---

### TESE 2 — JUSTIÇA GRATUITA
**Condição de aplicação**: cadastro do assistido no CadÚnico do Governo Federal foi
identificado nos autos ou informado pela Defensoria, caracterizando hipossuficiência econômica.

---

**DA JUSTIÇA GRATUITA**

O assistido desta Curadoria encontra-se inscrito no Cadastro Único para Programas Sociais
do Governo Federal (CadÚnico), instrumento de identificação e caracterização
socioeconômica das famílias de baixa renda, destinado à seleção de beneficiários de
programas federais de transferência de renda.

A inscrição no CadÚnico constitui presunção relativa de hipossuficiência econômica, apta
a autorizar a concessão dos benefícios da justiça gratuita independentemente de maiores
comprovações, na esteira do entendimento do Superior Tribunal de Justiça, segundo o qual
a declaração de pobreza goza de presunção juris tantum de veracidade (STJ, AgInt no
AREsp 1.801.994/SP).

Nos termos do art. 98 do Código de Processo Civil, a pessoa natural ou jurídica, brasileira
ou estrangeira, com insuficiência de recursos para pagar as custas, as despesas processuais
e os honorários advocatícios tem direito à gratuidade da justiça, na forma da lei.

Requer-se, portanto, a concessão dos benefícios da justiça gratuita ao assistido desta
Curadoria, com isenção de custas, despesas processuais e honorários advocatícios, nos
termos do art. 98 e seguintes do CPC.

---

### TESE 3 — PRESCRIÇÃO INTERCORRENTE (regime CPC/73)
**Condição de aplicação**: a ação monitória foi ajuizada sob a vigência do CPC/73
(anterior a 18/03/2016) e o processo ficou paralisado por inércia do autor após
`{data_fim_suspensao}`, superando o prazo prescricional de `{prazo_prescricional}`.

**Variáveis**: `{data_fim_suspensao}`, `{prazo_prescricional}`

---

**DA PRESCRIÇÃO INTERCORRENTE — REGIME DO CPC/73**

A presente ação monitória foi distribuída na vigência do Código de Processo Civil de 1973,
motivo pelo qual as regras relativas à prescrição intercorrente devem ser analisadas à
luz do regime então vigente, aplicando-se o direito intertemporal.

Sob a égide do CPC/73, a citação válida interrompia a prescrição (art. 219), cujos efeitos
retroagiam à data da propositura da ação (art. 219, § 1º). Todavia, a interrupção operava
uma única vez, sendo que, após o reinício do prazo, sua fluência não era obstada pela
simples tramitação do processo.

No caso em exame, após **{data_fim_suspensao}**, o feito permaneceu paralisado por
inércia exclusiva da parte autora, sem que fossem realizados atos processuais aptos a
impulsionar o andamento do processo. Desde então, transcorreu integralmente o prazo
prescricional de **{prazo_prescricional}** aplicável à espécie, sem qualquer causa
interruptiva ou suspensiva.

O Superior Tribunal de Justiça, ao apreciar a Súmula 150/STF — segundo a qual prescreve
a execução no mesmo prazo da ação — estendeu o entendimento à fase de conhecimento
paralisada por desídia do autor, reconhecendo a incidência da prescrição intercorrente
ainda sob o regime do CPC/73, quando demonstrada a inércia do credor por lapso temporal
equivalente ao prazo prescricional da pretensão (STJ, REsp 1.604.412/SC, Tema 566).

Configurada a prescrição intercorrente, impõe-se a extinção do processo com resolução
do mérito, nos termos do art. 487, II, do CPC/15, aplicável por força do art. 1.046
do mesmo diploma.

---

### TESE 4 — PRESCRIÇÃO INTERCORRENTE (CPC/15 — redação original, antes de 26/08/2021)
**Condição de aplicação**: a ação tramita sob o CPC/15 em sua redação original
(entre 18/03/2016 e 25/08/2021); houve decisão de suspensão por não localização
do executado em **{data_decisao_suspensao}** e o prazo prescricional encerrou-se
em **{data_fim_prescricao}** sem que o processo fosse reativado.

**Variáveis**: `{data_decisao_suspensao}`, `{data_fim_prescricao}`

---

**DA PRESCRIÇÃO INTERCORRENTE — CPC/15, REDAÇÃO ORIGINAL**

O Código de Processo Civil de 2015, em sua redação original, disciplinou expressamente
a prescrição intercorrente na fase de cumprimento de sentença e no processo de execução,
nos §§ 1º a 4º do art. 921:

> *"Art. 921. [...] § 1º Na hipótese do inciso III, o juiz suspenderá a execução pelo
> prazo de 1 (um) ano, durante o qual se suspenderá a prescrição. § 2º Decorrido o prazo
> máximo de 1 (um) ano sem que seja localizado o executado ou que sejam encontrados bens
> penhoráveis, o juiz ordenará o arquivamento dos autos. § 3º Os autos serão
> desarquivados para prosseguimento da execução se a qualquer tempo forem encontrados
> bens penhoráveis. § 4º Decorrido o prazo de que trata o § 1º sem manifestação do
> exequente, começa a correr o prazo de prescrição intercorrente."*

No presente caso, a suspensão do processo foi determinada em **{data_decisao_suspensao}**,
iniciando-se o prazo de um ano de suspensão da prescrição. Findo esse prazo, sem
localização do executado e sem manifestação do exequente, passou a correr o prazo de
prescrição intercorrente, que se encerrou em **{data_fim_prescricao}**, sem que a parte
autora houvesse promovido qualquer ato de impulso processual.

O Superior Tribunal de Justiça, ao apreciar o Tema Repetitivo 566, fixou a tese de que,
após o transcurso do prazo de suspensão de um ano, inicia-se automaticamente o prazo de
prescrição intercorrente, independentemente de intimação do credor (STJ, REsp 1.604.412/SC,
j. 22/05/2018, DJe 02/08/2018).

Configurada a prescrição intercorrente, requer-se a extinção do processo com resolução
do mérito, nos termos do art. 924, V, do CPC/15.

---

### TESE 5 — PRESCRIÇÃO INTERCORRENTE (a partir de 26/08/2021 — Lei 14.195/2021)
**Condição de aplicação**: o processo tramita sob o regime da Lei 14.195/2021
(vigente a partir de 26/08/2021), que alterou os §§ 1º a 5º do art. 921 do CPC/15,
tornando mais objetiva a contagem da prescrição intercorrente. O prazo prescricional
aplicável ao `{tipo_titulo}` é de `{prazo_prescricional}`, o ajuizamento ocorreu em
`{data_ajuizamento}` e o edital foi publicado em `{data_publicacao_edital}`.

**Variáveis**: `{tipo_titulo}`, `{prazo_prescricional}`, `{data_ajuizamento}`,
`{data_publicacao_edital}`

---

**DA PRESCRIÇÃO INTERCORRENTE — LEI 14.195/2021**

A Lei nº 14.195, de 26 de agosto de 2021, conferiu nova redação ao art. 921 do Código de
Processo Civil, aprimorando o regramento da prescrição intercorrente e tornando sua
incidência mais objetiva. O § 1º passou a dispor que, decorrido o prazo de suspensão
de um ano sem localização do executado ou de bens penhoráveis, inicia-se de pleno direito
o prazo de prescrição intercorrente, com fluência simultânea ao arquivamento dos autos.

Na espécie, trata-se de {tipo_titulo}, cuja pretensão de cobrança sujeita-se ao prazo
prescricional de **{prazo_prescricional}**. A ação foi ajuizada em **{data_ajuizamento}**
e a citação por edital foi publicada em **{data_publicacao_edital}**.

Considerando a data de ajuizamento, o tempo de tramitação sem impulso efetivo do autor
e a ausência de atos interruptivos da prescrição após a publicação do edital, verifica-se
que transcorreu integralmente o prazo de **{prazo_prescricional}** sem qualquer ato apto
a interromper a prescrição intercorrente.

Nos termos da nova redação do § 5º do art. 921 do CPC, o juiz, ao reconhecer a prescrição
intercorrente, deverá ouvi o exequente previamente — providência que se requer seja
observada — e, constatada a inércia, declarar extinto o processo com resolução do mérito,
nos termos do art. 924, V, do CPC/15.

---

### TESE 6 — NEGATIVA GERAL (sempre presente — fallback obrigatório)
**Condição de aplicação**: sempre incluída, independentemente das demais teses. Quando
for a única tese aplicável, constitui o fundamento principal dos embargos.

---

**DA NEGATIVA GERAL**

O Curador Especial é beneficiado com a isenção do ônus de impugnação especificada
exatamente porque não tem contato com o curatelado e está impossibilitado de contrariar
cada um dos fatos deduzidos na inicial como fundamento da pretensão autoral.

Assim, por imposição legal e não obstante as afirmativas da parte Autora, impugna-se
todos os fatos alegados na inicial, tornando-os controvertidos, conforme art. 341,
parágrafo único do CPC.

---

## PEDIDOS

Montar conforme as teses aplicadas. Pedidos fixos são sempre incluídos.

### Pedidos fixos (sempre presentes)
```
Ante o exposto, requer:

a) O recebimento e provimento dos presentes Embargos à Ação Monitória;

[INSERIR PEDIDOS ESPECÍFICOS POR TESE — ver abaixo]

[PENÚLTIMO] A condenação da parte embargada ao pagamento de honorários sucumbenciais,
nos termos do art. 85 do CPC;

[ÚLTIMO] A total improcedência da ação monitória, com a extinção do processo nos
termos que a lei autorizar.
```

### Pedidos por tese
| Tese aplicada | Pedido específico a inserir |
|---|---|
| TESE 1 | A declaração de nulidade da citação por edital e de todos os atos processuais subsequentes (art. 280 do CPC), determinando-se a citação pessoal do réu no endereço: {novo_endereco}; |
| TESE 2 | A concessão dos benefícios da justiça gratuita ao assistido desta Curadoria, nos termos do art. 98 do CPC; |
| TESE 3 | O reconhecimento da prescrição intercorrente e a extinção do processo com resolução do mérito, nos termos do art. 487, II, do CPC; |
| TESE 4 | O reconhecimento da prescrição intercorrente e a extinção do processo com resolução do mérito, nos termos do art. 924, V, do CPC; |
| TESE 5 | O reconhecimento da prescrição intercorrente, na forma do art. 921, § 5º, do CPC (redação da Lei 14.195/2021), com a extinção do processo nos termos do art. 924, V, do CPC; |

---

## RODAPÉ PADRÃO (fixo — sempre igual)

```
Em tempo, reitera-se necessidade de observância às prerrogativas legais da Defensoria
Pública, especialmente atuação independente de mandato, intimação pessoal com a
integralidade do processo (art. 4º, § 2º do Provimento n.º 143/2008 da CGJ) e contagem
de prazo em dobro, nos termos do art. 128 da LC 80/94 e do art. 34 da LCE 01/90.

Espera deferimento.

Manaus, data registrada no sistema.
```

---

## Variáveis do template

| Variável | Fonte | Tese |
|---|---|---|
| `{vara}` | NER — COURT_DIVISION | Cabeçalho |
| `{numero_processo}` | NER — CASE_NUMBER | Cabeçalho |
| `{nome_assistido}` | NER — EMBARGANTE_NOME | Cabeçalho |
| `{novo_endereco}` | INFOSEG / dado informado | Tese 1 |
| `{data_fim_suspensao}` | NER — DATE / dado informado | Tese 3 |
| `{prazo_prescricional}` | Calculado conforme tipo de título | Teses 3, 5 |
| `{data_decisao_suspensao}` | NER — DATE / dado informado | Tese 4 |
| `{data_fim_prescricao}` | Calculado: data_decisao_suspensao + 1 ano + prazo prescricional | Tese 4 |
| `{tipo_titulo}` | NER — TITULO_TYPE | Tese 5 |
| `{data_ajuizamento}` | NER — DATE | Tese 5 |
| `{data_publicacao_edital}` | NER — DATE / EDITAL_CITATION | Tese 5 |

## Prazos prescricionais por tipo de título
- Cheque: 6 meses para apresentação + 2 anos para ação cambial; após exaurimento: 5 anos (Súmula 503/STJ)
- Nota promissória: 3 anos (art. 70 do Decreto 57.663/1966)
- Duplicata: 3 anos da data do vencimento (art. 18 da Lei 5.474/1968)
- Contrato em geral: 5 anos (art. 206, § 5º, I, CC) ou 3 anos (art. 206, § 3º, VIII, CC)
- Cédula de crédito bancário: 5 anos (art. 206, § 5º, I, CC)
