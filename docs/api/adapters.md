# Adapters API

Framework adapter interfaces.

## FastAPIAdapter

Integrate REROUTE with FastAPI.

```python
from reroute.adapters import FastAPIAdapter

adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir=path,
    config=MyConfig
)
adapter.register_routes()
```

**Parameters:**
- `fastapi_app` (FastAPI): FastAPI application instance
- `app_dir` (Path): Application directory
- `config` (Config): Configuration class

## FlaskAdapter

Integrate REROUTE with Flask (Coming Soon).

```python
from reroute.adapters import FlaskAdapter

adapter = FlaskAdapter(
    flask_app=app,
    app_dir=path,
    config=MyConfig
)
adapter.register_routes()
```

**Parameters:**
- `flask_app` (Flask): Flask application instance
- `app_dir` (Path): Application directory
- `config` (Config): Configuration class

## Base Adapter

Create custom adapters by extending `BaseAdapter`.

```python
from reroute.adapters import BaseAdapter

class MyAdapter(BaseAdapter):
    def register_routes(self):
        # Implementation
        pass
```

## Methods

### register_routes()
Register all discovered routes with the framework.

```python
adapter.register_routes()
```

### discover_routes()
Scan and discover route files.

```python
routes = adapter.discover_routes()
```
