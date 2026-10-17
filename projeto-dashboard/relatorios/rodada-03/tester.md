# Relatório do Tester — rodada 03

## Versão examinada

HEAD observado `f16f4f85df0f2b91dfac99e17030439b7855193c`; `projeto-dashboard/` continua sem commit. Os hashes abaixo identificam a versão visual do Construtor e os artefatos usados na execução final:

| Arquivo | SHA-256 |
|---|---|
| `papelaria_limpa.csv` | `7c8222e3090cc9bb81d2b19ee43a6d7f99a6e30f7e572cd0c28de98d71cb7374` |
| `preparar_dados.py` | `b3b468d8e6e79aea74789cdc2fcb3b7bf30519eadf700696536ce6ce49714a0e` |
| `ANALISE.md` | `5a7711f880ee503a8a0424a94a2961ce1f982312fb7ad10720c0392b2de14c6a` |
| `README.md` | `a7afb03e3807e90c348fbbb4279832c4afc13ba0fcf0ac577599c246f5e907e8` |
| `dados_preparados.csv` | `2074634bfebe9e4a6cf17a36de57f22a6a880267920f917753e072da203673d4` |
| `site/index.html` | `bc4dc910a146d4aab0c877d96810c63cb8a91878227f1d6b7c4c894058201212` |
| `site/styles.css` | `60cb27ae2f1bda9b63cc9c6cb510142cc58c78faa83e13ee1c715595a9c934c8` |
| `site/app.js` | `c510c116514edc582c33b4393a828a5565bb166cf1b99038418a59731284d277` |
| `site/data/vendas.json` | `4b6f0e2b49199e6c75bbbae82f9b8457089cec1f60ba99b75556532e034c777b` |
| `tests/test_dashboard.py` | `5b7b2b7798311a05a1e279ffa5d53785d3fd4d39ca5c30b27f4da4fbfcb51f0a` |
| `tests/browser_cdp.js` | `b609378204f9682be7c229da850c0fbb6e9bba85c934007cf1070397ceff0a4a` |
| `tests/browser-evidence.json` | `0a56b25e19db746102be43aed6e4b39e35408b993b99cf4750d6f6e44ebd8770` |
| `tests/unittest-output.txt` | `0e905d833459fa76d2ba78a734a1afdb5952e354ac0872dda6a2b8b69153332e` |
| `relatorios/rodada-03/construtor.md` | `5a10cff77db9552b3d460aecf527bd8bfdfa1c1b7dbb6060b74f80bf645d575b` |

## Cobertura e resultados

- Mantive a suíte existente de dados, cálculos, HTTP, filtros, tela vazia/reset e correspondência dos artefatos; `app.js` e `tests/test_dashboard.py` não foram alterados. Verifiquei também que não há interface de login/cadastro ou controles de autenticação neste escopo público.
- Ampliei `tests/browser_cdp.js` para conferir em Chrome real, por estilos computados e interação CDP: tokens de paleta e acento vermelho/âmbar; wordmark textual “Painel de vendas” sem logotipo gráfico; legibilidade dos rótulos; `:focus-visible`; hover/active do botão e selects; hover dos cards de filtros, KPI, gráfico e tabela; `prefers-reduced-motion`; layout desktop/mobile; e projeções dos indicadores, gráficos e tabela após a sequência visual.
- Contrastes WCAG recalculados independentemente a partir das cores computadas; todos os valores alegados pelo Construtor conferem (tolerância 0,02): texto principal/creme 14,81:1; secundário/branco 7,16:1; vermelho/branco 7,05:1; vermelho profundo/branco 9,91:1; nota do KPI 6,52:1; estado vazio 10,19:1; erro 9,40:1; cabeçalho de tabela 10,53:1; borda do select 4,56:1; barra âmbar/trilho 4,35:1. Pares de texto atingem ≥4,5:1 e a barra gráfica ≥3:1.
- Estados reais aprovados: botão hover vermelho/branco e active vermelho profundo com pressão; select hover e active diferenciados; os quatro tipos de card recebem realce de hover. Botão e filtros exibem foco visível vermelho de 3 px/offset 3 px; o wordmark apresenta foco visível de 3 px em vermelho profundo, com contraste ≥3:1.
- Movimento reduzido: media query reconhecida; transições/animações computadas em 0,01 ms e rolagem `auto`. Desktop 1440 px sem overflow (1425 px), dois gráficos em colunas; mobile 390 px sem overflow, filtros/gráficos empilhados, filtro 334×42 px e rótulos legíveis (12 px nos filtros, mínimo 10 px nos gráficos).
- Execução final, cwd `projeto-dashboard`: `python -m unittest discover -s tests -v`, com saída capturada em `tests/unittest-output.txt` — **11 testes passaram, 0 falhas, 25,482 s**, incluindo o Chrome/CDP. Evidência estruturada em `tests/browser-evidence.json`, `status: PASS`, sem erros JS. `node --check tests/browser_cdp.js` — passou.
- O hash do CSV de origem permaneceu igual. Chrome solicita `/favicon.ico` implicitamente (404), mas a página, CSS, JavaScript e JSON referenciados respondem 200; nenhum recurso necessário falha.

## Ajustes de teste e encaminhamentos

- Nas primeiras execuções, corrigi apenas o helper de teste: par de contraste secundário selecionava o card errado; foco via teclado assumia wrap do Tab; estados active eram lidos antes do fim da transição; card da tabela estava fora do viewport. Ajustei seletor, sequência de foco, espera da transição e rolagem do alvo. Repeti a suíte completa após esses ajustes; o resultado final acima passou. Nenhuma falha de produto foi reproduzida.
- Não editei HTML, CSS, JavaScript, dados ou relatórios de outros papéis. Sem defeitos encaminhados ao Construtor; sem pendências ou bloqueios do Tester.

## Comandos e ambiente

- `python -m unittest discover -s tests -v` (cwd `projeto-dashboard`; chamada via `cmd.exe /d /c` para gravar a saída em `tests/unittest-output.txt`) — 11/11 aprovados.
- `node --check tests/browser_cdp.js` — aprovado.
- `Get-FileHash ..\papelaria_limpa.csv, preparar_dados.py, ANALISE.md, README.md, dados_preparados.csv, site\index.html, site\styles.css, site\app.js, site\data\vendas.json, tests\test_dashboard.py, tests\browser_cdp.js, tests\browser-evidence.json, tests\unittest-output.txt, relatorios\rodada-03\construtor.md -Algorithm SHA256` — hashes acima.
- Ambiente: Python 3.14.8, pandas 3.0.6, Node 24.21.0, Chrome `C:\Program Files\Google\Chrome\Application\chrome.exe`.

## Status

**APROVADO — Tester, rodada 03.** Os 11 testes, inclusive a regressão visual em browser real e a verificação independente dos contrastes, passaram sobre a versão identificada acima. Aprovação conjunta e revisão independente continuam a cargo do Revisor/coordenador.
