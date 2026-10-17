# Relatório do Tester — rodada 01

## Versão examinada

Repositório em `f16f4f85df0f2b91dfac99e17030439b7855193c`; os artefatos examinados estavam como arquivos de trabalho não commitados. Hashes SHA-256:

| Arquivo | SHA-256 |
|---|---|
| `papelaria_limpa.csv` (origem) | `7c8222e3090cc9bb81d2b19ee43a6d7f99a6e30f7e572cd0c28de98d71cb7374` |
| `preparar_dados.py` | `098dcd3bc936fdfdf6cdfa16dd831bae06fc5e43f1cef49414ad39bb7bf8f088` |
| `ANALISE.md` | `0c709d59216522642e86b2c8bbebe1fdb95491bcd8cef7822120f8c261975b05` |
| `README.md` | `c56a41cbde5d3009783b0a8eb557be20ae5a4bb3924077245962d305d5de5e86` |
| `dados_preparados.csv` | `2074634bfebe9e4a6cf17a36de57f22a6a880267920f917753e072da203673d4` |
| `site/index.html` | `bcb717c683bfac8938df2925cf3067aab78ac2352c0e18c35012d28faa0c71e6` |
| `site/styles.css` | `670fb693c31acbc1aa4f3d445f83a9893c8e2928f91cbde2a3b201379f9163fb` |
| `site/app.js` | `c510c116514edc582c33b4393a828a5565bb166cf1b99038418a59731284d277` |
| `site/data/vendas.json` | `4b6f0e2b49199e6c75bbbae82f9b8457089cec1f60ba99b75556532e034c777b` |
| `tests/test_dashboard.py` | `dce877f9287870d9f937b4b3daada4e063395df7113cd1b3198ed307b2bf18a2` |
| `tests/browser_cdp.js` | `0027176f04fbaa67fb17af6812de37ffdc1bf8a46b8694cbc17417a6fef38506` |

## O que foi conferido

- Conferi a preservação do CSV por SHA-256 antes/depois da preparação; hash permaneceu igual. A suíte confirma 200 linhas, colunas e tipos esperados, nenhum valor ausente, 200 IDs únicos, ausência de duplicatas exatas e rejeição explícita de IDs repetidos em fixture, sem descarte silencioso.
- Recalculei totais e grupos do CSV de origem independentemente, com `Decimal`: 200 vendas, faturamento de R$ 11.546,80 e ticket de R$ 57,734 (R$ 57,73 arredondado na interface). Para os três primeiros registros, confirmei 9 unidades e R$ 108,90. Comparei os cálculos com CSV preparado e JSON.
- Testei por Chrome real via CDP: indicadores, agrupamentos dos dois gráficos e IDs da tabela; categoria, região, filtros combinados, seleção vazia e limpeza; execução sem exceções JavaScript; viewport 1440×1000 e 390×844; navegação por Tab, foco visível e reset por teclado. Ampliei `tests/browser_cdp.js` para falhar também quando `NaN`/`Infinity` aparecem no texto renderizado.
- HTTP: página e arquivos CSS, JavaScript e JSON respondem 200; recurso inexistente responde 404; também testei o comando HTTP documentado no README. Chrome registrou apenas o pedido automático não referenciado de `/favicon.ico` (404); nenhum recurso usado pelo site falhou.

## Comandos, resultados e evidências

- Ambiente: Python 3.14.8, pandas 3.0.6, Node 24.21.0 e Google Chrome instalado em `C:\Program Files\Google\Chrome\Application\chrome.exe`; Playwright/Selenium Python não instalados. O helper usa CDP nativo do Chrome e WebSocket do Node, sem dependências externas.
- Em `projeto-dashboard`, executei `python -m unittest discover -s tests -v` (captura equivalente em `tests/unittest-output.txt`): **9 testes passaram**, 0 falhas, em 5,695 s.
- Evidências do navegador: `tests/browser-evidence.json` — `status: PASS`, inclui verificações de filtros, projeções, responsividade, teclado e ausência de erros; viewport pequeno não teve overflow (390/390 px), filtros mediram 334×42 px e o viewport grande também coube (1425/1440 px).
- A execução da suíte chama `preparar_dados.py`, que regenerou os dados. Os hashes de ambos os artefatos preparados permaneceram iguais aos da versão examinada.

## Problemas, encaminhamentos e pendências

- Nenhum defeito reproduzível encontrado; sem encaminhamento ao Construtor.
- Verificações obrigatórias executadas, inclusive navegador real; nenhuma pendente ou bloqueada.

## Status

**APROVADO — Tester, rodada 01.** Aprovação limitada à versão e aos hashes acima; a aprovação conjunta e a revisão independente cabem ao coordenador e ao Revisor.
