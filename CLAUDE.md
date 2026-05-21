# Dash Journey

Projeto Power BI de análise de Jobs e Produtos, utilizando Power BI Project (PBIP) com modelo semântico em TMDL.

## Estrutura

```
Dash Journey/
├── Jobs_Produtos.pbix          # Power BI Desktop file
├── Jobs_Produtos.pbip          # Power BI Project manifest
├── Jobs_Produtos.Report/       # Relatório (páginas, visuais, layout)
├── Jobs_Produtos.SemanticModel/
│   ├── definition/
│   │   ├── model.tmdl          # Definição do modelo (tabelas, cultura, refs)
│   │   ├── database.tmdl       # Compat level 1600
│   │   ├── relationships.tmdl  # Relacionamentos entre tabelas
│   │   ├── tables/             # Tabelas em arquivos .tmdl
│   │   │   ├── dCalendar.tmdl
│   │   │   ├── JOBS_Produto.tmdl
│   │   │   └── medidas_html.tmdl
│   │   └── cultures/           # pt-BR
│   └── .pbi/                   # Cache e settings locais (gitignored)
├── .mcp.json                   # Power BI Modeling MCP server
└── .claude/settings.local.json # Permissões e MCP habilitado
```

## Stack

- **Power BI Desktop** — arquivo .pbix com modelo semântico
- **Power BI Project (PBIP)** — source control do modelo em TMDL
- **Compatibilidade:** 1600
- **Cultura:** pt-BR
- **Anotações:** MCP-PBIModeling, DevMode, Time Intelligence habilitado

## Power BI Modeling MCP

O MCP server está configurado via `.mcp.json` e habilitado em `.claude/settings.local.json`. Para conectar ao modelo:

```
Open semantic model from PBIP folder 'Jobs_Produtos.SemanticModel/definition'
```

### Ferramentas disponíveis

- `connection_operations` — conectar a Desktop, Fabric ou PBIP
- `model_operations` — manipular o modelo (refresh, rename, stats)
- `table_operations` / `column_operations` / `measure_operations` — gerenciar tabelas, colunas e medidas
- `relationship_operations` — relacionamentos entre tabelas
- `dax_query_operations` — executar e validar queries DAX
- `calculation_group_operations` — grupos de cálculo
- `security_role_operations` — RLS e regras de segurança
- `perspective_operations` — perspectivas
- `culture_operations` — traduções e multi-idioma
- `named_expression_operations` — parâmetros Power Query
- `transaction_operations` — transações (begin, commit, rollback)

### Cuidados

- **Sempre faça backup do modelo antes de modificações**
- O MCP pede confirmação antes da primeira modificação e da primeira query
- Use `--skipconfirmation` nas args do .mcp.json para pular confirmações (com cautela)

<!-- SPECKIT START -->
For additional context about technologies to be used, project structure,
shell commands, and other important information, read the current plan
<!-- SPECKIT END -->
