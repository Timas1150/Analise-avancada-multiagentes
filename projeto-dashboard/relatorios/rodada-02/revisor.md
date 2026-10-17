# Relatório do Revisor — rodada 02

## Versão examinada

HEAD: `f16f4f85df0f2b91dfac99e17030439b7855193c`; raiz Git: `C:/Users/iadados1/Documents/agentes-habilidades`. `git status --short` informa `?? AGENTS.md` e `?? projeto-dashboard/`, então os artefatos do projeto permanecem fora do commit. O Construtor e o Tester registram o mesmo HEAD e identificam explicitamente os arquivos de trabalho por hash; conferi os hashes abaixo e eles coincidem com os relatórios da rodada 02.

| Arquivo | SHA-256 |
|---|---|
| `papelaria_limpa.csv` | `7c8222e3090cc9bb81d2b19ee43a6d7f99a6e30f7e572cd0c28de98d71cb7374` |
| `preparar_dados.py` | `b3b468d8e6e79aea74789cdc2fcb3b7bf30519eadf700696536ce6ce49714a0e` |
| `README.md` | `a7afb03e3807e90c348fbbb4279832c4afc13ba0fcf0ac577599c246f5e907e8` |
| `ANALISE.md` | `5a7711f880ee503a8a0424a94a2961ce1f982312fb7ad10720c0392b2de14c6a` |
| `dados_preparados.csv` | `2074634bfebe9e4a6cf17a36de57f22a6a880267920f917753e072da203673d4` |
| `site/index.html` | `bcb717c683bfac8938df2925cf3067aab78ac2352c0e18c35012d28faa0c71e6` |
| `site/styles.css` | `670fb693c31acbc1aa4f3d445f83a9893c8e2928f91cbde2a3b201379f9163fb` |
| `site/app.js` | `c510c116514edc582c33b4393a828a5565bb166cf1b99038418a59731284d277` |
| `site/data/vendas.json` | `4b6f0e2b49199e6c75bbbae82f9b8457089cec1f60ba99b75556532e034c777b` |
| `tests/test_dashboard.py` | `5b7b2b7798311a05a1e279ffa5d53785d3fd4d39ca5c30b27f4da4fbfcb51f0a` |
| `tests/browser_cdp.js` | `0027176f04fbaa67fb17af6812de37ffdc1bf8a46b8694cbc17417a6fef38506` |
| `tests/browser-evidence.json` | `6af251f2c704aa043b3848702acae1a37eed6165d6982f3023097b513481924a` |
| `tests/unittest-output.txt` | `3680a548c743de24fc67d827a3a6bdc831ac818b129ac12cafff50d9baa71afe` |
| `relatorios/rodada-02/construtor.md` | `a7d83072d17dd04b9fce8bc49cf4ee77fbb76bb7a00485d8117b78d2a6756070` |
| `relatorios/rodada-02/tester.md` | `5c4f242b2cd1eb9cb8a69f7bae87fbf9d8ca23906d1c55c093dce994c770da34` |

## Verificações independentes

- Li o parecer da rodada 01, os relatórios de Construtor e Tester da rodada 02, o preparador, a análise, o README, os testes e a evidência de browser. O preparador agora exige números finitos e positivos nas três métricas, quantidade inteira dentro do intervalo `int64`, cálculos/agrupamentos finitos; a validação ocorre antes da gravação. A promessa futura foi removida do README.
- Rodei em cópia temporária isolada a suíte integral: `python -m unittest discover -s tests -v` — **11 testes passaram, 0 falhas, 10,020 s**. A cópia foi removida; os arquivos examinados não foram alterados. A execução incluiu Chrome real via CDP. A evidência recém-gerada na cópia foi igual a `tests/browser-evidence.json`: PASS, 1440×1000 e 390×844, sem overflow; filtros isolados/combinados, vazio, reset, projeções, teclado/foco e sem exceções JavaScript.
- Testei independentemente 19 fixtures numéricas temporárias: NaN, ±Infinity, zero e negativo em cada uma das três métricas; quantidade fracionária e maior que `int64`; overflow do produto e da soma agregada. **19/19 rejeitadas**, preservando CSV e arquivos-sentinela de saída. Nenhuma fixture foi gravada no projeto.
- Recalculei diretamente do CSV com `csv` e `Decimal`: 200 linhas/IDs, nenhum campo vazio, total **R$ 11.546,80**, ticket exato **R$ 57,734** (exibição R$ 57,73) e três primeiras linhas **R$ 108,90**. Os agrupamentos independentes por categoria e mês conferem com o payload e total da análise. A fonte manteve SHA-256 esperado.
- Conferi os comandos do README. A suíte executa a preparação, verifica os três comandos documentados e testa o servidor exatamente em `python -m http.server 8000 --directory site`; HTTP para página, CSS, JS e JSON funciona. O README informa corretamente Node.js/Chrome necessários para o teste de browser e `site/` como publicável.
- Corrijo a narrativa do parecer anterior: o coordenador informa que as skills `explore-data`, `data-visualization` e `validate-data` foram usadas pelo harness em `.opencode/skills/`. O caminho `.agents/skills/` indicado na instrução antiga não existe, mas isso não significa ausência das skills; não registro bloqueio por esse caminho legado.

## Cobertura e limites

A cobertura obrigatória está representada por testes reproduzíveis de fonte/tipos/nulos/duplicatas, totais e exemplo conhecido, correspondência dos artefatos, filtros/projeções, HTTP, interação real, layout e teclado. O teste de regressão de inválidos no repositório cobre especificamente faturamento negativo, infinito e NaN; os demais 16 casos foram exercitados nesta revisão em fixtures temporárias, além das 18 fixtures manuais registradas pelo Construtor. Recomendo que o Tester torne permanente a cobertura de invalidez também para quantidade/preço e de overflow, mas não encontrei falha nesses casos.

O Chrome solicita implicitamente `/favicon.ico`, que recebe 404; o helper ignora esse pedido não referenciado. Todos os arquivos realmente referenciados pelo painel respondem 200 e o erro não afeta o uso. É uma pequena limitação da evidência (não uma falha de recurso do dashboard); pode-se adicionar ícone ou documentar/excluir explicitamente esse pedido automático.

## Comandos e evidências

- `git rev-parse --show-toplevel`, `git rev-parse HEAD`, `git status --short` — HEAD e estado descritos acima.
- `Get-FileHash ... -Algorithm SHA256` — hashes tabelados; concordam com a versão identificada pelo Tester.
- `python -m unittest discover -s tests -v` em cópia temporária do projeto e do CSV — 11/11 aprovados; evidência persistida anterior em `tests/unittest-output.txt`; evidência de navegador atual em `tests/browser-evidence.json`.
- Fixture temporária da revisão — 19/19 rejeitadas sem sobrescrita de sentinelas; temporários removidos.
- Cálculo independente — CSV original e aritmética `Decimal`; dados agregados conferem com `ANALISE.md` e JSON.

## Status

**APROVADO — Revisor, rodada 02.** A correção da rodada 01 está confirmada, os hashes/HEAD e relatórios são coerentes, a suíte completa foi repetida com navegador real e os cálculos foram conferidos independentemente. Sem pendências bloqueantes. Favicon automático 404 e oportunidade de ampliar os testes permanentes de fixtures são limitações/recomendações não impeditivas; preservar a execução registrada e a identificação por hash enquanto o projeto não tiver commit.
