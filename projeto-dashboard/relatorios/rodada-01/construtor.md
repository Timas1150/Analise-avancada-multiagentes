# Relatório do Construtor — rodada 01

## Versão examinada

Não há commit: o diretório de trabalho não é um repositório Git. Fonte examinada: `papelaria_limpa.csv`, SHA-256 `7c8222e3090cc9bb81d2b19ee43a6d7f99a6e30f7e572cd0c28de98d71cb7374`. O hash da fonte permaneceu igual após a preparação.

| Arquivo | SHA-256 |
|---|---|
| `preparar_dados.py` | `098dcd3bc936fdfdf6cdfa16dd831bae06fc5e43f1cef49414ad39bb7bf8f088` |
| `requirements.txt` | `a02d73f9d659e3bfa10341b1ce25bc7084d0540fb241a213aaf693328c197232` |
| `README.md` | `c56a41cbde5d3009783b0a8eb557be20ae5a4bb3924077245962d305d5de5e86` |
| `ANALISE.md` | `0c709d59216522642e86b2c8bbebe1fdb95491bcd8cef7822120f8c261975b05` |
| `dados_preparados.csv` | `2074634bfebe9e4a6cf17a36de57f22a6a880267920f917753e072da203673d4` |
| `site/index.html` | `bcb717c683bfac8938df2925cf3067aab78ac2352c0e18c35012d28faa0c71e6` |
| `site/styles.css` | `670fb693c31acbc1aa4f3d445f83a9893c8e2928f91cbde2a3b201379f9163fb` |
| `site/app.js` | `c510c116514edc582c33b4393a828a5565bb166cf1b99038418a59731284d277` |
| `site/data/vendas.json` | `4b6f0e2b49199e6c75bbbae82f9b8457089cec1f60ba99b75556532e034c777b` |

## Entregas construídas

- Pipeline reprodutível em `preparar_dados.py`, com pandas, que lê exclusivamente o CSV solicitado na raiz e gera `dados_preparados.csv` e `site/data/vendas.json`.
- Cabeçalho literal `\ufeffid_venda` normalizado apenas na leitura; texto UTF-8, incluindo `Hambúrgueres`, preservado.
- `ANALISE.md` registra grão, perfil, regras, duplicatas, métrica e limitações. O faturamento de origem é mantido e usado no dashboard; quantidade × preço serve como conferência, sem sobrescrever o campo fonte.
- Dashboard estático em `site/`: três indicadores, gráficos de valor por categoria e por mês, tabela e filtros nativos de categoria e região que atualizam todos os elementos; há botão para limpar, estado sem resultados e layout responsivo.
- `README.md` documenta dependências, preparação, comando de testes, servidor HTTP local e `site/` como pasta publicável.

Perfil observado: 200 linhas, 200 IDs únicos, nenhum nulo ou ID duplicado; soma de `faturamento` R$ 11.546,80. `quantidade × preco_unitario` também soma R$ 11.546,80 e coincide com o campo fonte nas 200 linhas. O ticket médio é R$ 57,73.

## Comandos executados e evidências

- `python .\preparar_dados.py` — executado duas vezes; gerou CSV e JSON com 200 registros, soma de R$ 11.546,80 e zero diferenças na conferência.
- Verificação pontual via `python -c` — confirmou 200 linhas nos dois artefatos, IDs únicos, ausência de nulos, total esperado e texto UTF-8 `Hambúrgueres`.
- `python -c "import ast,pathlib; ast.parse(...)"` — sintaxe de `preparar_dados.py` válida.
- `node --check site/app.js` — sintaxe JavaScript válida.
- Evidências de dados: `dados_preparados.csv`, `site/data/vendas.json`; documentação da metodologia: `ANALISE.md`.
- Ambiente usado: `python` 3.14.8 e pandas 3.0.6; `.venv/Scripts/python.exe` não estava disponível.

## Pendências e encaminhamento

- A suíte `python -m unittest discover -s tests -v` ainda não foi executada: o diretório/testes do Tester não estavam presentes nesta etapa. **PENDENTE — Tester:** criar e executar os testes obrigatórios sobre estes arquivos.
- Carregamento HTTP real, interação no navegador, responsividade observada em tela grande/pequena e acessibilidade por teclado não foram testados nesta etapa. **PENDENTE — Tester/Revisor:** executar e registrar evidências; não há aprovação de interface aqui.
- Os arquivos `.agents/skills/explore-data/SKILL.md`, `.agents/skills/data-visualization/SKILL.md` e `.agents/skills/validate-data/SKILL.md` não estão disponíveis. Não afirmo uso dessas skills; informar a limitação ao coordenador.
- Não alterei CSVs nem arquivos anteriores fora de `projeto-dashboard/`.

## Status

**APROVADO — construção e verificações locais do Construtor.** Isso não aprova a suíte obrigatória, interface, revisão independente nem status final conjunto; itens não executados permanecem **PENDENTES**.
