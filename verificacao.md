# Verificação da análise de papelaria

## Escopo e método

Conferi localmente `vendas_papelaria.csv`, `papelaria_limpa.csv`, `resumo.md`, `graficos.py` e os gráficos `categorias.png` e `cidades.png`. Usei `.venv/Scripts/python.exe` para recalcular valores diretamente dos CSVs; o recálculo usou aritmética decimal e não dependeu dos totais escritos no resumo nem de integrações externas.

## Contagens e total vendido

| Verificação | Resultado |
|---|---:|
| Linhas de dados no original | 182 |
| IDs de venda distintos no original | 180 |
| Linhas excedentes por duplicação exata | 2 (uma cópia extra de `V0010` e outra de `V0020`) |
| Vendas distintas após limpeza (`id_venda` distinto) | 180 |
| Soma no original sem remover duplicatas | R$ 20.066,00 |
| Soma após deduplicar e recalcular quantidade × preço | R$ 20.059,00 |
| Soma de `valor_venda` na cópia limpa | R$ 20.059,00 |

A diferença de R$ 7,00 entre a soma bruta e a limpa corresponde às duas linhas duplicadas, cada uma no valor de R$ 3,50. Portanto, **a contagem de vendas é 180**, não 182: 182 é a quantidade de linhas do arquivo original antes de remover as repetições.

## Conferência dos subtotais

| Categoria | Valor vendido | Cidade | Valor vendido |
|---|---:|---|---:|
| Acessórios | R$ 10.517,00 | Campinas | R$ 8.578,00 |
| Livros | R$ 7.438,00 | Santos | R$ 6.937,50 |
| Papelaria | R$ 2.104,00 | Sorocaba | R$ 4.543,50 |
| **Soma por categoria** | **R$ 20.059,00** | **Soma por cidade** | **R$ 20.059,00** |

Ambas as somas conferem com o total vendido da cópia limpa.

## Código e gráficos

- `graficos.py` lê `papelaria_limpa.csv` no mesmo diretório do script; não usa o CSV original para gerar os gráficos.
- O código agrupa e soma `valor_venda` por categoria e cidade. Conferi que os resultados correspondem aos subtotais acima.
- Os arquivos `categorias.png` e `cidades.png` foram regenerados pelo script. Conferi títulos, período (01/01/2026 a 31/03/2026), formatação em reais e eixo vertical iniciado em zero.

## Revisão das conclusões e ressalvas

O cálculo e os subtotais do resumo conferem. Ajustei `resumo.md` para distinguir explicitamente as 182 linhas brutas das 180 vendas distintas e para chamar o resultado de **valor total vendido**, não lucro. O valor é derivado de `quantidade × preco_unitario`; não há dados de custos ou despesas para calcular lucro. A verificação se limita aos arquivos locais fornecidos e não valida se os preços registrados correspondem a recebimentos ou registros contábeis externos.

**Avaliação:** cálculos, agregações e base dos gráficos conferidos; compartilhável com a ressalva de que valor vendido não é lucro.
