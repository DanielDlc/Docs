## Configurando o Ambiente para o Projeto fast_zero

### 1. Criar diretório 📂

```powershell title="$ Execução no terminal!"
mkdir fast_zero
```

### 2. Instalar Ferramentas Necessárias 🖥️

### 2.1 Instalar `pyenv`

Para instalar o `pyenv`, siga as instruções no [site oficial](https://pypi.org/project/pyenv-win/).

```powershell title="$ Execução no terminal!"
Invoke-WebRequest -UseBasicParsing -Uri "https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1" -OutFile "./install-pyenv-win.ps1"; &"./install-pyenv-win.ps1"
```

### 2.2 Instalar `pipx`

```powershell title="$ Execução no terminal!"
pip install pipx
```

### 2.3 Instalar `poetry`

```powershell title="$ Execução no terminal!"
pipx install poetry
```

Para atualizar o `poetry`, utilize:

```powershell title="$ Execução no terminal!"
pipx upgrade poetry
```

### 2.4 Instalar `ignr` para criar `.gitignore`

```powershell title="$ Execução no terminal!"
pipx install ignr
```

### 3. Iniciando com `poetry` 🗃️

### 3.1 Criar um novo projeto

```powershell title="$ Execução no terminal!"
poetry new fast_zero
```

### 3.2 Instalar dependências do projeto

```powershell title="$ Execução no terminal!"
poetry install
```

Caso não funcione, utilize:

```powershell title="$ Execução no terminal!"
poetry lock
```

### 3.3 Adicionar dependências

Para instalar a biblioteca `fastapi`:

```powershell title="$ Execução no terminal!"
poetry add fastapi
```

Se o comando acima não funcionar, atualize com:

```powershell title="$ Execução no terminal!"
poetry update fastapi
```

Para instalar a última versão disponível:

```powershell title="$ Execução no terminal!"
poetry add fastapi@latest
```

### 3.4 Verificar todas as bibliotecas instaladas

```powershell title="$ Execução no terminal!"
poetry show
```

### 3.5 Executar o arquivo `.py`

Antes de executar, ative o ambiente virtual:

```powershell title="$ Execução no terminal!"
poetry shell
```

Para executar o arquivo principal `app.py`:

```powershell title="$ Execução no terminal!"
uvicorn app:app --reload
```

### 3.6 Exibir diretório criado no Windows 11

```powershell title="$ Execução no terminal!"
tree /f
```

**Observação**: O arquivo `poetry.lock` pode ser adicionado ao GitHub (remova do `.gitignore`).

### 4. Documentação do Swagger 🌐

### 4.1 Verifique se seu ambiente está ativado, caso precise ativar:

```powershell title="$ Execução no terminal!"
poetry shell
```

### 4.2 execute sua aplicação com

```powershell title="$ Execução no terminal!"
fastapi dev fast_zero/app.py
```

### 4.3 Acesse a documentação do Swagger e ReDoc quando estiver servindo app:

- [Swagger](http://127.0.0.1:8000/docs)
- [ReDoc](http://127.0.0.1:8000/redoc)

### 5. Instalando Ferramentas Úteis para FastAPI

### 5.1 Instalar `Ruff` (Linter)

Adicione `Ruff` como dependência de desenvolvimento:

```powershell title="$ Execução no terminal!"
poetry add --group dev ruff
```

### 5.2 Configurar `Ruff`

Adicione as seguintes configurações no arquivo `pyproject.toml`:

### 5.3. Configurar limite de linha e excluir migrações

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.ruff]
line-length = 79
extend-exclude = ['migrations']
```

### 5.4. Selecionar linters

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.ruff.lint]
preview = true
select = ['I', 'F', 'E', 'W', 'PL', 'PT']
```

### 5.5 Formatador de código

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.ruff.format]
preview = true
quote-style = 'single'
```

### 5.6 Verificar o código

```powershell title="$ Execução no terminal!"
ruff check .
```

### 5.7 Organizar o código

```powershell title="$ Execução no terminal!"
ruff check . --fix
```

### 5.8 Formatar o código

```powershell title="$ Execução no terminal!"
ruff format .
```

Quando tudo estiver correto, aparecerá a mensagem: "all checks passed!".

### 6. Pytest

### 6.1 Instalar `Pytest` (testar código)

```powershell title="$ Execução no terminal!"
poetry add --group dev pytest pytest-cov
```

### 6.2 Adicionar no arquivo `toml`

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.pytest.ini_options]
pythonpath = "."
addopts = '-p no:warnings'
```

### 6.3 Verificar o que está ou não testado especificando o diretório

```powershell title="$ Execução no terminal!"
pytest --cov=fast_zero
```

### 6.4 Gerar arquivo HTML para verificar os testes

```powershell title="$ Execução no terminal!"
coverage html
```

### 7. Taskipy

### 7.1 Instalar Taskipy para lembrar de comandos

Adicione essa dependência no `poetry`:

```powershell title="$ Execução no terminal!"
poetry add --group dev taskipy
```

### 7.2 Criar task no arquivo `pyproject.toml title=" ⚙ Arquivo pyproject.toml"`

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.taskipy.tasks]
run = 'fastapi dev fast_zero/app.py'
```

### 7.3 Rodar um comando simples para executar o projeto

```powershell title="$ Execução no terminal!"
task run
```

Ao invés de:

```powershell title="$ Execução no terminal!"
fastapi dev fast_zero/app.py
```

### 7.4 Editar o arquivo `pyproject.toml` para incluir testes

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.taskipy.tasks]
run = 'fastapi dev fast_zero/app.py'
test = 'pytest -s -x --cov=fast_zero -vv'
```

### 7.5 Adicionar lint (podemos concatenar entre outras coisas)

No comando lint abaixo, repare que utilizei dois "&&". Caso esteja no Mac e Linux, pode utilizar ";".

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.taskipy.tasks]
lint = 'ruff check . && ruff check . --diff'
format = 'ruff check . --fix && ruff format .'
```

### 7.6 Executar cadeia de comandos para executar os testes

Vamos editar o arquivo novamente para verificar se está tudo certo.

```toml title=" ⚙ Arquivo pyproject.toml"
pre_test = 'task lint'
test = 'pytest -s -x --cov=fast_zero -vv'
post_test = 'coverage html'
```

### 7.8 Arquivo completo `pyproject.toml`

```toml title=" ⚙ Arquivo pyproject.toml"
[tool.taskipy.tasks]
run = 'fastapi dev fast_zero/app.py'
pre_test = 'task lint'
test = 'pytest -s -x --cov=fast_zero -vv'
post_test = 'coverage html'
lint = 'ruff check . && ruff check . --diff'
format = 'ruff check . --fix && ruff format .'
```

### 7.9 Ver todos os comandos task

```powershell title="$ Execução no terminal!"
task --list
```

### 8. Criar um Arquivo de Testes

### 8.1 Criar um arquivo no diretório `tests` com o nome abaixo:

```
touch test_app.py
```

### 8.2 Estrutura de um Teste

- Organizar (Arrange)
- Agir (Act)
- Afirmar (Assert)
- Teardown
