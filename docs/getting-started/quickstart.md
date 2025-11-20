# Quick Start

Build your first REROUTE application in 5 minutes!

## Step 1: Install REROUTE

```bash
pip install reroute[fastapi]
```

## Step 2: Create Project Structure

```bash
mkdir my-reroute-app
cd my-reroute-app
mkdir -p app/routes/user
```

Your structure should look like:

```
my-reroute-app/
├── app/
│   └── routes/
│       └── user/
│           └── page.py
└── main.py
```

## Step 3: Create a Route

Create `app/routes/user/page.py`:

```python
from reroute import RouteBase

class UserRoutes(RouteBase):
    """User management routes"""

    tag = "Users"

    def get(self):
        """Get all users"""
        return {
            "users": [
                {"id": 1, "name": "Alice"},
                {"id": 2, "name": "Bob"}
            ]
        }

    def post(self):
        """Create a user"""
        return {"message": "User created", "id": 3}
```

## Step 4: Create Main Application

Create `main.py`:

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

### Rate Limiting

```python
from reroute import RouteBase, rate_limit

class UserRoutes(RouteBase):
    @rate_limit("5/min")
    def post(self):
        return {"message": "User created"}
```

### Caching

```python
from reroute import RouteBase, cache

class UserRoutes(RouteBase):
    @cache(duration=60)
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
