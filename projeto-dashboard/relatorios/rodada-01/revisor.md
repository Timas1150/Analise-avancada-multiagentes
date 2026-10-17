# Relatório do Revisor — rodada 01

## Versão examinada

O ambiente anunciado à sessão dizia que a raiz não era Git; nesta execução, porém, `git rev-parse --show-toplevel` retornou `C:/Users/iadados1/Documents/agentes-habilidades` e `git rev-parse HEAD` retornou `f16f4f85df0f2b91dfac99e17030439b7855193c`. `git status --short` mostra `?? AGENTS.md` e `?? projeto-dashboard/`: os artefatos examinados não estão commitados. Portanto, o hash citado pelo Tester existe e é o HEAD observado, mas o registro do Construtor (“não há commit”) e os metadados do ambiente descrevem outro contexto/estado; a cronologia não pode ser confirmada daqui.

Hashes SHA-256 observados na versão revisada:

| Arquivo | SHA-256 |
|---|---|
| `papelaria_limpa.csv` | `7c8222e3090cc9bb81d2b19ee43a6d7f99a6e30f7e572cd0c28de98d71cb7374` |
| `preparar_dados.py` | `098dcd3bc936fdfdf6cdfa16dd831bae06fc5e43f1cef49414ad39bb7bf8f088` |
| `ANALISE.md` | `0c709d59216522642e86b2c8bbebe1fdb95491bcd8cef7822120f8c261975b05` |
| `README.md` | `c56a41cbde5d3009783b0a8eb557be20ae5a4bb3924077245962d305d5de5e86` |
| `site/app.js` | `c510c116514edc582c33b4393a828a5565bb166cf1b99038418a59731284d277` |
| `site/data/vendas.json` | `4b6f0e2b49199e6c75bbbae82f9b8457089cec1f60ba99b75556532e034c777b` |
| `tests/test_dashboard.py` | `dce877f9287870d9f937b4b3daada4e063395df7113cd1b3198ed307b2bf18a2` |
| `tests/browser_cdp.js` | `0027176f04fbaa67fb17af6812de37ffdc1bf8a46b8694cbc17417a6fef38506` |
| `tests/browser-evidence.json` (após repetição) | `6af251f2c704aa043b3848702acae1a37eed6165d6982f3023097b513481924a` |

## O que foi conferido

- Li preparação, análise, documentação, HTML/CSS/JS, testes, helper CDP e relatórios do Construtor e Tester. Os três arquivos de skill requeridos (`explore-data`, `data-visualization`, `validate-data`) não existem em `.agents/skills/`; não afirmo tê-los usado.
- Recalculei diretamente do CSV com `csv` e `Decimal`, sem usar pandas: 200 linhas/IDs, zero campos vazios, total R$ 11.546,80, ticket R$ 57,734, exemplo das três primeiras linhas: 9 unidades e R$ 108,90. Conferi também os agrupamentos por categoria e mês; batem com a soma total e com a implementação/evidência. O hash da fonte continuou igual.
- Reexecutei `python -m unittest discover -s tests -v` em `projeto-dashboard/`: **9 testes passaram**, 0 falhas, 5,608 s. Reexecutou também o Chrome headless/CDP integrado; `tests/browser-evidence.json` registrou PASS, viewport 1440×1000 e 390×844 sem overflow, navegação por teclado e projeções. A checagem cobre filtros isolados/combinados, seleção vazia, reset, tabela, gráficos, indicadores e console.
- Reproduzi defeito do preparador em diretório temporário: alterei somente uma cópia fixture do CSV para `faturamento=-28.80` na primeira venda e executei o script copiado. Resultado: código de saída 0, CSV e JSON foram gerados, soma fonte R$ 11.489,20 e uma diferença de faturamento registrada. A fonte real não foi alterada.

## Problemas e encaminhamento

1. **REPROVADO — Construtor:** `ANALISE.md` (linhas 14 e 28) afirma validar valores numéricos positivos; `preparar_dados.py` (linhas 61–66) só exige positividade para `quantidade` e `preco_unitario`, aceitando faturamento negativo. Validar também `faturamento > 0` (e valores finitos) ou documentar uma regra diferente fundamentada. O Tester deve adicionar fixture regressiva negativa/infinita e executá-la após a correção.
2. **REPROVADO — Construtor:** `README.md`, linha 16, ainda diz “Os testes serão adicionados pelo tester”, embora `tests/` contenha a suíte e o comando esteja documentado. Remover a promessa futura e revisar a sequência de instruções.
3. **Tester:** a suíte passa, mas não detecta nenhum dos dois problemas acima. Acrescentar verificações: validação dos valores numéricos conforme a regra documentada e ausência da afirmação obsoleta no README. Reexecutar a suíte após correções.
4. Relatórios divergem sobre a presença de commit (Construtor: nenhum; Tester: `f16f…`). O hash do Tester é verificável no ambiente atual e seus hashes de arquivos principais coincidem; ainda assim, registrar que os arquivos estavam não commitados e explicar a diferença de contexto/tempo na próxima rodada. Os artefatos sem commit ficam identificados pelos hashes desta revisão.
5. As skills mandatórias estão ausentes; informar a limitação ao coordenador e não marcar uso como realizado.

## Comandos e evidências

- `git rev-parse --show-toplevel; git rev-parse HEAD; git status --short` — raiz e HEAD acima; produto não commitado.
- `Get-FileHash ... -Algorithm SHA256` — hashes listados acima; arquivo fonte corresponde ao hash registrado pelo Construtor/Tester.
- `python -m unittest discover -s tests -v` — 9/9 aprovados; evidência detalhada no resultado do comando desta revisão e em `tests/unittest-output.txt` (captura anterior). Teste de navegador atualizado em `tests/browser-evidence.json`.
- Fixture negativa temporária — preparador aceitou faturamento negativo; nenhum artefato da fixture foi criado no projeto.
- Cálculo independente — CSV original; total e agrupamentos concordam com `ANALISE.md` e JSON.

## Pendências e status

**REPROVADO — Revisor, rodada 01.** Há defeito reproduzível de validação e texto desatualizado no README; encaminhar correções ao Construtor e cobertura ao Tester. Reexecutar toda a suíte e revisar os novos hashes/evidências na próxima rodada. A revisão de cálculos, funcionamento do navegador e layout passou, mas isso não aprova a entrega conjunta. Skill ausente permanece limitação registrada.
