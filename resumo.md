# Resumo do tratamento — vendas_papelaria.csv

## Visão geral

- **Arquivo de origem:** `vendas_papelaria.csv` (preservado sem alterações)
- **Cópia de trabalho:** `papelaria_limpa.csv`
- **Grão:** uma linha por venda; `id_venda` identifica a venda
- **Linhas originais:** 182 (dados, sem contar o cabeçalho)
- **Colunas originais:** 8 — id_venda, data, cidade, categoria, produto, quantidade, preco_unitario, forma_pagamento
- **Linhas após limpeza:** 180
- **Colunas na cópia:** 9 (8 originais + `valor_venda`)
- **Período coberto:** 2026-01-01 a 2026-03-31

## Problemas encontrados no arquivo original

| # | Problema | Gravidade | Detalhe |
|---|----------|-----------|---------|
| 1 | Linhas duplicadas exatas | Alta | 2 linha(s) repetida(s) em 2 grupo(s). IDs afetados: V0010, V0020 — os valores são iguais em todas as colunas originais, incluindo `id_venda`; cada grupo representa a mesma venda registrada duas vezes. |
| 2 | Espaços em `cidade` | Média | 6 registro(s) com espaços no início/fim, distribuídos entre: ` Santos ` (2), ` Campinas ` (2), ` Sorocaba ` (2). Sem remoção, esses valores seriam agrupados como cidades distintas de suas versões sem espaços. |
| 3 | `categoria` inconsistente | Média | Grafias fora do padrão canônico: `acessórios` (3); isso fragmentaria os agrupamentos por categoria. |
| 4 | `forma_pagamento` vazia | Média | 4 venda(s) sem forma de pagamento (V0004, V0018, V0067, V0121), o que quebraria a análise por meio de pagamento. |
| 5 | Ausência de métrica de receita | Baixa | Não havia coluna de valor da venda; era preciso calcular `quantidade * preco_unitario` toda vez. |

Observações que **não** são problema: `quantidade` e `preco_unitario` sem nulos e sempre positivos; datas todas válidas e em ordem; `id_venda` sequencial sem lacunas.

## Tratamentos aplicados (somente na cópia)

1. **Duplicatas exatas removidas** — 2 linha(s) descartada(s), mantendo a primeira ocorrência. Foram removidas apenas linhas com valores iguais em todas as colunas originais. Vendas com produtos iguais, mas ID/data diferentes, foram preservadas — não são duplicatas exatas.
2. **`cidade` com `strip()`** — espaços das bordas removidos. Nenhum valor mudou de identidade, apenas o mesmo nome passou a ser escrito igual em toda a base (Campinas, Santos, Sorocaba).
3. **`categoria` padronizada** — por um mapa canônico, resultado: Acessórios, Livros, Papelaria. Todas as grafias de Acessórios convergiram para `Acessórios`.
4. **`forma_pagamento` vazia → `Não informado`** — 4 venda(s) deixada(s) com valor explícito, para não aparecer como nulo e distorcer a contagem por forma de pagamento.
5. **`valor_venda` calculada** — `valor_venda = quantidade * preco_unitario`, arredondada a 2 casas, adicionada como última coluna. O `preco_unitario` original foi mantido.

Nada além disso foi alterado: nenhuma linha foi excluída por outro motivo, nenhum valor foi imputado ou excluído como outlier.

## Resultado da limpeza

- Vendas distintas (`id_venda`): 180. O arquivo original tinha 182 linhas, incluindo 2 linhas duplicadas exatas.
- Valor total vendido após deduplicação: **R$ 20.059,00** (soma de `valor_venda`; não representa lucro).

### Valor vendido por categoria

| Categoria | Valor vendido (R$) |
|-----------|--------------|
| Acessórios | 10.517,00 |
| Livros | 7.438,00 |
| Papelaria | 2.104,00 |

### Valor vendido por cidade

| Cidade | Valor vendido (R$) |
|--------|--------------|
| Campinas | 8.578,00 |
| Santos | 6.937,50 |
| Sorocaba | 4.543,50 |

### Valor vendido por forma de pagamento

| Forma de pagamento | Valor vendido (R$) |
|---------------------|--------------|
| Dinheiro | 7.456,00 |
| Pix | 6.831,50 |
| Cartão | 5.135,50 |
| Não informado | 636,00 |

## Como reproduzir

```powershell
.venv\Scripts\python.exe analise_papelaria.py
```

O script usa apenas a biblioteca padrão do Python, lendo o CSV local — sem conexão com banco de dados. O `vendas_papelaria.csv` não é modificado em momento algum.
