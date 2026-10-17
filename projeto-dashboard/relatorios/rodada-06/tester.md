# Relatório do Tester — rodada 06

**Data:** 2026-10-17  
**Status do Tester: APROVADO**  
**Versão examinada:** sem commit (a pasta não é repositório Git). Os hashes do produto `site/index.html`, `site/styles.css`, `site/app.js` e `site/data/vendas.json` conferem com a versão entregue pelo Construtor na rodada-06.

## Versão e hashes SHA-256

| Arquivo | SHA-256 |
|---|---|
| `site/index.html` | `28C76E0D8823D741B35BF4EFFD82903DA45A406FCEAA5B75E539EE56318265F7` |
| `site/styles.css` | `B43D8AECAE4FF4F8335189CCB84958A0009F9A37CFF23C06F6D257782FB63399` |
| `site/app.js` | `68D85D31BF00CF579CD9A323CE733F000D6B7043F994CF5E57A5BB08013C0461` |
| `site/data/vendas.json` | `4B6F0E2B49199E6C75BBBAE82F9B8457089CEC1F60BA99B75556532E034C777B` |
| `tests/test_dashboard.py` (inalterado) | `5B7B2B7798311A05A1E279FFA5D53785D3FD4D39CA5C30B27F4DA4FBFCB51F0A` |
| `tests/browser_cdp.js` (cobertura ampliada nesta rodada) | `4455F95D016FBE2B5C40F0A9D0EEF810C4F4A49F0A1FDB8F6B027DD23B313735` |
| `tests/browser-evidence.json` (evidência nova) | `1E7D0FBB895D4D7FECDFECE05B190B46604C382F5D663C8595AC9F1B79A3F9EA` |
| `relatorios/rodada-06/unittest-output.txt` (log novo) | `5EAF1BB5C025F53E139B1E382A86621F4F0E9527F5C0DCD33CC7694A87FFE644` |
| `preparar_dados.py` | `B3B468D8E6E79AEA74789CDC2FCB3B7BF30519EADF700696536CE6CE49714A0E` |
| `dados_preparados.csv` | `2074634BFEBE9E4A6CF17A36DE57F22A6A880267920F917753E072DA203673D4` |
| `papelaria_limpa.csv` (original preservado) | `7C8222E3090CC9BB81D2B19EE43A6D7F99A6E30F7E572CD0C28DE98D71CB7374` |

## Alteração de cobertura e justificativa

Não alterei arquivos do produto nem `tests/test_dashboard.py`. Ampliei somente `tests/browser_cdp.js` porque a verificação anterior comparava a UI com o JSON lido diretamente do disco, sem provar que a página havia requisitado esse arquivo, e comparava na tabela somente IDs.

O teste de navegador agora confirma uma única requisição real a `/data/vendas.json`, status HTTP 200 e MIME JSON; compara o corpo recebido pelo Chrome com o payload em disco e reprova qualquer chamada a `/api/`. A projeção da interface compara todas as células da tabela e a contagem de resultados, além de verificar mensagem de vazio e ausência de erro de carregamento. A ampliação foi executada em Chrome real e passou.

## Comandos e resultados

Comando integral executado de `projeto-dashboard`:

```powershell
& 'C:\Users\iadados1\Documents\agentes-habilidades\.venv\Scripts\python.exe' -m unittest discover -s tests -v
```

**Resultado:** `Ran 11 tests ... OK`; todos os 11 testes passaram na execução final. A saída nova está em `relatorios/rodada-06/unittest-output.txt`. O arquivo antigo `tests/unittest-output.txt` não foi usado nem alterado.

Coberturas aprovadas:

- Integridade do CSV original antes/depois da preparação, colunas, tipos, ausências, IDs e duplicatas; rejeição de duplicatas, faturamento negativo e valores não finitos.
- Totais e agregações recalculados independentemente, exemplo conhecido, correspondência CSV/JSON e valores finitos.
- Servidor HTTP: página, CSS, JS e JSON servidos; rota inexistente retorna 404; sintaxe JavaScript conferida por `node --check`.
- Comando de servidor documentado no README testado na porta 8000.
- Chrome headless/CDP: fetch estático efetivo validado contra o arquivo JSON e sem chamadas de API; KPIs, gráficos e todas as células da tabela comparados com os dados esperados; filtros isolados e combinados; seleção vazia, mensagem de vazio, limpar filtros e foco; ausência de `NaN`/`Infinity` e de erros de carregamento/console/rede.
- Viewports desktop `1440px` e móvel `390px`, sem overflow; filtros e gráficos responsivos; navegação por teclado e foco visível conferidos.

Evidências do navegador foram geradas novamente nesta execução em `tests/browser-evidence.json` (`status=PASS`, Chrome headless via CDP; 15 verificações). Medidas registradas: desktop `1425/1440px`, móvel `390/390px`; Tab chegou ao filtro de região. O 404 de `/favicon.ico` é a requisição automática conhecida do Chrome e é excluído da validação de erros; a requisição intencional a `/missing.js` também é testada como 404 esperado.

## Problemas, pendências e encaminhamento

- Nenhuma falha ou verificação obrigatória pendente para a função de Tester.
- Não editei arquivos do Construtor. A aprovação conjunta do projeto depende da revisão independente do Revisor e da consolidação pelo coordenador.

**Conclusão:** esta versão da rodada-06 está **APROVADA pelo Tester**.
