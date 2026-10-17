# Análise da base de vendas

## Fonte, grão e qualidade

A fonte é `papelaria_limpa.csv`, preservada na pasta raiz do projeto. Tem 200 linhas e oito colunas de origem. Cada linha representa uma venda; `id_venda` é o identificador. Há 200 IDs distintos, nenhuma linha duplicada e nenhum campo vazio. O período vai de 2025-01-02 a 2025-12-25.

O primeiro cabeçalho contém os caracteres literais `\\ufeff` antes de `id_venda`; `preparar_dados.py` remove esse trecho apenas do nome da coluna em memória. Lê os valores com pandas, valida números finitos e positivos, quantidade inteira, datas `AAAA-MM-DD` e campos ausentes. Não elimina duplicatas: interrompe a preparação se encontrar um `id_venda` repetido, para que os registros sejam investigados. Também preserva o faturamento informado na fonte.

O faturamento da fonte soma **R$ 11.546,80**. Nas 200 linhas, ele coincide com quantidade × preço unitário arredondado para duas casas. Esse cálculo é somente uma conferência e não substitui o faturamento informado.

## Indicadores e visualizações

- **Valor total vendido:** soma do `faturamento` da fonte para as vendas selecionadas.
- **Vendas:** quantidade de `id_venda` distintos no conjunto selecionado; cada linha validada representa uma venda.
- **Ticket médio:** valor total vendido dividido pela quantidade de vendas; para uma seleção vazia, o valor é R$ 0,00.
- **Gráficos:** faturamento por categoria e por mês. A ordem dos meses é cronológica; categorias são ordenadas pelo faturamento.
- **Filtros:** categoria e região; cada mudança recalcula indicadores, gráficos e tabela sobre os mesmos registros.

Os valores monetários são somados em centavos inteiros no navegador para evitar resíduos de ponto flutuante. A pasta `site/` lê apenas o JSON preparado em `site/data/vendas.json`; os filtros não gravam nem alteram dados. Como a fonte não contém custos, despesas, devoluções ou margem, faturamento não deve ser interpretado como lucro.

## Limitações e arquivos legados

O painel é uma fotografia dos dados gerados por `preparar_dados.py`. Para refletir mudanças na fonte, execute novamente o script e publique o novo JSON junto com os demais arquivos de `site/`. O dashboard estático não oferece cadastro, autenticação ou persistência.

Há arquivos de uma implementação anterior com API e SQLite (`app.py`, `database.py`, `init_db.py`, `schema.sql` e `banco.db`). Eles não são usados pelo site estático e foram preservados nesta correção; não os apague sem revisar e arquivar a implementação anterior. O README contém os comandos atuais de preparação, testes e servidor HTTP local.
