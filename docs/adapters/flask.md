# Flask Adapter

!!! warning "Not Yet Available"
    The Flask adapter is **not yet implemented**. This page shows planned features for a future release.
    **Current Status:** In development
    **Available Now:** [FastAPI Adapter](fastapi.md)

## Status

The Flask adapter is currently in development and will be available in a future release.

## Planned Features

- Blueprint integration
- Flask extension compatibility
- Session management
- Template rendering support
- Simple decorator-based routing

## Quick Preview

```python
from flask import Flask
from reroute.adapters import FlaskAdapter
from pathlib import Path

app = Flask(__name__)
adapter = FlaskAdapter(
    flask_app=app,
    app_dir=Path(__file__).parent / "app"
)
adapter.register_routes()
```

## Stay Updated

Watch the [GitHub repository](https://github.com/cbsajan/reroute) for updates on Flask support.

- [Flask Documentation](https://flask.palletsprojects.com)
- [Getting Started](../getting-started/quickstart.md)
