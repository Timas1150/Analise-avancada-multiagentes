# Relatório do Construtor — rodada 02

## Antecedente e versão

Atende aos itens 1 e 2 de `relatorios/rodada-01/revisor.md`: validação incompleta de faturamento/valores não finitos e frase obsoleta no README. O repositório está em `C:/Users/iadados1/Documents/agentes-habilidades`, HEAD observado `f16f4f85df0f2b91dfac99e17030439b7855193c`; os artefatos do projeto continuam não commitados (`projeto-dashboard/` aparece como não rastreado). Na primeira rodada, o Construtor não tinha acesso a esses metadados; o HEAD e o estado atual foram confirmados nesta sessão.

Hashes SHA-256 desta versão:

| Arquivo | SHA-256 |
|---|---|
| `papelaria_limpa.csv` (fonte) | `7c8222e3090cc9bb81d2b19ee43a6d7f99a6e30f7e572cd0c28de98d71cb7374` |
| `preparar_dados.py` | `b3b468d8e6e79aea74789cdc2fcb3b7bf30519eadf700696536ce6ce49714a0e` |
| `README.md` | `a7afb03e3807e90c348fbbb4279832c4afc13ba0fcf0ac577599c246f5e907e8` |
| `ANALISE.md` | `5a7711f880ee503a8a0424a94a2961ce1f982312fb7ad10720c0392b2de14c6a` |
| `dados_preparados.csv` | `2074634bfebe9e4a6cf17a36de57f22a6a880267920f917753e072da203673d4` |
| `site/data/vendas.json` | `4b6f0e2b49199e6c75bbbae82f9b8457089cec1f60ba99b75556532e034c777b` |
| `tests/test_dashboard.py` (somente conferência; não editado) | `5b7b2b7798311a05a1e279ffa5d53785d3fd4d39ca5c30b27f4da4fbfcb51f0a` |
| `tests/browser_cdp.js` (somente conferência; não editado) | `0027176f04fbaa67fb17af6812de37ffdc1bf8a46b8694cbc17417a6fef38506` |
| `tests/browser-evidence.json` (somente conferência; não editado) | `6af251f2c704aa043b3848702acae1a37eed6165d6982f3023097b513481924a` |

O hash atual de `tests/test_dashboard.py` difere do hash listado na revisão 01; não editei esse arquivo. As verificações unitárias direcionadas abaixo foram executadas sobre o conteúdo presente nesta rodada.

## Alterações

- `preparar_dados.py` agora valida, antes de criar ou gravar saídas, que `quantidade`, `preco_unitario` e `faturamento` são numéricos, finitos e estritamente positivos. Quantidade ainda precisa ser inteira e caber em `int64`.
- Após calcular `faturamento_calculado` e `diferenca_faturamento`, também rejeita resultados não finitos. Totais, ticket e maior diferença são verificados antes da gravação para impedir overflow em agregações.
- A fonte e o faturamento registrado continuam preservados; `faturamento` continua sendo a métrica do dashboard. A validação não deduplica nem altera a fonte.
- O `README.md` não promete testes futuros: agora descreve a suíte existente, comandos em sequência, e os pré-requisitos Node.js/Chrome para a verificação de navegador.
- `ANALISE.md` descreve as validações de finitude, positividade e a interrupção sem novas gravações diante de dados inválidos.

## Comandos, testes e resultados

- `python preparar_dados.py` — passou na base real: 200 linhas, 200 IDs, faturamento R$ 11.546,80, zero diferenças frente a quantidade × preço. Os hashes dos artefatos preparados permaneceram iguais aos da rodada 01; hash da fonte também permaneceu igual.
- Fixture temporária executada com `@' ... '@ | python -` no PowerShell; 18/18 casos inválidos foram rejeitados: NaN, +Infinity, -Infinity, zero e negativo nas três métricas de origem; quantidade além de `int64`; overflow do produto calculado; overflow de soma agregada. Em cada fixture, o hash do CSV de entrada e sentinelas nos dois caminhos de saída permaneceram intactos. Os temporários foram removidos pelo `TemporaryDirectory`; nenhum teste foi escrito no projeto.
- `python -m unittest discover -s tests -p test_dashboard.py -k source_preserved_and_pipeline_only_writes_outputs -v` — 1 teste passou.
- `python -m unittest discover -s tests -p test_dashboard.py -k preparation_rejects_negative_and_non_finite_revenue -v` — 1 teste passou.
- `python -m unittest discover -s tests -p test_dashboard.py -k readme_has_no_obsolete_future_testing_promise -v` — 1 teste passou.
- AST do Python e `node --check site/app.js` — passaram. O JavaScript e a interface não foram alterados nesta rodada.
- Skills `explore-data`, `data-visualization` e `validate-data` foram carregadas pelo harness em `.opencode/skills/`; não usei caminhos `.agents/skills/` ausentes.

## Pendências e encaminhamento

- **PENDENTE — Tester:** executar a suíte completa na versão/hashes acima, complementar a cobertura para finitude em todas as métricas e atualizar o relatório da rodada 02. Não executei a suíte integral porque o teste de navegador regrava `tests/browser-evidence.json`, e a instrução desta rodada proíbe alterações em `tests/`.
- **PENDENTE — Revisor:** rever a correção e os novos hashes após a execução do Tester.
- O diretório `tests/` e os relatórios de Tester/Revisor/rodada 01 não foram editados por mim.

## Status

**APROVADO — correção e verificações do Construtor.** A aprovação conjunta não está concluída; suíte completa e revisão desta versão continuam **PENDENTES**.
