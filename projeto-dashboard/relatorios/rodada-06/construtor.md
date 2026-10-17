# Relatório do Construtor — rodada 06

**Data:** 2026-10-17  
**Versão:** sem commit (diretório sem Git); hashes SHA-256 dos arquivos relevantes:

| Arquivo | SHA-256 |
|---|---|
| `site/index.html` | `28C76E0D8823D741B35BF4EFFD82903DA45A406FCEAA5B75E539EE56318265F7` |
| `site/styles.css` | `B43D8AECAE4FF4F8335189CCB84958A0009F9A37CFF23C06F6D257782FB63399` |
| `site/app.js` | `68D85D31BF00CF579CD9A323CE733F000D6B7043F994CF5E57A5BB08013C0461` |
| `site/data/vendas.json` | `4B6F0E2B49199E6C75BBBAE82F9B8457089CEC1F60BA99B75556532E034C777B` |
| `README.md` | `0268C29E753CCE508EAA41726F561B7DDFBBA1CA8A32CF9A4F0573810F8BC55B` |
| `ANALISE.md` | `3D9684F8B558965C3344B63F9059E9679876AC6E3D09D14D6ECFEDA60C5EE44A` |
| `preparar_dados.py` | `B3B468D8E6E79AEA74789CDC2FCB3B7BF30519EADF700696536CE6CE49714A0E` |
| `dados_preparados.csv` | `2074634BFEBE9E4A6CF17A36DE57F22A6A880267920F917753E072DA203673D4` |
| `banco.db` (preservado) | `3A98DD7B701C2BE40A09D6DE095BF9EAED53DFFF9D45857581DC237E2AAB190D` |
| `tests/test_dashboard.py` (inalterado) | `5B7B2B7798311A05A1E279FFA5D53785D3FD4D39CA5C30B27F4DA4FBFCB51F0A` |
| `tests/browser_cdp.js` (inalterado) | `B609378204F9682BE7C229DA850C0FBB6E9BBA85C934007CF1070397CEFF0A4A` |
| `tests/browser-evidence.json` (preservado vazio) | `E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855` |
| `papelaria_limpa.csv` (preservado) | `7C8222E3090CC9BB81D2B19EE43A6D7F99A6E30F7E572CD0C28DE98D71CB7374` |

## Alterações

- Reconectei `site/app.js` ao JSON relativo `data/vendas.json`. Categorias e regiões são obtidas dos registros carregados; filtros combinados atualizam os mesmos KPIs, gráficos e tabela. Totais e agrupamentos monetários são acumulados em centavos inteiros; seleção vazia produz zeros, gráficos vazios e a mensagem existente.
- Retirei o formulário de cadastro e seus estilos associados. Um site estático somente leitura não tem API nem persistência; manter “Salvar venda” seria oferecer uma ação que não funciona corretamente.
- Atualizei `README.md` com comandos PowerShell executáveis para criar/ativar `.venv`, instalar pandas, preparar os dados, executar testes e servir `site/` por HTTP. Identifiquei `site/` como pasta publicável.
- Reescrevi `ANALISE.md` para documentar grão, fonte, validações, regra de duplicatas, métricas, limitações e leitura estática.
- Não editei testes, CSV, relatórios anteriores, `banco.db` nem arquivos de backend. Nenhum arquivo foi removido. `tests/browser-evidence.json` foi restaurado aos bytes originais (arquivo vazio) após a execução que o helper atualiza.

## Comandos e evidências

- `node --check site/app.js` — passou.
- `C:\Users\iadados1\Documents\agentes-habilidades\.venv\Scripts\python.exe -m unittest discover -s tests -v` — primeira execução: 10 passaram e 1 falhou porque o README não continha o comando literal `python preparar_dados.py`; ajustei os comandos documentados e executei a suíte novamente.
- Reexecução integral com `C:\Users\iadados1\Documents\agentes-habilidades\.venv\Scripts\python.exe -m unittest discover -s tests -v` — **11 testes passaram**. Incluiu preparação/preservação do CSV, duplicatas e dados inválidos, totais e agrupamentos independentes, HTTP estático, comandos do README e Chrome headless/CDP para filtros isolados e combinados, resultado vazio, limpar filtros, responsividade, teclado e console sem erros JavaScript.
- A execução HTTP confirmou `/`, `/index.html`, CSS, JS e `/data/vendas.json` com HTTP 200. O Chrome também pediu `/favicon.ico` e recebeu 404; é uma requisição automática do navegador, não um recurso usado pelo painel.
- O teste de preparação validou novamente o hash original do CSV. O navegador foi executado em Chrome real; a evidência transitória impressa pelo helper não foi mantida para preservar `tests/browser-evidence.json` como estava. A saída integral dos testes foi apresentada no terminal desta rodada, não gravada sobre `tests/unittest-output.txt`.

## Limpeza legada — recomendação ao coordenador

Não apaguei o grupo `app.py`, `database.py`, `init_db.py`, `schema.sql` ou `banco.db`. Em consulta SQLite somente leitura, o banco tinha 200 vendas, todas com `origem='csv'`, e nenhuma com `origem='cadastro'`; portanto, nesta inspeção, não havia vendas cadastradas exclusivas. Ainda assim, `banco.db` contém cópia relacional, sequências e estado de migração, e poderá ganhar dados exclusivos se o backend voltar a ser usado. Recomendo arquivar esse grupo como uma unidade, fazer backup e só então removê-lo mediante decisão do coordenador/revisor. Não remover `requirements.txt` (pandas é usado pela preparação) nem `dados_preparados.csv` (saída documentada e coberta pelos testes). Caches Python regeneráveis foram deixados intactos.

## Skills e pendências

Usei as skills disponíveis `explore-data` e `data-visualization` para conferir grão/qualidade e a adequação das agregações por categoria e mês. A skill `validate-data` não está disponível; não foi usada. A checagem quantitativa registrada acima é a suíte existente, não uma revisão independente do validador.

**Status: APROVADO pelo Construtor para handoff.** Tester e Revisor ainda precisam validar formalmente esta versão; a aprovação conjunta permanece **PENDENTE**. Próximo passo: executar a revisão independente sobre os hashes desta rodada.
