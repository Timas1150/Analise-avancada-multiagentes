# Relatório do Construtor — rodada 03

## Versão

HEAD observado: `f16f4f85df0f2b91dfac99e17030439b7855193c`. `projeto-dashboard/` permanece não commitado/não rastreado. O CSS e o HTML abaixo representam esta rodada; JavaScript e dados foram preservados.

| Arquivo | SHA-256 |
|---|---|
| [`site/styles.css`](../../site/styles.css) | `60cb27ae2f1bda9b63cc9c6cb510142cc58c78faa83e13ee1c715595a9c934c8` |
| [`site/index.html`](../../site/index.html) | `bc4dc910a146d4aab0c877d96810c63cb8a91878227f1d6b7c4c894058201212` |
| [`site/app.js`](../../site/app.js), não alterado | `c510c116514edc582c33b4393a828a5565bb166cf1b99038418a59731284d277` |

## Decisões visuais e acessibilidade

- Atualizei apenas `site/styles.css` e ajustes textuais de apresentação em `site/index.html`. Troquei a antiga marca/monograma fictícios do cabeçalho por “Painel de vendas”; não usei logotipo, marca real ou elemento copiado. Conteúdo, rótulos, dados, IDs, filtros e estrutura funcional foram mantidos.
- Identidade editorial própria em vermelho profundo, amarelo-âmbar e branco/creme, com tipografia de sistema, linha de acento no topo e KPI principal vermelho. Barras usam vermelho e âmbar e mantêm rótulos numéricos, sem depender apenas da cor.
- Controles: botão tem hover vermelho, active vermelho profundo com leve pressão e foco de teclado visível; selects têm borda inicial escura, hover/active diferenciados e foco visível de 3 px. O link do cabeçalho também tem `:focus-visible`.
- Cards informativos recebem somente realce sutil de hover (borda/sombra), sem cursor ou estilo de clique: não são controles e não há abas no markup. Não foi adicionada ação falsa nem comportamento JS.
- Transições são curtas; `prefers-reduced-motion: reduce` reduz transições/animações para `0.01 ms` e desativa rolagem suave.
- Contrastes calculados pela fórmula WCAG: texto principal/creme **14,81:1**; texto secundário/branco **7,16:1**; vermelho/branco **7,05:1**; vermelho profundo/branco **9,91:1**; texto do KPI/branco quente sobre vermelho **6,52:1**; estado vazio **10,19:1**; erro **9,40:1**; cabeçalho da tabela **10,53:1**. A borda dos selects/branco é **4,56:1**. A barra âmbar/trilho é **4,35:1** como contraste gráfico, acima de 3:1.

## Comandos e validações

- Servi `site/` por HTTP com `python -m http.server 8884 --directory site` e executei [`tests/browser_cdp.js`](../../tests/browser_cdp.js) diretamente via Node/Chrome headless, sem executar o teste unittest que grava evidência em `tests/`. Resultado: **PASS**, sem exceções JS; filtros isolados/combinados, estado vazio, reset, foco, layout e navegação por teclado mantiveram o comportamento.
- Dimensões verificadas pelo navegador: desktop **1440×1000**, largura de conteúdo 1425 px sem overflow e dois gráficos em colunas; celular **390×844**, largura 390 px sem overflow, gráficos/filtros empilhados, filtro 334×42 px e rótulos 12 px (gráfico 10 px).
- Teste visual via CDP/Node inline contra HTTP `8885`: **PASS**. Confirmou hover/active do botão, hover do select e card, e emulação de `prefers-reduced-motion` (`match: true`, transição computada `1e-05s`). Resultado capturado no terminal, resumido aqui; não criei arquivo de evidência adicional.
- Calculei os contrastes listados acima com uma verificação Python inline da fórmula de luminância relativa WCAG.
- Evidências de implementação: [`site/index.html`](../../site/index.html), [`site/styles.css`](../../site/styles.css); evidência funcional reutilizada: [`tests/browser_cdp.js`](../../tests/browser_cdp.js). Não alterei `site/app.js`, `tests/` nem relatórios de outras rodadas.

## Pendências e status

- **PENDENTE — Tester:** executar a suíte completa e registrar evidência da versão visual desta rodada, inclusive a checagem responsive/teclado existente. A evidência CDP desta rodada ficou no relatório/saída do comando, sem sobrescrever `tests/browser-evidence.json`.
- Nenhum bloqueio conhecido. Dados, agregações e filtros não foram modificados.

**APROVADO — construção visual pelo Construtor; pronto para Tester.** Isto não é aprovação conjunta nem substitui a revisão do Revisor.
