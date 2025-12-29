# fastapi-recipes


A curated collection of task-specific FastAPI recipes and snippets. Practical, solutions for common tasks like authentication, header manipulation, and database integration. Updated as I learn (TIL).

---

simple snippets for accessing different parts of an HTTP request in FastAPI.

Setup Include these imports for all examples:

```
from typing import Annotated
from fastapi import FastAPI, Header
from pydantic import BaseModel

app = FastAPI()

```


## 1. Fetch Path Parameter

FastAPI detects path parameters when the function argument matches a variable in the route path (inside {}).

```
@app.get("/items/{item_id}")
def read_path(item_id: int):
    return {"path_value": item_id}

```


## 2. Fetch Header Value

Use Header() to tell FastAPI to look in the HTTP headers. 

**NOTE** : FastAPI converts the header name (e.g., User-Agent) to snake_case (user_agent).

```
@app.get("/headers/")
def read_header(user_agent: Annotated[str | None, Header()] = None):
    return {"header_value": user_agent}
```

## 3. Fetch Query Parameter

Any function argument that is not a path parameter and is a simple type (like int, str) is automatically interpreted as a query parameter.

```
# URL example: /query/?q=test

@app.get("/query/")
def read_query(q: str):
    return {"query_value": q}

```

## 4. Fetch Request Body

Declare a Pydantic model and use it as a type hint. FastAPI interprets Pydantic models as the request body.

```
class Item(BaseModel):
    name: str
    price: float

@app.post("/body/")
def read_body(item: Item):
    return {"body_received": item}
```
