# Relatório do Tester — rodada 02

## Versão testada

HEAD observado: `f16f4f85df0f2b91dfac99e17030439b7855193c`; as mudanças de produto/testes estão no diretório de trabalho, sem commit. Identificação da versão testada pelos hashes SHA-256:

| Arquivo | SHA-256 |
|---|---|
| `papelaria_limpa.csv` | `7c8222e3090cc9bb81d2b19ee43a6d7f99a6e30f7e572cd0c28de98d71cb7374` |
| `preparar_dados.py` | `b3b468d8e6e79aea74789cdc2fcb3b7bf30519eadf700696536ce6ce49714a0e` |
| `ANALISE.md` | `5a7711f880ee503a8a0424a94a2961ce1f982312fb7ad10720c0392b2de14c6a` |
| `README.md` | `a7afb03e3807e90c348fbbb4279832c4afc13ba0fcf0ac577599c246f5e907e8` |
| `dados_preparados.csv` | `2074634bfebe9e4a6cf17a36de57f22a6a880267920f917753e072da203673d4` |
| `site/index.html` | `bcb717c683bfac8938df2925cf3067aab78ac2352c0e18c35012d28faa0c71e6` |
| `site/styles.css` | `670fb693c31acbc1aa4f3d445f83a9893c8e2928f91cbde2a3b201379f9163fb` |
| `site/app.js` | `c510c116514edc582c33b4393a828a5565bb166cf1b99038418a59731284d277` |
| `site/data/vendas.json` | `4b6f0e2b49199e6c75bbbae82f9b8457089cec1f60ba99b75556532e034c777b` |
| `tests/test_dashboard.py` | `5b7b2b7798311a05a1e279ffa5d53785d3fd4d39ca5c30b27f4da4fbfcb51f0a` |
| `tests/browser_cdp.js` | `0027176f04fbaa67fb17af6812de37ffdc1bf8a46b8694cbc17417a6fef38506` |
| `tests/browser-evidence.json` | `6af251f2c704aa043b3848702acae1a37eed6165d6982f3023097b513481924a` |
| `tests/unittest-output.txt` | `3680a548c743de24fc67d827a3a6bdc831ac818b129ac12cafff50d9baa71afe` |

## Alterações e verificações desta rodada

- Acrescentei em `tests/test_dashboard.py` um teste independente que executa cópias temporárias do preparador sobre fixtures com faturamento negativo, `inf` e `NaN`; exige falha, preservação da fixture e ausência de artefatos parciais.
- Acrescentei teste específico que proíbe no README a frase obsoleta `Os testes serão adicionados pelo tester`.
- Depois das correções do Construtor, executei em `projeto-dashboard` `python -m unittest discover -s tests -v`: **11 testes passaram**, incluindo os novos testes e o navegador Chrome/CDP; 0 falhas. Repeti a suíte e gravei a saída em `tests/unittest-output.txt` (11 passaram, 25,245 s). A primeira execução direta passou em 10,482 s.
- O novo teste de preparação usa CSVs temporários independentes e confirma rejeição de faturamento negativo, `inf` e `NaN`, preservação de cada fixture e ausência de arquivos de saída parciais. O teste do README confirma que a promessa obsoleta foi removida.
- Chrome real via CDP: filtros de categoria/região, combinação, seleção vazia, limpeza, indicadores, gráficos, tabela, ausência de erros de JavaScript, teclado e layout. `tests/browser-evidence.json`: `status: PASS`; viewport 1440×1000 sem overflow (1425/1440 px) e 390×844 sem overflow (390/390 px); foco visível e filtros utilizáveis.
- `Get-FileHash ..\papelaria_limpa.csv, preparar_dados.py, ANALISE.md, README.md, dados_preparados.csv, site\index.html, site\styles.css, site\app.js, site\data\vendas.json, tests\test_dashboard.py, tests\browser_cdp.js, tests\browser-evidence.json, tests\unittest-output.txt -Algorithm SHA256` — hashes listados acima; o hash da origem permaneceu igual.
- `git rev-parse HEAD` — `f16f4f85df0f2b91dfac99e17030439b7855193c`; `git status --short` — produto não commitado, portanto hashes identificam a versão efetivamente testada.
- Ambiente: Python 3.14.8, pandas 3.0.6, Node 24.21.0, Chrome em `C:\Program Files\Google\Chrome\Application\chrome.exe`.

## Bloqueios, pendências e encaminhamento

- Nenhum bloqueio ou teste pendente para o Tester nesta versão. A fonte original e as evidências anteriores foram preservadas; a execução atualizou somente evidências dentro de `tests/`.
- Não editei produto, README ou relatórios de outros papéis. A revisão final independente desta versão cabe ao Revisor/coordenador.

## Status

**APROVADO — Tester, rodada 02.** Todos os 11 testes passaram na versão identificada pelos hashes acima, inclusive suíte completa de browser. Esta aprovação é do Tester; a aprovação conjunta depende da revisão final independente.
