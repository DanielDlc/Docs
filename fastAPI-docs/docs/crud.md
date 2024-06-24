# Criando e Estruturando rotas CRUD

## 1. Criando schemas

dentro do diretório fast_zero, crie um arquivo com nome `schemas.py`

```python title="➾ fast_zero/schemas.py"
from pydantic import BaseModel


class Message(BaseModel):
    message: str
```

### 1.1 Schemas para Cadastro de Usuário

```python title="➾ fast_zero/schemas.py" hl_lines="8 9 10 11" linenums="15"
from pydantic import BaseModel


class Message(BaseModel):
    message: str


class UserSchema(BaseModel):
    username: str
    email: str
    password: str
    
```

### 1.2 Criando Import EmailStr

Validando o formato que caracteriza o email utilizando `EmailStr`

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/schemas.py"
    
    from pydantic import BaseModel, EmailStr


    class Message(BaseModel):
        message: str


    class UserSchema(BaseModel):
        username: str
        email: EmailStr
        password: str


    class UserPublic(BaseModel):
        username: str
          email: EmailStr
    ```

```python title="➾ fast_zero/schemas.py" hl_lines="1 3" linenums="15"
from pydantic import BaseModel, EmailStr
...
    email: EmailStr
    ...
```

## 2 Create user

Criando uma rota trazendo `users`

```python title="➾ fast_zero/app.py"
from http import HTTPStatus

from fastapi import FastAPI

from fast_zero.schemas import Message

app = FastAPI()


@app.get('/', status_code=HTTPStatus.OK, response_model=Message)
def read_root():
    return {'message': 'Olá, Mundo!'}


@app.post('/users/')
def create_user():
    ...
```

### 2.1 Importando UserSchema

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/app.py"
    
    from http import HTTPStatus

    from fastapi import FastAPI

    from fast_zero.schemas import Message, UserSchema

    app = FastAPI()


    @app.get('/', status_code=HTTPStatus.OK, response_model=Message)
    def read_root():
        return {'message': 'Olá, Mundo!'}


    @app.post('/users/', response_model=UserSchema)
    def create_user(user: UserSchema):
        return user
    ```

```python title="➾ fast_zero/app.py" hl_lines="2 4 5 6" linenums="15"
...
from fast_zero.schemas import Message, UserSchema
...
@app.post('/users/', response_model=UserSchema)
def create_user(user: UserSchema):
    return user
```

### 2.2 Criar um novo Schema para omitir a senha `UserPublic`

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/schemas.py"

    from pydantic import BaseModel, EmailStr


    class Message(BaseModel):
        message: str


    class UserSchema(BaseModel):
        username: str
        email: EmailStr
        password: str


    class UserPublic(BaseModel):
        username: str
        email: EmailStr
    ```

```python title="➾ fast_zero/schemas.py" hl_lines="2 3 4" linenums="15"
...
class UserPublic(BaseModel):
    username: str
    email: EmailStr
```

### 2.3 Importando `UserPublic`

Receberemos um usuário com o modelo `UserSchema`, que está retornando a senha. No entanto, como estamos enviando ele para o `response_model`, ele está sendo processado no arquivo `schemas.py` pelo modelo `UserPublic` na saída, removendo o campo `password` da resposta.

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/app.py"

    from http import HTTPStatus

    from fastapi import FastAPI

    from fast_zero.schemas import Message, UserSchema, UserPublic

    app = FastAPI()


    @app.get('/', status_code=HTTPStatus.OK, response_model=Message)
    def read_root():
        return {'message': 'Olá, Mundo!'}


    @app.post('/users/', response_model=UserPublic)
    def create_user(user: UserSchema):
        return user
    ```

```python title="➾ fast_zero/app.py" hl_lines="2 3 4" linenums="15"
...
from fast_zero.schemas import Message, UserSchema, UserPublic
...
@app.post('/users/', response_model=UserPublic)
...
```

### 2.4 Incluindo Resposta do Status Code 201 Created

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/app.py"

    from http import HTTPStatus

    from fastapi import FastAPI

    from fast_zero.schemas import Message, UserSchema, UserPublic

    app = FastAPI()


    @app.get('/', status_code=HTTPStatus.OK, response_model=Message)
    def read_root():
        return {'message': 'Olá, Mundo!'}


    @app.post('/users/', status_code=HTTPStatus.CREATED, response_model=UserPublic)
    def create_user(user: UserSchema):
        return user
    ```

```python title="➾ fast_zero/app.py" hl_lines="2" linenums="15"
...
@app.post('/users/', status_code=HTTPStatus.CREATED, response_model=UserPublic)
...
```

## 3 Criando um Banco de Dados

Crie um pequeno banco de dados para exibir o CRUD.

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/app.py"

    from http import HTTPStatus

    from fastapi import FastAPI

    from fast_zero.schemas import Message, UserSchema, UserPublic

    app = FastAPI()

    database = []


    @app.get('/', status_code=HTTPStatus.OK, response_model=Message)
    def read_root():
        return {'message': 'Olá, Mundo!'}


    @app.post('/users/', status_code=HTTPStatus.CREATED, response_model=UserPublic)
    def create_user(user: UserSchema):
        return user
    ```


```python title="➾ fast_zero/app.py" hl_lines="2" linenums="15"
...
database = []
...
```

### 3.1  Criar Identificador dentro da API

Crie um identificador na API em UserPublic. Crie um novo schema com o nome UserDB, herdando de UserSchema.

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/schemas.py"
    
    from pydantic import BaseModel, EmailStr


    class Message(BaseModel):
       message: str


    class UserSchema(BaseModel):
       username: str
       email: EmailStr
       password: str

    class UserDB(UserSchema):
       id: int
       

    class UserPublic(BaseModel):
       id: int
       username: str
       email: EmailStr
    ```

```python title="➾ fast_zero/schemas.py" hl_lines="2 3 6" linenums="15"
...
    class UserDB(UserSchema):
        id: int

    class UserPublic(BaseModel):
       id: int
       username: str
       email: EmailStr
```

### 3.2 Criar e Importar UserDB

Crie e importe UserDB, crie um modelo user_with_id e caracterize um id. Converta um objeto do Pydantic em um dicionário e retorne user_with_id. Adicione um registro no banco de dados para ficar dinâmico.

??? tip "Clique para ver o código completo:"

    ```python title="➾ fast_zero/app.py"

    from http import HTTPStatus

    from fastapi import FastAPI

    from fast_zero.schemas import (
        Message, UserSchema, UserPublic, UserDB
    )

    app = FastAPI()

    database = []


    @app.get('/', status_code=HTTPStatus.OK, response_model=Message)
    def read_root():
        return {'message': 'Olá, Mundo!'}


    @app.post('/users/', status_code=HTTPStatus.CREATED, response_model=UserPublic)
    def create_user(user: UserSchema):

        user_with_id = UserDB(
            id=len(database) + 1, 
            **user.model_dump()
        )

        database.append(user_with_id)

        return user_with_id
    ```

```python title="➾ fast_zero/app.py" hl_lines="2 6 7 8 9 10 12 14" linenums="15"

...
from fast_zero.schemas import (Message, UserSchema, UserPublic, UserDB)
...
    @app.post('/users/', status_code=HTTPStatus.CREATED, response_model=UserPublic)
    def create_user(user: UserSchema):

        user_with_id = UserDB(
            id=len(database) + 1, 
            **user.model_dump()
        )

        database.append(user_with_id)

        return user_with_id
```        

## 4 Criando testes
