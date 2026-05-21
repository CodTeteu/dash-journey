<!--
Sync Impact Report:
- Version change: [TEMPLATE] -> 1.0.0
- List of modified principles:
  * [PRINCIPLE_1] -> I. Clareza antes de complexidade
  * [PRINCIPLE_2] -> II. Modelo semântico como fonte da verdade
  * [PRINCIPLE_3] -> III. Dados confiáveis, rastreáveis e explicáveis
  * [PRINCIPLE_4] -> IV. Desenvolvimento versionável e revisável
  * [PRINCIPLE_5] -> V. Experiência visual consistente
  * [NEW] VI. Performance como requisito de produto
  * [NEW] VII. Segurança e responsabilidade no uso dos dados
- Added sections:
  * Critérios de Qualidade
  * Fluxo de Trabalho Esperado
- Removed sections: None
- Templates requiring updates:
  * ✅ .specify/templates/plan-template.md (already aligned)
  * ✅ .specify/templates/spec-template.md (already aligned)
  * ✅ .specify/templates/tasks-template.md (already aligned)
- Follow-up TODOs: None
-->

# Dashboard Power BI Constitution

## Core Principles

### I. Clareza antes de complexidade

Toda entrega deve priorizar entendimento, objetividade e utilidade para o usuário final.

A visualização deve responder perguntas de negócio de forma direta, evitando excesso de elementos visuais, métricas redundantes ou interações desnecessárias. Qualquer gráfico, cartão, tabela ou segmentador deve existir porque ajuda o usuário a tomar uma decisão ou entender melhor o contexto apresentado.

**Obrigatório**:
- Cada página deve ter um objetivo claro.
- Cada métrica deve ter interpretação evidente.
- Elementos visuais decorativos não devem competir com a informação principal.
- O layout deve facilitar leitura rápida e comparação entre indicadores.

### II. Modelo semântico como fonte da verdade

As regras de negócio devem estar centralizadas no modelo semântico sempre que possível.

Medidas, colunas calculadas, tabelas auxiliares e relacionamentos devem ser criados de forma consistente, evitando duplicidade de lógica entre páginas, visuais ou arquivos. Toda métrica relevante deve ser reutilizável, nomeada de forma clara e organizada em pastas ou grupos lógicos.

**Obrigatório**:
- Medidas devem ter nomes descritivos.
- Cálculos repetidos devem ser transformados em medidas reutilizáveis.
- Regras de negócio não devem ficar escondidas apenas em filtros visuais.
- Alterações em métricas críticas devem ser revisadas antes de publicação.

### III. Dados confiáveis, rastreáveis e explicáveis

O dashboard deve permitir que o usuário confie nos números apresentados.

Toda métrica importante deve ter origem, filtro e regra de cálculo compreensíveis. Quando houver filtros, recortes de período ou exclusões de dados, essas condições devem ser visíveis ou documentadas.

**Obrigatório**:
- Toda página deve deixar claro o contexto dos dados exibidos.
- Filtros obrigatórios devem ser explícitos.
- Métricas ambíguas devem ter descrição ou tooltip.
- Divergências conhecidas devem ser documentadas antes da entrega.

### IV. Desenvolvimento versionável e revisável

O projeto deve ser mantido de forma compatível com versionamento em Git.

Como o projeto utiliza Power BI Project (`.pbip`), alterações relevantes devem ser revisáveis por arquivo, com commits claros e escopo controlado. Arquivos locais, temporários ou de cache não devem ser versionados.

### V. Experiência visual consistente

A experiência do usuário deve ser simples, consistente e adequada ao público-alvo.

Cores, fontes, espaçamentos, títulos, ícones e interações devem seguir um padrão visual único. O dashboard deve evitar sobrecarga cognitiva e funcionar bem em cenários comuns de apresentação, acompanhamento e análise.

**Obrigatório**:
- Páginas devem seguir uma hierarquia visual consistente.
- Indicadores principais devem ter destaque proporcional à sua importância.
- Cores devem ter significado claro e consistente.
- A navegação deve ser previsível.

### VI. Performance como requisito de produto

O dashboard deve carregar, filtrar e interagir com fluidez.

Medidas DAX, relacionamentos, cardinalidade, filtros e visuais devem ser pensados para evitar lentidão. A performance deve ser considerada desde a modelagem, não apenas no final do projeto.

**Obrigatório**:
- Medidas devem evitar complexidade desnecessária.
- Visuais com alto volume de dados devem ser justificados.
- Tabelas detalhadas devem ser usadas com cautela.
- Interações lentas devem ser investigadas antes da entrega.

### VII. Segurança e responsabilidade no uso dos dados

Dados sensíveis, pessoais, estratégicos ou operacionais devem ser tratados com cuidado.

O dashboard deve apresentar apenas as informações necessárias ao objetivo da análise. Exposição de detalhes sensíveis deve ser limitada, justificada e, quando necessário, protegida por permissões, filtros ou regras de acesso.

**Obrigatório**:
- Dados sensíveis devem ser exibidos apenas quando forem essenciais.
- Páginas executivas devem evitar exposição desnecessária de dados granulares.
- Regras de acesso devem ser respeitadas.
- Exportações e compartilhamentos devem considerar o risco de exposição indevida.

## Critérios de Qualidade

Uma entrega só deve ser considerada pronta quando atender aos critérios abaixo:

1. O objetivo da visualização está claro.
2. Os principais indicadores estão corretos e explicáveis.
3. O layout é consistente e legível.
4. A navegação é simples.
5. A performance é aceitável para uso real.
6. O projeto continua compatível com `.pbip`.
7. O versionamento não contém arquivos indevidos.
8. As principais regras de negócio estão documentadas.
9. A exposição de dados é adequada ao público-alvo.
10. A entrega foi revisada contra esta constituição.

## Fluxo de Trabalho Esperado

### 1. Especificação

Antes de desenvolver, a necessidade deve ser descrita em termos de problema, público, objetivo e decisão esperada.

A especificação deve responder:
- Qual problema será resolvido?
- Quem usará a visualização?
- Que decisão ou acompanhamento será apoiado?
- Quais métricas são necessárias?
- Quais filtros e recortes são importantes?
- Quais limitações ou premissas existem?

### 2. Planejamento

O plano deve transformar a especificação em uma proposta de implementação.

O planejamento deve definir:
- Páginas ou seções necessárias.
- Métricas a criar ou alterar.
- Fontes e tabelas envolvidas.
- Regras de modelagem.
- Riscos técnicos ou de negócio.
- Critérios de validação.

### 3. Implementação

A implementação deve seguir o plano aprovado e preservar a estrutura do projeto.

Durante a implementação:
- Evite criar lógica duplicada.
- Teste medidas em diferentes contextos de filtro.
- Revise o comportamento visual em cenários comuns.
- Mantenha o repositório limpo.
- Documente decisões relevantes.

### 4. Validação

A validação deve confirmar se a entrega atende ao objetivo original.

A validação deve verificar:
- Coerência dos números.
- Funcionamento de filtros.
- Legibilidade das páginas.
- Performance.
- Aderência às regras de negócio.
- Aderência a esta constituição.

## Governance

### Regras de Governança

O projeto deve manter uma estrutura simples, legível e compatível com Power BI Project.

Arquivos e pastas devem ser organizados para facilitar manutenção, revisão e colaboração. Nomes devem ser descritivos e evitar abreviações obscuras.

### Nomeação

A nomenclatura deve favorecer leitura e manutenção.

**Diretrizes**:
- Use nomes claros para medidas, tabelas, páginas e campos.
- Evite nomes técnicos quando o público for de negócio.
- Use idioma consistente em todo o projeto.
- Evite duplicidade de métricas com nomes parecidos.

### Documentação

Toda regra relevante deve ser documentada no local mais próximo possível de sua implementação.

**Devem ser documentados**:
- Métricas críticas.
- Regras de cálculo.
- Filtros obrigatórios.
- Premissas de negócio.
- Limitações conhecidas.
- Decisões de modelagem.

### Revisão de mudanças

Antes de considerar uma alteração pronta, devem ser verificados:
- O dashboard abre corretamente.
- As principais páginas carregam sem erro.
- As métricas críticas continuam coerentes.
- Filtros e interações funcionam como esperado.
- Nenhum arquivo local ou temporário foi incluído no versionamento.
- A mudança respeita os princípios desta constituição.

### Requisitos para Uso com Spec Kit

Ao usar o Spec Kit neste projeto:
- Toda especificação deve respeitar os princípios desta constituição.
- Todo plano deve declarar como atende aos princípios aplicáveis.
- Toda lista de tarefas deve incluir validação de dados, revisão visual e checagem de versionamento.
- Qualquer exceção aos princípios deve ser justificada explicitamente.
- A constituição tem prioridade sobre preferências pontuais de implementação.

### Controle de Mudanças

Alterações nesta constituição devem ser versionadas e justificadas.

Cada mudança deve indicar:
- O que foi alterado.
- Por que foi alterado.
- Qual impacto esperado no projeto.
- Se templates, specs ou planos precisam ser atualizados.

**Version**: 1.0.0 | **Ratified**: 2026-05-21 | **Last Amended**: 2026-05-21
