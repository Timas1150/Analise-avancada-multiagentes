# Dashboard estático de vendas — Windows e VS Code

O dashboard lê os dados preparados de `site/data/vendas.json`. É somente leitura: não cadastra nem salva vendas. Filtros por categoria e região atualizam juntos os indicadores, os gráficos e a tabela. Faturamento é valor vendido, não lucro.

## Preparar, testar e abrir

Abra o terminal do VS Code nesta pasta (`projeto-dashboard`) e execute os comandos no PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python preparar_dados.py
python -m unittest discover -s tests -v
python -m http.server 8000 --directory site
```

Mantenha o último comando em execução e abra <http://127.0.0.1:8000>. Para parar o servidor, pressione `Ctrl+C` no terminal. Se já tiver um ambiente virtual configurado, também pode executar os comandos com `python`.

Se o PowerShell bloquear a ativação, execute `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` nessa janela e repita o comando de ativação.

O script lê `papelaria_limpa.csv` na pasta acima do projeto, sem alterar o original, e gera `dados_preparados.csv` e `site/data/vendas.json`. É necessário instalar pandas somente na primeira configuração.

## Publicação estática

`site/` é a pasta publicável em uma hospedagem estática: publique o conteúdo dessa pasta mantendo `data/vendas.json` dentro de `data/`. Não é necessário nem utilizado servidor de aplicação ou banco de dados pelo dashboard. O cadastro de vendas foi retirado porque uma página estática não pode persistir registros.

Os arquivos legados `app.py`, `database.py`, `init_db.py`, `schema.sql` e `banco.db` foram preservados; não fazem parte dos comandos acima nem da aplicação estática. Consulte `ANALISE.md` para a definição dos dados e as regras de preparação.
