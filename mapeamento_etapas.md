# Mapeamento de Áreas Operacionais para Etapas do Dashboard

Este documento registra a análise técnica de cobertura e mapeamento entre a coluna original `JOBS_PRODUTO_AREA[NOME_AREA]` (da base de dados real do Excel) e as etapas de funil exibidas nas medidas HTML do Power BI.

A auditoria de dados processou as **176.252 linhas** da base operacional real e confirmou **100% de cobertura de mapeamento** (nenhum registro órfão ou não classificado).

---

## Resumo de Distribuição das Etapas

A tabela abaixo apresenta como os volumes de registros de jobs operacionais reais estão distribuídos nas etapas lógicas do painel:

| Ordem | Etapa Lógica no Painel | Palavras-Chave no DAX | Qtd. Ocorrências | % na Base Real | Tipo de Etapa |
| :---: | :--- | :--- | :---: | :---: | :---: |
| 1 | **FINALIZADO / FATURADO** | `FIM`, `FATUR` | 125.607 | 71,27% | Terminal (Sucesso) |
| 2 | **CHECKLIST** | `CHECKLIST`, `QUALIDADE`, `VALIDA` | 21.723 | 12,32% | Ativa (Em andamento) |
| 3 | **PROCURAÇÃO** | `PROCURA` | 6.914 | 3,92% | Ativa (Em andamento) |
| 4 | **OPERAÇÃO** | `OPER`, `PLANILH`, `RETIFIC`, `REVISÃO`, `JURÍD`, `MANIPULA` | 6.849 | 3,89% | Ativa (Em andamento) |
| 5 | **DIAGNÓSTICO** | `DIAGN`, `ENTREGA` | 5.232 | 2,97% | Ativa (Em andamento) |
| 6 | **CADASTRO / OUTROS** | *(Fallback / Sem correspondência)* | 4.220 | 2,39% | Ativa (Suporte/Inicial) |
| 7 | **REUNIÃO TÉCNICA** | `REUNIÃO`, `REUNIAO` | 3.564 | 2,02% | Ativa (Em andamento) |
| 8 | **CANCELADO / PERDIDO** | `DISTRATO`, `NÃO FATURADO`, `NÃO APROVADO`, `NÃO APRESENTADO` | 1.135 | 0,64% | Terminal (Perda) |
| 9 | **CRÉDITOS** | `CRÉDIT`, `CREDIT`, `COMPENSA`, `LIQUID` | 1.008 | 0,57% | Terminal (Êxito) |
| - | **TOTAL GERAL** | - | **176.252** | **100,00%** | - |

---

## Detalhamento de Mapeamento por Categoria

Abaixo estão listadas todas as strings originais e únicas encontradas na base operacional e suas respectivas classificações:

### 1. FINALIZADO / FATURADO
Etapa concluída operacionalmente e com faturamento/honorários liquidados.
* `'FIM'`: 125.307 ocorrências (71,095%)
* `'FATURADO'`: 300 ocorrências (0,170%)

### 2. CHECKLIST
Fase de conferência inicial de documentos fiscais e saneamento de pendências.
* `'CHECKLIST'`: 21.260 ocorrências (12,062%)
* `'QUALIDADE'`: 444 ocorrências (0,252%)
* `'PRAZO DE VALIDAÇÃO'`: 11 ocorrências (0,006%) *(contém "VALIDA")*
* `'VALIDAÇÃO SEFAZ'`: 8 ocorrências (0,005%) *(contém "VALIDA")*

### 3. PROCURAÇÃO
Fase de procurações eletrônicas junto ao e-CAC/Receita Federal.
* `'PROCURAÇÃO ELETRÔNICA'`: 6.914 ocorrências (3,923%)

### 4. OPERAÇÃO
Fila ativa de cálculos, revisões e auditorias operacionais.
* `'OPERAÇÃO'`: 6.113 ocorrências (3,468%)
* `'JURÍDICO'`: 240 ocorrências (0,136%)
* `'STAND BY OPERAÇÃO'`: 171 ocorrências (0,097%)
* `'RETIFICAÇÃO'`: 145 ocorrências (0,082%)
* `'REVISÃO'`: 106 ocorrências (0,060%)
* `'MANIPULAÇÃO'`: 45 ocorrências (0,026%)
* `'PLANILHAMENTO'`: 29 ocorrências (0,016%)

### 5. DIAGNÓSTICO
Fase final de elaboração ou apresentação do diagnóstico tributário.
* `'ENTREGA'`: 5.227 ocorrências (2,966%)
* `'DIAGNÓSTICO'`: 5 ocorrências (0,003%)

### 6. CADASTRO / OUTROS (Fallback)
Representa a fase pré-operacional de atração, contato e negociação comercial, onde ainda não há um job técnico aberto na esteira de produção.
* `'CONTATO'`: 2.301 ocorrências (1,306%)
* `'NEGOCIAÇÃO'`: 1.072 ocorrências (0,608%)
* `'CADASTRO'`: 779 ocorrências (0,442%)
* `'PREDOT'`: 13 ocorrências (0,007%)
* `'TREINAMENTO'`: 9 ocorrências (0,005%)
* `'AUDITORIA TÉCNICA'`: 8 ocorrências (0,005%)
* `'FINANCEIRO'`: 8 ocorrências (0,005%)
* `'IMPLANTAÇÃO'`: 7 ocorrências (0,004%)
* `'AJUÍZAMENTO'`: 7 ocorrências (0,004%)
* `'CONTRATO ENVIADO'`: 6 ocorrências (0,003%)
* `'HABILITAÇÃO'`: 4 ocorrências (0,002%)
* `'STAND BY'`: 3 ocorrências (0,002%)
* `'IMPLEMENTAÇÃO'`: 2 ocorrências (0,001%)
* `'REVISTA'`: 1 ocorrência (0,001%)

### 7. REUNIÃO TÉCNICA
Projetos aguardando ou em reunião de aprovação de números com o cliente.
* `'REUNIÃO TÉCNICA'`: 3.563 ocorrências (2,022%)
* `'REUNIÃO DE ABERTURA'`: 1 ocorrência (0,001%)

### 8. CANCELADO / PERDIDO
Trabalhos operacionais cancelados na esteira ou oportunidades que não seguiram.
* `'DISTRATO'`: 935 ocorrências (0,530%)
* `'NÃO FATURADO'`: 140 ocorrências (0,079%)
* `'NÃO FATURADO - COMERCIAL'`: 49 ocorrências (0,028%)
* `'NÃO APRESENTADO'`: 7 ocorrências (0,004%)
* `'NÃO APROVADO'`: 4 ocorrências (0,002%)

### 9. CRÉDITOS
Trabalhos técnicos concluídos com créditos em fase de homologação ou compensação.
* `'COMPENSAÇÃO'`: 992 ocorrências (0,563%)
* `'LIQUIDAÇÃO'`: 16 ocorrências (0,009%)
