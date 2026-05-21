# Feature Specification: Visualização Macro e Acompanhamento Executivo

**Feature Branch**: `001-executive-dashboard`

**Created**: 2026-05-21

**Status**: Draft

**Input**: User description: "Criar uma visualização macro em Power BI para acompanhamento executivo de uma área/processo de negócio..."

## User Scenarios & Testing

### User Story 1 - Visão Executiva e Painel Macro (Priority: P1)

Como usuário executivo, eu preciso de um painel de visualização de alto nível consolidado com os principais indicadores de desempenho (KPIs), evolução temporal e status operacionais, para entender o cenário geral da operação em poucos minutos.

**Why this priority**: É o objetivo central do projeto. Sem a visão macro estruturada de forma simples, o dashboard falha em atender os executivos.

**Independent Test**: Pode ser testado apresentando o painel principal a um usuário executivo para verificar se ele consegue identificar os principais indicadores e o status geral do negócio em menos de 2 minutos sem treinamento.

**Acceptance Scenarios**:
1. **Given** que o usuário executivo abre o relatório, **When** ele visualiza a tela principal, **Then** ele vê os cartões com os principais KPIs atualizados e um indicador visual claro do status geral (ex: alertas de atenção ou crítico).
2. **Given** a página principal do dashboard, **When** o usuário observa a distribuição por status operacionais, **Then** ele identifica claramente a quantidade e a proporção de itens ativos, concluídos, pendentes e críticos.

---

### User Story 2 - Análise de Desvios e Comparação Temporal (Priority: P2)

Como analista de negócios, eu preciso filtrar os dados do painel por períodos (ex: meses, trimestres) e categorias (ex: linhas de produto, áreas de negócio) e comparar os resultados com o período anterior, para identificar variações importantes e pontos que exigem atenção.

**Why this priority**: Permite que o dashboard seja útil para investigar os desvios apontados na visão executiva, viabilizando ações corretivas.

**Independent Test**: Pode ser testado selecionando filtros específicos de categoria e comparando se a variação exibida corresponde matematicamente aos dados reais do período atual contra o anterior.

**Acceptance Scenarios**:
1. **Given** que o dashboard possui filtros globais consistentes, **When** o analista seleciona uma categoria e um período específicos, **Then** todos os visuais da página são filtrados e mostram a variação percentual em relação ao mês anterior (MoM - Month-over-Month).
2. **Given** a seleção de um filtro global de período, **When** o analista altera a página do relatório, **Then** os filtros selecionados persistem em todas as páginas para manter a consistência da análise.

---

### User Story 3 - Detalhamento Macro Explicativo (Drill-Down / Tooltip) (Priority: P2)

Como usuário executivo ou analista, eu preciso ver um detalhamento focado apenas quando um KPI ou categoria apresentar variação fora do esperado, para entender o resultado sem sobrecarregar a visão principal.

**Why this priority**: Evita a sobrecarga cognitiva e garante o princípio de clareza antes de complexidade.

**Independent Test**: Pode ser testado interagindo com um elemento de alerta ou categoria específica no gráfico para acionar a visão detalhada (ex: via tooltips descritivos ou drill-through) e validar se as informações complementares explicam o comportamento do KPI.

**Acceptance Scenarios**:
1. **Given** um gráfico de evolução temporal que apresenta um desvio incomum, **When** o usuário posiciona o cursor sobre o ponto do gráfico, **Then** um tooltip contextual é exibido explicando as principais categorias ou itens que causaram aquela variação.
2. **Given** um KPI com status de alerta crítico, **When** o usuário clica com o botão direito para detalhamento (drill-through), **Then** ele é direcionado a uma tela de apoio com a lista das principais pendências que impactam a métrica.

---

## Edge Cases

- Como tratar a exibição de variações percentuais quando o valor do período anterior for zero (evitar erros de divisão por zero na interface)?
- O que acontece se o volume de dados ativos em uma categoria for muito pequeno para gerar uma análise de tendência estatística relevante?
- Como lidar com atualizações de dados que ocorrem enquanto um usuário executivo está ativamente visualizando o dashboard no ambiente compartilhado?
- O que exibir se todos os filtros globais forem desmarcados (exibir totalizador padrão ou forçar seleção padrão de período)?

## Requirements

### Functional Requirements

- **FR-001**: O dashboard DEVE consolidar e exibir indicadores principais (KPIs) para a Esteira de Jobs / Journey em cartões em destaque no topo da página. As três métricas principais são: Quantidade de Jobs Ativos, Tempo Médio de Processamento e Taxa de Sucesso/Conclusão (%).
- **FR-002**: O sistema DEVE disponibilizar filtros globais de fácil acesso para segmentação de dados por períodos e categorias de negócio em todas as páginas.
- **FR-003**: O relatório DEVE destacar de forma visual (ex: cores ou ícones de alerta) os itens com status operacionais específicos: ativos, concluídos, pendentes ou em situação crítica.
- **FR-004**: O dashboard DEVE exibir a evolução temporal dos principais indicadores em um gráfico de tendência.
- **FR-005**: O sistema DEVE calcular e exibir a variação (crescimento/declínio) dos indicadores em relação ao período de comparação selecionado.
- **FR-006**: O dashboard DEVE oferecer mecanismos de detalhamento sob demanda (drill-down ou tooltips explicativos) nas variações macro de desempenho.
- **FR-007**: A exibição dos dados executivos DEVE respeitar possíveis regras de acesso corporativas aplicadas na publicação final. O modelo de dados interno não exige a implementação de RLS nesta fase de desenvolvimento.
- **FR-008**: O relatório e o modelo semântico DEVEM ser mantidos estritamente organizados e compatíveis com a estrutura do Power BI Project (.pbip) para permitir versionamento via Git.

### Key Entities

- **dCalendar**: Tabela de dimensão de calendário para suporte a inteligência de tempo e comparações de períodos.
- **Tabela de Fatos de Negócio (ex: QBERT_JOBS_JOURNEY ou similar)**: Registro das operações, contendo status, datas e valores quantitativos.
- **Dimensão de Categorias (ex: MAP_PRODUTOS_ESTEIRA_TAX)**: Mapeamento de produtos, subprodutos ou áreas de negócio para aplicação dos filtros.
- **Modelo de Relatório (.pbip)**: A definição do relatório e das páginas estruturada em arquivos JSON legíveis.

## Success Criteria

### Measurable Outcomes

- **SC-001**: O tempo necessário para um usuário executivo identificar o status de saúde geral do processo (OK, Atenção, Crítico) ao abrir o dashboard é menor que 30 segundos.
- **SC-002**: 100% dos filtros globais configurados persistem e funcionam de forma síncrona em todas as páginas do dashboard.
- **SC-003**: 100% das métricas e KPIs principais exibidos na tela possuem tooltips explicativos contendo sua fórmula lógica ou regra de negócio detalhada.
- **SC-004**: Todos os visuais da página executiva carregam em menos de 3 segundos após a aplicação de qualquer filtro ou segmentador.
- **SC-005**: A alteração de qualquer visual ou criação de nova página executiva não gera conflitos que impeçam o merge limpo no Git (verificação da integridade do arquivo `.pbip`).

## Assumptions

- O modelo de dados possui uma tabela de calendário (`dCalendar`) completa e com relacionamento ativo com as datas de transação.
- O desenvolvimento das páginas executivas não deve impactar ou alterar retroativamente a lógica interna dos dados das fontes primárias sem aviso prévio.
- O público-alvo executivo possui acesso adequado à licença do Power BI para visualização interativa do dashboard.
- As regras de formatação e temas visuais seguirão os padrões corporativos estabelecidos.
