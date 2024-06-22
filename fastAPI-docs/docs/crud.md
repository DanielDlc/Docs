# Criando e Estruturando rotas CRUD

## 1. Criando schemas

dentro do diretório fast_zero, crie um arquivo com nome `schemas.py`

```py title="¤ IDE"
from pydantic import BaseModel


class Message(BaseModel):
    message: str


class UserSchema(BaseModel):
    username: str
    email: str
    password: str
    
```

### 1.1 Criando import EmailStr

```py title="¤ IDE"
from pydantic import BaseModel, EmailStr


class Message(BaseModel):
    message: str


class UserSchema(BaseModel):
    username: str
    email: EmailStr
    password: str
```

## 2 Create user

```py title="¤ IDE"
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

```py title="¤ IDE"
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

### 2.2 Criar um novo Schema para omitir a senha

```py title="¤ IDE"
class UserPublic(BaseModel):
    username: str
    email: EmailStr
```