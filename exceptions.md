# Exception Handler Global no FastAPI

## Objetivo

Centralizar o tratamento de erros da API para:

* evitar `try/except` repetido em routers
* padronizar respostas
* separar regras de negócio de HTTP
* manter logs organizados
* facilitar manutenção

---

# Problema sem Exception Handler

## Router

```python
@router.post("/")
def create_user(
    dto: CreateUserDTO,
    db: Session = Depends(get_db)
):

    try:

        return UserService.create(
            db,
            dto
        )

    except ValueError as e:

        raise HTTPException(
            status_code=400,
            detail=str(e)
        )

    except PermissionError:

        raise HTTPException(
            status_code=403,
            detail="Sem permissão"
        )

    except Exception:

        raise HTTPException(
            status_code=500,
            detail="Erro interno do servidor"
        )
```

### Problemas

* repetição
* muito boilerplate
* difícil manutenção
* tratamento espalhado
* routers poluídos

---

# Solução: Exception Handler Global

---

# Estrutura sugerida

```text
app/
├── exceptions/
│   ├── base.py
│   ├── user.py
│   └── permission.py
│
├── handlers/
│   └── exception_handlers.py
```

---

# 1. Criando uma exception base

## `exceptions/base.py`

```python
class AppException(Exception):

    def __init__(
        self,
        detail: str,
        status_code: int
    ):

        self.detail = detail
        self.status_code = status_code

        super().__init__(detail)
```

---

# 2. Criando exceptions específicas

## `exceptions/user.py`

```python
from app.exceptions.base import AppException


class UserAlreadyExistsError(AppException):

    def __init__(self):

        super().__init__(
            detail="Email já cadastrado",
            status_code=400
        )


class UserNotFoundError(AppException):

    def __init__(self):

        super().__init__(
            detail="Usuário não encontrado",
            status_code=404
        )
```

---

## `exceptions/permission.py`

```python
from app.exceptions.base import AppException


class PermissionDeniedError(AppException):

    def __init__(self):

        super().__init__(
            detail="Sem permissão",
            status_code=403
        )
```

---

# 3. Criando o exception handler

## `handlers/exception_handlers.py`

```python
from fastapi import Request
from fastapi.responses import JSONResponse

from app.exceptions.base import AppException


async def app_exception_handler(
    request: Request,
    exc: AppException
):

    return JSONResponse(
        status_code=exc.status_code,
        content={
            "detail": exc.detail
        }
    )
```

---

# 4. Registrando o handler no FastAPI

## `main.py`

```python
from fastapi import FastAPI

from app.exceptions.base import AppException

from app.handlers.exception_handlers import (
    app_exception_handler
)

app = FastAPI()


app.add_exception_handler(
    AppException,
    app_exception_handler
)
```

---

# 5. Usando no service

## `services/user_service.py`

```python
from app.exceptions.user import (
    UserAlreadyExistsError
)


class UserService:

    @staticmethod
    def create(
        db: Session,
        dto: CreateUserDTO
    ):

        existing_user = (
            UserRepository.get_by_email(
                db,
                dto.email
            )
        )

        if existing_user:

            raise UserAlreadyExistsError()

        ...
```

---

# 6. Router limpo

## `routers/user_router.py`

```python
@router.post(
    "/",
    response_model=UserResponseDTO,
    status_code=201
)
def create_user(
    dto: CreateUserDTO,
    db: Session = Depends(get_db)
):

    return UserService.create(
        db,
        dto
    )
```

---

# Fluxo completo

```text
Router
    ↓
Service
    ↓
raise UserAlreadyExistsError()
    ↓
FastAPI Exception Handler
    ↓
JSONResponse
```

---

# Resposta da API

## Response

```json
{
  "detail": "Email já cadastrado"
}
```

## Status

```http
400 Bad Request
```

---

# Exemplo de chamada

## Request

```http
POST /users/
```

```json
{
  "nome": "Luna",
  "email": "luna@email.com",
  "senha": "123456"
}
```

---

## Resposta de erro

```json
{
  "detail": "Email já cadastrado"
}
```

---

# Tratando erros internos

Você também pode criar:

```python
class InternalServerError(AppException):

    def __init__(self):

        super().__init__(
            detail="Erro interno do servidor",
            status_code=500
        )
```

---

# Logs continuam no service

## Exemplo

```python
except SQLAlchemyError:

    db.rollback()

    logger.exception(
        "Erro SQLAlchemy ao criar usuário"
    )

    raise InternalServerError()
```

---

# Benefícios da abordagem

| Benefício                      | Resultado                         |
| ------------------------------ | --------------------------------- |
| Routers limpos                 | menos boilerplate                 |
| Padronização                   | respostas consistentes            |
| Separação de responsabilidades | HTTP separado da regra de negócio |
| Escalabilidade                 | fácil adicionar novas exceptions  |
| Segurança                      | evita expor erro interno          |
| Observabilidade                | logs centralizados                |

---

# Padrão recomendado

## Router

Responsável apenas por:

* HTTP
* request
* response_model

---

## Service

Responsável por:

* regras de negócio
* transação
* exceptions

---

## Exception Handler

Responsável por:

* converter exception em resposta HTTP

---

# Resultado final

Uma arquitetura muito mais:

* limpa
* escalável
* profissional
* reutilizável
* fácil de manter

Especialmente em projetos grandes usando FastAPI + SQLAlchemy + Pydantic.
