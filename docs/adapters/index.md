# Adapters

REROUTE adapters integrate file-based routing with different Python web frameworks.

## Available Adapters

<div class="grid cards" markdown>

-   :zap: [**FastAPI**](fastapi.md)

    ---

    High-performance async framework with automatic API docs

-   :material-flask: [**Flask**](flask.md)

    ---

    Lightweight and flexible WSGI framework

-   :material-triangle: [**Django**](django.md)

    ---

    Full-featured framework (coming soon)

</div>

## How Adapters Work

Adapters translate REROUTE's file-based routes into framework-specific routes:

```python
from reroute.adapters import FastAPIAdapter

adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir=Path(__file__).parent / "app"
)
adapter.register_routes()
```

## Common Features

All adapters support:

- File-based routing discovery
- HTTP method mapping (GET, POST, PUT, DELETE, etc.)
- Lifecycle hooks (before_request, after_request, on_error)
- Decorator integration
- Custom configuration

## Choosing an Adapter

| Feature | FastAPI | Flask | Django |
|---------|---------|-------|--------|
| Async Support | Yes | Partial | Yes |
| Auto API Docs | Yes | No | No |
| Performance | Excellent | Good | Good |
| Learning Curve | Medium | Low | High |
| REROUTE Support | Full | Full | Coming Soon |

## Framework-Specific Guides

- [FastAPI Integration](fastapi.md) - Setup, async routes, OpenAPI
- [Flask Integration](flask.md) - Setup, blueprints, extensions
- [Django Integration](django.md) - Coming soon

## Custom Adapters

You can create custom adapters for other frameworks. See the [API Reference](../api/adapters.md) for the adapter interface.
