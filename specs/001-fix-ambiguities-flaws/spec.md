# Feature Specification: Fix Ambiguities and Flaws

**Feature Branch**: `001-fix-ambiguities-flaws`

**Created**: 2026-05-19

**Status**: Draft

**Input**: User description: "criar um plano para verificar todas ambiguidades existentes e corrijir falhas e problemas encontrados"

## User Scenarios & Testing

### User Story 1 - Auditoria e Diagnóstico de Ambiguidades Conhecidas (Priority: P1)

O analista de negócios precisa visualizar e entender todas as ambiguidades identificadas no mapeamento DOCX vs. Banco de Dados, incluindo guias sem correspondência, códigos duplicados/sobrepostos, e itens pendentes de validação, para poder tomar decisões sobre correções.

**Why this priority**: Sem entender o estado atual completo das ambiguidades, não é possível priorizar ou planejar correções.

**Independent Test**: Pode ser testado gerando um relatório/listagem de todas as ambiguidades conhecidas e validando com o especialista de negócio.

**Acceptance Scenarios**:

1. **Given** a list de itens MAP_TRIBUTOS_DOCX com StatusValidacao = Revisar, **When** o analista revisa cada um, **Then** cada item é classificado como Aprovado ou Rejeitado com justificativa
2. **Given** 6 guias DOCX sem correspondência em nenhuma coluna, **When** o analista investiga cada guia, **Then** cada uma tem uma ação definida (adicionar mapeamento, marcar como fora de escopo, ou solicitar novo mapeamento da área tributária)

---

### User Story 2 - Correção de Falhas de Performance e Renderização (Priority: P1)

O usuário do dashboard precisa visualizar os diagnósticos Journey sem travamentos ou timeout, com paginação funcional e performance aceitável (renderização < 3s para 500 linhas).

**Why this priority**: Os problemas de performance (560MB peak, 7.7s) impactam diretamente a usabilidade do dashboard.

**Independent Test**: Pode ser testado medindo o tempo de renderização e o consumo de memória com um conjunto de dados representativo.

**Acceptance Scenarios**:

1. **Given** a medida HTML_DETALHAMENTO_DIAGNOSTICO_JOURNEY_PAGINADO, **When** renderizada no visual HTML Content com 750 jobs, **Then** a renderização não excede 3 segundos
2. **Given** o dataLimit.override do Deneb, **When** validado no Power BI Desktop, **Then** o limite de 10.000 linhas é excedido sem truncamento

---

### User Story 3 - Resolução de Inconsistências no Mapeamento DOCX (Priority: P2)

O administrador dos dados precisa corrigir as inconsistências identificadas nas guias DOCX, incluindo códigos de produto duplicados, guias sem match, e false positives em filtros textuais.

**Why this priority**: Inconsistências no mapeamento produzem resultados incorretos no dashboard.

**Independent Test**: Pode ser testado verificando se cada guia DOCX tem um mapeamento válido e sem conflitos em MAP_TRIBUTOS_DOCX.

**Acceptance Scenarios**:

1. **Given** o código duplicado `369/SUPPLY TAX` na guia Liquidação Contratada, **When** validado com a área tributária, **Then** o código correto é estabelecido (ou a guia é marcada como erro documental)
2. **Given** os 18 itens com StatusValidacao = Revisar, **When** revisados pelo negócio, **Then** cada um recebe Ativo = TRUE ou é removido do mapeamento ativo

---

### Edge Cases

- Como tratar guias DOCX que mapeiam para o mesmo NOME_TRIBUTO (ex: RENEGOCIAÇÃO DE DÍVIDAS → 2 guias)?
- Como lidar com filtros que usam termos genéricos (ENERGIA, SEGUROS, IRPJ/CSLL) que produzem false positives?
- O que acontece quando um novo produto é adicionado ao banco que corresponde a uma guia sem mapeamento?
- Como garantir que correções em uma área não quebram o funcionamento de outra?

## Requirements

### Functional Requirements

- **FR-001**: Sistema DEVE listar todas as ambiguidades conhecidas do mapeamento DOCX vs. banco de dados em um formato auditável
- **FR-002**: Cada item em MAP_TRIBUTOS_DOCX com StatusValidacao = Revisar DEVE ter uma ação de revisão documentada (aprovar, rejeitar, ou solicitar mais informações)
- **FR-003**: Guias DOCX sem correspondência no NOME_TRIBUTO ([NEEDS CLARIFICATION: 6 guias identificadas sem match - qual o processo para definir se devem ser mapeadas ou excluídas do escopo?]) DEVEM ter uma ação definida
- **FR-004**: Falhas de performance na renderização HTML ([NEEDS CLARIFICATION: qual o limite aceitável de tempo de renderização e consumo de memória para o dashboard em produção?]) DEVEM ser resolvidas para garantir usabilidade
- **FR-005**: O dataLimit.override do Deneb DEVE ser validado no Power BI Desktop para garantir que o limite de 10.000 linhas é excedido corretamente
- **FR-006**: Códigos de produto duplicados ou sobrepostos entre guias DOCX DEVEM ser identificados e resolvidos com a área tributária
- **FR-007**: Filtros textuais que produzem false positives DEVEM ser substituídos por mapeamento governado via MAP_TRIBUTOS_DOCX
- **FR-008**: [NEEDS CLARIFICATION: qual o processo de governança para manter MAP_TRIBUTOS_DOCX atualizado quando novos produtos forem adicionados?]
- **FR-009**: Toda correção DEVE ser feita de forma não-destrutiva, mantendo backup das medidas originais
- **FR-010**: Sistema DEVE validar que as correções aplicadas não alteram os totais esperados (comparação antes/depois)

### Key Entities

- **MAP_TRIBUTOS_DOCX**: Tabela calculada que mapeia NOME_TRIBUTO para guias DOCX com StatusValidacao e Ativo. Contém 38 registros (20 ativos, 18 em revisão).
- **JOBS_Produto[IsTributoDocx]**: Coluna calculada oculta que filtra registros por NOME_TRIBUTO mapeados como ativos.
- **GUIAS_DOCX**: 31 seções-guia no documento GS GQT - Esteira Journey.docx, das quais 8 têm tabelas de mapeamento explícitas com códigos de produto.
- **Medidas HTML**: Conjunto de medidas em medidas_html que renderizam HTML para visualização no dashboard, atualmente com limitações de performance.

## Success Criteria

### Measurable Outcomes

- **SC-001**: 100% dos 18 itens com StatusValidacao = Revisar têm decisão documentada (Aprovado/Rejeitado) até a conclusão da auditoria
- **SC-002**: 100% das 6 guias DOCX sem correspondência têm ação definida (mapeamento criado, fora de escopo, ou pendente de informações)
- **SC-003**: Renderização do HTML principal no Power BI Desktop não excede 3 segundos para 750 jobs renderizados
- **SC-004**: Pico de memória durante renderização HTML reduzido em pelo menos 50% em relação aos ~560MB atuais
- **SC-005**: Zero códigos de produto duplicados entre guias DOCX ativas no mapeamento final
- **SC-006**: Zero false positives conhecidos nos filtros de produto/subproduto do dashboard
- **SC-007**: Todas as medidas alteradas possuem backup funcional e validado antes da substituição

## Assumptions

- A área de negócio tributária estará disponível para revisar e validar os mapeamentos ambíguos
- As 6 guias DOCX sem match podem requerer acesso ao documento original para investigação adicional
- O Power BI Desktop é a ferramenta primária de validação; mudanças no modelo semântico são feitas via MCP
- A performance atual (7.7s, 560MB) é considerada inaceitável para produção e requer melhoria
- O código duplicado `369/SUPPLY TAX` na guia Liquidação Contratada é provavelmente um erro documental no DOCX
- Termos como ENERGIA, SEGUROS, IRPJ/CSLL não podem ser usados como filtros textuais amplos
- MAP_TRIBUTOS_DOCX é a fonte de verdade governada para mapeamento de tributos
