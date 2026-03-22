# Python API Development Reference

## FastAPI (preferred for new APIs)

```python
from fastapi import FastAPI, HTTPException, Depends
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    value: float

@app.get("/items/{item_id}")
async def get_item(item_id: int):
    if item_id < 0:
        raise HTTPException(status_code=400, detail="Invalid ID")
    return {"id": item_id}
```

## Auth pattern (JWT)

```python
from fastapi.security import HTTPBearer

security = HTTPBearer()

@app.get("/protected")
async def protected_route(token: str = Depends(security)):
    # validate token
    ...
```

## Environment config

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    db_host: str
    db_password: str
    secret_key: str

    class Config:
        env_file = ".env"

settings = Settings()
```

Never pass `settings` objects to templates or logs — they contain secrets.
