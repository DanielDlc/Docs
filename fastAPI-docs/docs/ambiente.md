## Configurando o Ambiente para o Projeto fast_zero

### 1. Criar diretório 📂

```powershell title="$ Execução no terminal!"
mkdir fast_zero
```

### 2. Instalar Ferramentas Necessárias 🖥️

### 2.1 Instalar `pyenv`

Para instalar o `pyenv`, siga as instruções no [site oficial](https://pypi.org/project/pyenv-win/).

```powershell
Invoke-WebRequest -UseBasicParsing -Uri "https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1" -OutFile "./install-pyenv-win.ps1"; &"./install-pyenv-win.ps1"
```

### 2.2 Instalar `pipx`

```bash
pip install pipx
```

### 2.3 Instalar `poetry`

```bash
pipx install poetry
```

Para atualizar o `poetry`, utilize:

```bash
pipx upgrade poetry
```

### 2.4 Instalar `ignr` para criar `.gitignore`

```bash
pipx install ignr
```

### 3. Iniciando com `poetry` 🗃️

### 3.1 Criar um novo projeto

```bash
poetry new fast_zero
```

### 3.2 Instalar dependências do projeto

```bash
poetry install
```

Caso não funcione, utilize:

```bash
poetry lock
```

### 3.3 Adicionar dependências

Para instalar a biblioteca `fastapi`:

```bash
poetry add fastapi
```

Se o comando acima não funcionar, atualize com:

```bash
poetry update fastapi
```

Para instalar a última versão disponível:

```bash
poetry add fastapi@latest
```

### 3.4 Verificar todas as bibliotecas instaladas

```bash
poetry show
```

### 3.5 Executar o arquivo `.py`

Antes de executar, ative o ambiente virtual:

```bash
poetry shell
```

Para executar o arquivo principal `app.py`:

```bash
uvicorn app:app --reload
```

### 3.6 Exibir diretório criado no Windows 11

```bash
tree /f
```

**Observação**: O arquivo `poetry.lock` pode ser adicionado ao GitHub (remova do `.gitignore`).

### 4. Documentação do Swagger 🌐

### 4.1 Verifique se seu ambiente está ativado, caso precise ativar:

```bash
poetry shell
```

### 4.2 execute sua aplicação com

```bash
fastapi dev fast_zero/app.py
```

### 4.3 Acesse a documentação do Swagger e ReDoc quando estiver servindo app:

- [Swagger](http://127.0.0.1:8000/docs)
- [ReDoc](http://127.0.0.1:8000/redoc)

### 5. Instalando Ferramentas Úteis para FastAPI

### 5.1 Instalar `Ruff` (Linter)

Adicione `Ruff` como dependência de desenvolvimento:

```bash
poetry add --group dev ruff
```

### 5.2 Configurar `Ruff`

Adicione as seguintes configurações no arquivo `pyproject.toml`:

### 5.3. Configurar limite de linha e excluir migrações

```toml
[tool.ruff]
line-length = 79
extend-exclude = ['migrations']
```

### 5.4. Selecionar linters

```toml
[tool.ruff.lint]
preview = true
select = ['I', 'F', 'E', 'W', 'PL', 'PT']
```

### 5.5 Formatador de código

```toml
[tool.ruff.format]
preview = true
quote-style = 'single'
```

### 5.6 Verificar o código

```bash
ruff check .
```

### 5.7 Organizar o código

```bash
ruff check . --fix
```

### 5.8 Formatar o código

```bash
ruff format .
```

Quando tudo estiver correto, aparecerá a mensagem: "all checks passed!".

### 6. Pytest

### 6.1 Instalar `Pytest` (testar código)

```bash
poetry add --group dev pytest pytest-cov
```

### 6.2 Adicionar no arquivo `toml`

```toml
[tool.pytest.ini_options]
pythonpath = "."
addopts = '-p no:warnings'
```

### 6.3 Verificar o que está ou não testado especificando o diretório

```bash
pytest --cov=fast_zero
```

### 6.4 Gerar arquivo HTML para verificar os testes

```bash
coverage html
```

### 7. Taskipy

### 7.1 Instalar Taskipy para lembrar de comandos

Adicione essa dependência no `poetry`:

```bash
poetry add --group dev taskipy
```

### 7.2 Criar task no arquivo `pyproject.toml`

```toml
[tool.taskipy.tasks]
run = 'fastapi dev fast_zero/app.py'
```

### 7.3 Rodar um comando simples para executar o projeto

```bash
task run
```

Ao invés de:

```bash
fastapi dev fast_zero/app.py
```

### 7.4 Editar o arquivo `pyproject.toml` para incluir testes

```toml
[tool.taskipy.tasks]
run = 'fastapi dev fast_zero/app.py'
test = 'pytest -s -x --cov=fast_zero -vv'
```

### 7.5 Adicionar lint (podemos concatenar entre outras coisas)

No comando lint abaixo, repare que utilizei dois "&&". Caso esteja no Mac e Linux, pode utilizar ";".

```toml
[tool.taskipy.tasks]
lint = 'ruff check . && ruff check . --diff'
format = 'ruff check . --fix && ruff format .'
```

### 7.6 Executar cadeia de comandos para executar os testes

Vamos editar o arquivo novamente para verificar se está tudo certo.

```toml
pre_test = 'task lint'
test = 'pytest -s -x --cov=fast_zero -vv'
post_test = 'coverage html'
```

### 7.8 Arquivo completo `pyproject.toml`

```toml
[tool.taskipy.tasks]
run = 'fastapi dev fast_zero/app.py'
pre_test = 'task lint'
test = 'pytest -s -x --cov=fast_zero -vv'
post_test = 'coverage html'
lint = 'ruff check . && ruff check . --diff'
format = 'ruff check . --fix && ruff format .'
```

### 7.9 Ver todos os comandos task

```bash
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

### 9.0 Link para o Curso completo

|               Lindo direto para o material               |                                        Curso FastAPI                                         |
| :------------------------------------------------------: | :------------------------------------------------------------------------------------------: |
| [FastAPI do Zero](https://fastapidozero.dunossauro.com/) | [Playlist Youtube](https://www.youtube.com/playlist?list=PLOQgLBuj2-3IuFbt-wJw2p2NiV9WTRzIP) |
