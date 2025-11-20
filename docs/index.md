# Welcome to REROUTE

**Modern file-based routing for Python backends**

---

Here are the introductory sections and the tutorials to learn **REROUTE**.

You could consider this **a book**, **a course**, the **official** and recommended way to learn REROUTE. 🎓

REROUTE brings the simplicity and elegance of Next.js-style file-based routing to Python web frameworks. Build clean, maintainable APIs with zero configuration.

## Why REROUTE?

<div class="grid cards" markdown>

-   :material-file-tree: **File-based Routing**

    ---

    Organize routes by folders and files, just like Next.js.

    No more manual route registration!

-   :material-language-python: **Class-based Handlers**

    ---

    HTTP methods as class methods: `get()`, `post()`, `put()`, `delete()`

    Clean, intuitive, and Pythonic

-   :material-speedometer: **Powerful Decorators**

    ---

    Built-in rate limiting, caching, validation, and more

    Extend with custom decorators easily

-   :material-grid: **Multi-framework Support**

    ---

    Works with FastAPI, Flask, Django, and more

    Switch frameworks without rewriting routes

</div>

## Quick Example

Create a file-based route in 3 steps:

### 1. Install REROUTE

```bash
pip install reroute
```

### 2. Create a route file

```python title="app/routes/user/page.py"
from reroute import RouteBase, cache, rate_limit

class UserRoutes(RouteBase):
    tag = "Users"

    @cache(duration=60)
    def get(self):
        return {"users": ["Alice", "Bob"]}

    @rate_limit("5/min")
    def post(self):
        return {"message": "User created"}
```

### 3. Register with your framework

=== "FastAPI"

    ```python title="main.py"
    from fastapi import FastAPI
    from reroute.adapters import FastAPIAdapter
    from pathlib import Path

    app = FastAPI()
    adapter = FastAPIAdapter(
        fastapi_app=app,
        app_dir=Path(__file__).parent / "app"
    )
    adapter.register_routes()
    ```

=== "Flask"

    ```python title="app.py"
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

That's it! Your routes are now available at `/api/v1/user` with automatic OpenAPI documentation.

## Features

### File-based Routing
Organize your API by filesystem structure. Each folder becomes a route segment:

```
app/routes/
├── user/
│   └── page.py        → /user
└── product/
    ├── page.py        → /product
    └── [id]/
        └── page.py    → /product/:id
```

### Decorators

Built-in decorators for common API patterns:

- `@rate_limit("5/min")` - Rate limiting per IP/user
- `@cache(duration=60)` - Response caching
- `@requires("admin")` - Role-based access
- `@validate(schema)` - Request validation
- `@timeout(5)` - Request timeout
- `@log_requests()` - Automatic logging

### Lifecycle Hooks

Control request/response flow with lifecycle methods:

```python
class MyRoute(RouteBase):
    def before_request(self):
        # Authentication, validation
        pass

    def after_request(self, response):
        # Add headers, transform response
        return response

    def on_error(self, error):
        # Custom error handling
        return {"error": str(error)}
```

### Custom Configuration

Fine-tune REROUTE's behavior:

```python
from reroute import Config

class MyConfig(Config):
    PORT = 8000
    API_BASE_PATH = "/api/v1"
    ENABLE_CORS = True
    CORS_ALLOW_ORIGINS = ["https://example.com"]
    VERBOSE_LOGGING = True
```

## What's Next?

<div class="grid cards" markdown>

-   :rocket: [**Quick Start**](getting-started/quickstart.md)

    ---

    Get up and running in 5 minutes

-   :books: [**Guides**](guides/index.md)

    ---

    Deep dives into routing, decorators, and more

-   :material-api: [**API Reference**](api/index.md)

    ---

    Complete API documentation

-   :material-code-braces: [**Examples**](examples/index.md)

    ---

    Real-world patterns and recipes

</div>

## Community & Support

- **GitHub**: [github.com/cbsajan/reroute](https://github.com/cbsajan/reroute)
- **PyPI**: [pypi.org/project/reroute](https://pypi.org/project/reroute)
- **Issues**: [Report bugs or request features](https://github.com/cbsajan/reroute/issues)
- **Contribute**: [Contributing Guide](contributing.md)

## License

REROUTE is released under the [Apache License 2.0](https://github.com/cbsajan/reroute/blob/main/LICENSE).
