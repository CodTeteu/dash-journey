# Mapeamento do Macrofluxo Journey

Documento de apoio para validar com a diretoria da area como as etapas, areas e indicadores do painel estao sendo interpretados no Power BI.

## Objetivo

Este documento compara tres visoes:

1. O macrofluxo operacional enviado na imagem.
2. Os dados reais existentes no modelo Power BI.
3. A forma como o visual principal esta apresentando essas etapas hoje.

A ideia e facilitar a conversa com o diretor da area, separando o que ja esta mapeado, o que esta agrupado, o que aparece apenas no detalhe e o que precisa de decisao de negocio.

## Fontes analisadas

### Modelo Power BI

Projeto: `Jobs_Produtos.pbip`

Modelo semantico: `Jobs_Produtos.SemanticModel/definition`

Tabelas principais usadas no visual:

| Tabela | Uso no painel | Colunas relevantes |
|---|---|---|
| `vw_powerbi_journeys` | Journeys pai | `CODIGO`, `JOB`, `DATA_CADASTRO`, `FIM` |
| `vw_powerbi_journeys_filhos_novos` | Jobs filhos, produtos, empresas e financeiro | `CODIGO`, `JOB`, `DATA_CADASTRO`, `CODIGO_PRODUTO`, `GppNome`, `FIM`, `ParCNPJ`, `PERCENTUAL`, `SIMULACAO_CREDITO`, `SIMULACAO_HONORARIO`, `CREDITOS`, `Honorarios` |
| `vw_powerbi_journeys_menor_area_novos` | Etapas/areas dos jobs | `PmvJob`, `PmvArea`, `ParNome` |

### Contagens gerais consultadas via MCP

| Tabela | Linhas |
|---|---:|
| `vw_powerbi_journeys` | 1.613 |
| `vw_powerbi_journeys_filhos_novos` | 3.110 |
| `vw_powerbi_journeys_menor_area_novos` | 3.086 |

## Regras tecnicas atuais do painel

| Tema | Regra atual |
|---|---|
| Modelo | Sem relacionamentos fisicos; os filtros sao resolvidos via DAX dentro das medidas HTML. |
| Periodo | O slicer de periodo usa `dCalendar`; os dados sao filtrados por `DATA_CADASTRO`. |
| Journey x Job | O prefixo de `vw_powerbi_journeys_filhos_novos[JOB]` e comparado com `vw_powerbi_journeys[JOB]`. Exemplo: filho `86398-FTX` pertence ao pai `86398`. |
| Ativo | `FIM = 1` significa ativo. |
| Empresas | Usa CNPJs unicos por produto via `ParCNPJ`. |
| Etapas | Usa `vw_powerbi_journeys_menor_area_novos[ParNome]`. |
| Area | Usa `vw_powerbi_journeys_menor_area_novos[PmvArea]`. |
| Financeiro PJ360 | Colunas com prefixo `SIMULACAO`: `SIMULACAO_CREDITO`, `SIMULACAO_HONORARIO`. |
| Financeiro PROJECT | Colunas sem prefixo `SIMULACAO`: `CREDITOS`, `Honorarios`. |
| Honorarios PROJECT | Aplica desconto: `Honorarios * (1 - PERCENTUAL / 100)`. |
| QTD financeiro | Conta jobs com valor preenchido/maior que zero. |
| `RF_VALOR` | Existe no modelo, mas nao esta sendo usado no visual atual. |

## Macrofluxo da imagem x dados reais

A imagem enviada apresenta 10 etapas principais:

1. Entrada do Job
2. Cadastro
3. Procuracao Eletronica
4. Checklist
5. Operacao
6. Revisao
7. Reuniao Tecnica
8. Negociacao
9. Retificacao
10. Fim

Comparacao com o modelo:

| Ordem na imagem | Etapa da imagem | Existe no modelo como `ParNome`? | `PmvArea` encontrada | Linhas no modelo | Observacao |
|---:|---|---|---|---:|---|
| 1 | Entrada do Job | Nao | - | - | Nao existe como etapa real em `ParNome`. Pode ser conceito operacional anterior ao cadastro ou pode ser inferida por jobs sem area, se a diretoria confirmar. |
| 2 | Cadastro | Sim | `1` | 93 | Etapa real existente. |
| 3 | Procuracao Eletronica | Sim | `1,3` | 1.106 | Etapa real existente. Hoje esta agrupada com Cadastro na coluna `Onboarding`. |
| 4 | Checklist | Sim | `2` | 437 | Etapa real existente. |
| 5 | Operacao | Sim | `5` | 282 | Etapa real existente. |
| 6 | Revisao | Sim | `5,2` | 61 | Etapa real existente. Hoje nao aparece como coluna propria na linha principal; aparece no expandido. |
| 7 | Reuniao Tecnica | Sim | `6` | 194 | Etapa real existente. Hoje nao aparece como coluna propria na linha principal; aparece no expandido. |
| 8 | Negociacao | Sim | `8,5` | 5 | Etapa real existente. |
| 9 | Retificacao | Sim | `8,8` | 5 | Etapa real existente. Hoje nao aparece como coluna propria na linha principal; aparece no expandido. |
| 10 | Fim | Sim | `15` | 900 | Etapa real existente. |

## Etapas extras encontradas no modelo

O modelo tambem possui etapas que nao aparecem no macrofluxo da imagem:

| Etapa extra | `PmvArea` | Linhas | Observacao para validar |
|---|---|---:|---|
| Compensacao | `13` | 2 | Confirmar se deve aparecer no painel principal, apenas no expandido ou ser agrupada em outra etapa. |
| Qualidade | `8,6` | 1 | Confirmar se deve aparecer no painel principal, apenas no expandido ou ser agrupada em outra etapa. |

## Como o visual principal esta hoje

O visual principal tem uma tabela resumida por produto. As etapas nao estao todas abertas em colunas separadas; algumas estao agrupadas ou aparecem somente quando o produto e expandido.

### Colunas atuais da linha principal

| Coluna atual | Conteudo real |
|---|---|
| Produto | Produto do job, vindo de `GppNome`. |
| Jobs | Total de jobs e total de jobs ativos. |
| Empresas | CNPJs unicos em `ParCNPJ`; abaixo, CNPJs unicos nos jobs ativos. |
| Onboarding | Agrupa `CADASTRO` + `PROCURAÇÃO ELETRÔNICA`. |
| Checklist | `CHECKLIST`. |
| Operacao | `OPERAÇÃO`. |
| Negociacao | `NEGOCIAÇÃO`. |
| Implantacao | Coluna preparada para `IMP`, `IMPLANTAÇÃO`, `IMPLANTACAO` ou valores iniciados por `IMP`; hoje nao ha etapa real correspondente no MCP. |
| Fim | `FIM`. |
| Creditos | Somente PROJECT: `CREDITOS`. |
| Honorarios | Somente PROJECT: `Honorarios * (1 - PERCENTUAL / 100)`. |

### Etapas que aparecem no expandido, mas nao como coluna propria na linha principal

| Etapa | Status atual |
|---|---|
| Revisao | Aparece no expandido. |
| Reuniao Tecnica | Aparece no expandido. |
| Retificacao | Aparece no expandido. |
| Compensacao | Aparece no expandido. |
| Qualidade | Aparece no expandido. |

## O que significa dizer que esta clusterizado

O visual principal nao esta necessariamente perdendo informacao, mas esta resumindo algumas etapas.

Exemplo principal:

| Coluna visual | Etapas reais dentro dela |
|---|---|
| Onboarding | `CADASTRO` + `PROCURAÇÃO ELETRÔNICA` |

Ja outras etapas reais existem no modelo e no expandido, mas nao aparecem como coluna propria na linha principal.

Portanto, existem tres niveis:

| Nivel | Descricao |
|---|---|
| Linha principal | Visao executiva resumida, com poucas colunas. |
| Expandido do produto | Visao mais detalhada com todas as etapas reais encontradas. |
| Modelo/MCP | Fonte completa das etapas e areas em `ParNome` e `PmvArea`. |

## Possivel novo layout de etapas para a tabela principal

Se a diretoria quiser que a tabela principal siga mais fielmente o macrofluxo da imagem, uma opcao seria trocar as colunas de etapa para:

| Ordem sugerida | Coluna sugerida | Base de calculo |
|---:|---|---|
| 1 | Cadastro | `ParNome = CADASTRO` |
| 2 | Procuracao Eletronica | `ParNome = PROCURAÇÃO ELETRÔNICA` |
| 3 | Checklist | `ParNome = CHECKLIST` |
| 4 | Operacao | `ParNome = OPERAÇÃO` |
| 5 | Revisao | `ParNome = REVISÃO` |
| 6 | Reuniao Tecnica | `ParNome = REUNIÃO TÉCNICA` |
| 7 | Negociacao | `ParNome = NEGOCIAÇÃO` |
| 8 | Retificacao | `ParNome = RETIFICAÇÃO` |
| 9 | Implantacao | Manter preparada para `IMP/IMPLANTAÇÃO`, se a area confirmar que e uma etapa valida fora do macrofluxo atual. |
| 10 | Fim | `ParNome = FIM` |

Observacao: essa opcao aumentaria a largura da tabela. Pode exigir reduzir fontes, largura de colunas ou manter algumas etapas apenas no expandido.

## Entrada do Job

`ENTRADA DO JOB` aparece na imagem, mas nao existe como `ParNome` na tabela de areas.

Foram encontrados jobs sem linha em `vw_powerbi_journeys_menor_area_novos`:

| Recorte | Jobs sem area |
|---|---:|
| Base total | 146 |
| Abril/2026 | 38 |

Ponto de decisao:

| Opcao | Como ficaria | Risco |
|---|---|---|
| Nao mostrar Entrada do Job | Mantem apenas etapas reais do modelo. | Pode faltar uma etapa esperada pela diretoria. |
| Mostrar Entrada do Job como jobs sem area | Conta jobs que existem em `vw_powerbi_journeys_filhos_novos`, mas nao possuem linha em `vw_powerbi_journeys_menor_area_novos`. | Pode misturar jobs realmente sem area com jobs que estao em outra situacao operacional. |
| Solicitar campo especifico na origem | Criar/usar uma etapa real para Entrada do Job na fonte. | Depende de ajuste na base/origem. |

Recomendacao: validar com o diretor se `ENTRADA DO JOB` deve ser uma etapa contavel no painel ou apenas uma etapa conceitual do processo.

## Implantacao

A coluna `IMPLANTAÇÃO` foi mantida no visual porque ja foi definida como etapa valida para o negocio.

Porem, no MCP atual nao existem linhas com:

- `IMP`
- `IMPLANTAÇÃO`
- `IMPLANTACAO`

Ponto de decisao:

| Pergunta | Motivo |
|---|---|
| Implantacao deve continuar como coluna na linha principal? | Hoje ela tende a mostrar zero enquanto a origem nao trouxer essa etapa. |
| Implantacao deve ser considerada fora do macrofluxo da imagem? | A imagem nao apresenta implantacao como uma das 10 etapas. |
| Alguma etapa atual deveria ser renomeada para Implantacao? | Se sim, precisamos saber qual `ParNome` ou `PmvArea` representa isso. |

## Financeiro

### Cards superiores

Os cards financeiros superiores mostram apenas PROJECT:

| Card | Regra |
|---|---|
| Total Creditos | `CREDITOS` |
| Total Honorarios | `Honorarios * (1 - PERCENTUAL / 100)` |

### Colunas da tabela principal

Na tabela principal, as colunas financeiras mostram apenas PROJECT:

| Coluna | Regra |
|---|---|
| Creditos | `CREDITOS` |
| Honorarios | `Honorarios * (1 - PERCENTUAL / 100)` |

### Matriz expandida

No expandido do produto, a matriz separa PJ360 e PROJECT:

| Bloco | Valor | QTD |
|---|---|---|
| PJ360 Credito | `SIMULACAO_CREDITO` | Jobs com `SIMULACAO_CREDITO > 0` |
| PJ360 Honorarios | `SIMULACAO_HONORARIO` | Jobs com `SIMULACAO_HONORARIO > 0` |
| PROJECT Credito | `CREDITOS` | Jobs com `CREDITOS > 0` |
| PROJECT Honorarios | `Honorarios * (1 - PERCENTUAL / 100)` | Jobs com `Honorarios > 0` |

Observacao importante: `RF_VALOR` existe no modelo, mas nao esta sendo usado.

## Perguntas para alinhar com o diretor

Use esta lista para bater rapidamente as decisoes.

### Sobre etapas

1. A tabela principal deve continuar resumida ou deve seguir exatamente o macrofluxo da imagem?
2. `Cadastro` e `Procuracao Eletronica` devem ficar separados ou podem continuar agrupados em `Onboarding`?
3. `Revisao`, `Reuniao Tecnica` e `Retificacao` devem virar colunas principais?
4. `Compensacao` e `Qualidade` devem aparecer como colunas, ficar apenas no expandido ou serem agrupadas em outra etapa?
5. `Entrada do Job` deve ser contada no painel?
6. Se sim, `Entrada do Job` pode ser definida como jobs sem area, ou precisa vir de um campo especifico da origem?
7. `Implantacao` deve continuar como coluna mesmo nao aparecendo na imagem e sem dados atuais no MCP?

### Sobre areas

1. O painel deve exibir apenas `ParNome` ou tambem deve mostrar `PmvArea`?
2. Os codigos compostos de area, como `1,3`, `5,2`, `8,5`, `8,6`, `8,8`, devem aparecer para o usuario ou ficar apenas em tooltip/documentacao?
3. O diretor quer validar a etapa pelo nome (`ParNome`) ou pelo codigo de area (`PmvArea`)?

### Sobre financeiro

1. Cards superiores devem continuar mostrando PJ360 + PROJECT juntos?
2. Tabela principal deve continuar mostrando apenas PROJECT em Creditos e Honorarios?
3. Matriz expandida PJ360 x PROJECT esta clara para a diretoria?
4. `RF_VALOR` deve continuar fora ou representa alguma quantidade/valor que precisa entrar depois?

## Recomendacao pratica

Para uma conversa executiva, a recomendacao e levar duas opcoes:

### Opcao A - Manter a tabela principal executiva

Manter a linha principal resumida e deixar todas as etapas detalhadas no expandido.

Vantagens:

- Mantem o visual mais limpo.
- Evita tabela muito larga.
- Bom para diretoria que quer leitura rapida por produto.

Pontos de atencao:

- Algumas etapas importantes ficam escondidas no expandido.
- `Onboarding` mistura `Cadastro` e `Procuracao Eletronica`.

### Opcao B - Abrir as etapas principais na linha

Separar as colunas conforme macrofluxo:

`Cadastro`, `Procuracao Eletronica`, `Checklist`, `Operacao`, `Revisao`, `Reuniao Tecnica`, `Negociacao`, `Retificacao`, `Implantacao`, `Fim`.

Vantagens:

- Fica mais parecido com o macrofluxo da imagem.
- Facilita comparar produto por produto em cada etapa.
- Reduz dependencia do expandido.

Pontos de atencao:

- A tabela fica mais larga.
- Talvez precise reduzir tamanho de fonte ou reorganizar colunas financeiras.
- `Entrada do Job`, `Compensacao` e `Qualidade` ainda precisam de decisao.

## Resumo para falar com o diretor

O painel ja usa as etapas reais da base, mas a linha principal esta resumida. O detalhe expandido mostra todas as etapas reais encontradas no MCP. A imagem ajuda a validar a ordem e os nomes esperados do macrofluxo. As principais decisoes pendentes sao:

1. Separar ou manter agrupado `Cadastro + Procuracao Eletronica`.
2. Adicionar `Revisao`, `Reuniao Tecnica` e `Retificacao` na linha principal.
3. Definir o tratamento de `Entrada do Job`.
4. Definir o tratamento de `Compensacao` e `Qualidade`.
5. Confirmar se `Implantacao` continua como coluna mesmo nao aparecendo no macrofluxo da imagem e sem dados atuais.
