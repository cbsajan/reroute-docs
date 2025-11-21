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

    Works with FastAPI (Flask & Django coming soon)

    Switch frameworks without rewriting routes

</div>

## Quick Example

Get started in 2 minutes using CLI commands:

### 1. Install & Initialize

```bash
pip install reroute[fastapi]
reroute init my-app --framework fastapi
cd my-app
```

### 2. Generate a Route

```bash
reroute create route --path /user --name User --methods GET,POST
```

This creates `app/routes/user/page.py`:

```python
from reroute import RouteBase
from reroute.decorators import cache, rate_limit

class UserRoutes(RouteBase):
    tag = "User"

    @cache(duration=60)
    def get(self):
        return {"users": ["Alice", "Bob"]}

    @rate_limit("5/min")
    def post(self):
        return {"message": "User created"}
```

### 3. Run Your API

```bash
python main.py  # Already created by init!
```

That's it! Your routes are now available at `/user` with automatic OpenAPI documentation at `/docs`.

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

- `@rate_limit("5/min")` - Rate limiting per IP/user ✅ **Available**
- `@cache(duration=60)` - Response caching ✅ **Available**
- `@requires("admin")` - Role-based access 🚧 **Coming Soon**
- `@validate(schema)` - Request validation 🚧 **Coming Soon**
- `@timeout(5)` - Request timeout 🚧 **Coming Soon**
- `@log_requests()` - Automatic logging 🚧 **Coming Soon**

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

    Get up and running in 2 minutes

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
