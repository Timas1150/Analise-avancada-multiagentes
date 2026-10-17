# Relatório final — dashboard de vendas

## Status conjunto: **APROVADO**

Construtor, Tester e Revisor aprovaram a versão da **rodada 03**. O `HEAD` é `f16f4f85df0f2b91dfac99e17030439b7855193c`; os arquivos do dashboard seguem não commitados, identificados pelos hashes dos relatórios.

## Entrega

- Pipeline reproduzível em Python/pandas: `preparar_dados.py`; metodologia e limitações em `ANALISE.md`.
- Site estático em `site/`, responsivo, com três indicadores, dois gráficos, tabela, filtros por categoria e região, botão de limpar e mensagem de resultado vazio. `site/` é a pasta publicável em hospedagem estática.
- Design autoral em vermelho, amarelo-âmbar, branco e creme, sem logotipo; controles com estados de interação e foco visível, com movimento reduzido respeitado.
- **Login e cadastro não foram implementados**, conforme confirmado pelo usuário.
- Fonte preservada: `papelaria_limpa.csv`, com 200 vendas/IDs únicos e faturamento total de **R$ 11.546,80**. Faturamento não representa lucro.

## Verificações e evidências

- Suíte completa: `python -m unittest discover -s tests -v` — **11 testes passaram**, inclusive testes de dados, filtros, servidor HTTP e navegador Chrome real via CDP.
- Layout verificado em 1440×1000 e 390×844, sem overflow. Hover/active, navegação por teclado, foco visível e `prefers-reduced-motion` aprovados.
- Tester e Revisor recalcularam os contrastes WCAG; todos os pares alegados conferem. Indicadores, gráficos, tabela e filtros mantiveram o comportamento.
- Revisor repetiu os 11 testes em cópia temporária e aprovou a mesma versão. Os hashes dos arquivos estão em `relatorios/rodada-03/construtor.md`, `tester.md` e `revisor.md`; saída e evidência do navegador em `tests/unittest-output.txt` e `tests/browser-evidence.json`.
- As skills `explore-data`, `data-visualization` e `validate-data` foram usadas pelo harness, a partir de `.opencode/skills/`; o caminho legado `.agents/skills/` não existe.

Observação não bloqueante: o Chrome requisita `/favicon.ico` automaticamente e recebe 404; isso não afeta os arquivos usados pelo dashboard. A verificação visual cobre os viewports registrados, não todos os tamanhos/níveis de zoom nem uma auditoria com leitor de tela.

## Como abrir

No terminal do VS Code, abra a pasta `projeto-dashboard` e execute estes comandos, um por vez:

```powershell
python -m pip install -r requirements.txt
python preparar_dados.py
python -m unittest discover -s tests -v
python -m http.server 8000 --directory site
```

Mantenha o último comando em execução e abra <http://localhost:8000>. A publicação não faz parte desta etapa.
