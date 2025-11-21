# Quick Start

Build your first REROUTE application in 2 minutes using CLI commands!

## Step 1: Install REROUTE

```bash
pip install reroute[fastapi]
```

## Step 2: Initialize Your Project

Use the REROUTE CLI to create your project structure automatically:

```bash
reroute init my-reroute-app --framework fastapi
cd my-reroute-app
```

This creates a complete project structure with everything you need:

```
my-reroute-app/
├── app/
│   ├── __init__.py
│   └── routes/
│       └── __init__.py
├── main.py          # Auto-generated entry point
├── .gitignore
└── requirements.txt
```

## Step 3: Generate a Route

Use the CLI to generate your first route:

```bash
reroute create route --path /user --name User --methods GET,POST
```

This creates `app/routes/user/page.py` with GET and POST methods:

```python
from reroute import RouteBase
from reroute.decorators import rate_limit, cache

class UserRoutes(RouteBase):
    """UserRoutes handles /user endpoints."""

    tag = "User"

    def __init__(self):
        super().__init__()
        self.data = []

    @cache(duration=60)
    def get(self):
        """Handle GET requests - retrieve user"""
        return {
            "users": [
                {"id": 1, "name": "Alice"},
                {"id": 2, "name": "Bob"}
            ]
        }

    @rate_limit("10/min")
    def post(self):
        """Handle POST requests - create new user"""
        return {"message": "User created", "id": 3}
```

**Tip:** Edit the generated file to customize the response data!

## Step 4: Your Application is Ready!

The `reroute init` command already created `main.py` for you:

```python
from fastapi import FastAPI
from reroute.adapters import FastAPIAdapter
from pathlib import Path

app = FastAPI(title="My REROUTE App")

adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir=Path(__file__).parent / "app"
)
adapter.register_routes()

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

**No manual setup required!** Everything is configured and ready to run.

## Step 5: Run the Application

```bash
python main.py
```

Visit:

- API Docs: [http://localhost:8000/docs](http://localhost:8000/docs)
- Users endpoint: [http://localhost:8000/user](http://localhost:8000/user)

## Step 6: Test Your API

```bash
# Get users
curl http://localhost:8000/user

# Create user
curl -X POST http://localhost:8000/user
```

## Add More Features

### Customize Your Generated Routes

The generated route already includes decorators! Edit `app/routes/user/page.py` to customize:

### Rate Limiting

```python
from reroute import RouteBase
from reroute.decorators import rate_limit

class UserRoutes(RouteBase):
    @rate_limit("5/min")  # Limit to 5 requests per minute
    def post(self):
        return {"message": "User created"}
```

### Caching

```python
from reroute import RouteBase
from reroute.decorators import cache

class UserRoutes(RouteBase):
    @cache(duration=60)  # Cache for 60 seconds
    def get(self):
        return {"users": [...]}
```

### Lifecycle Hooks

```python
class UserRoutes(RouteBase):
    def before_request(self):
        print("Authenticating user...")
        return None  # Continue to handler

    def after_request(self, response):
        response["timestamp"] = "2024-01-15T10:30:00Z"
        return response
```

## Next Steps

<div class="grid cards" markdown>

-   :material-file-tree: [**File-based Routing**](../guides/file-routing.md)

    ---

    Learn how folder structure maps to routes

-   :material-alpha-d-box: [**Decorators**](../guides/decorators.md)

    ---

    Master rate limiting, caching, and more

-   :material-api: [**API Reference**](../api/index.md)

    ---

    Explore all REROUTE features

</div>
