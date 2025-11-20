# FastAPI Adapter

Integrate REROUTE with FastAPI for high-performance async APIs.

## Quick Start

```python
from fastapi import FastAPI
from reroute.adapters import FastAPIAdapter
from pathlib import Path

app = FastAPI(title="My API")

adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir=Path(__file__).parent / "app"
)
adapter.register_routes()
```

## Features

- Automatic OpenAPI documentation
- Async route support
- Request/response models with Pydantic
- Dependency injection
- WebSocket support

## Configuration

```python
from reroute import Config

class FastAPIConfig(Config):
    PORT = 8000
    API_BASE_PATH = "/api/v1"
    ENABLE_CORS = True
```

## Async Routes

```python
from reroute import RouteBase

class AsyncRoutes(RouteBase):
    async def get(self):
        # Async operations
        return {"data": "async"}
```

## Learn More

- [FastAPI Documentation](https://fastapi.tiangolo.com)
- [Getting Started](../getting-started/quickstart.md)
