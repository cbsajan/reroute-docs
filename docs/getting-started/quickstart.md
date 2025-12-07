# Quick Start

Build your first REROUTE application in 2 minutes using CLI commands!

## Step 1: Create Virtual Environment

=== "UV (Recommended)"

    ```bash
    # Create virtual environment
    uv venv

    # Activate (Windows)
    .venv\Scripts\activate

    # Activate (Linux/Mac)
    source .venv/bin/activate
    ```

=== "Traditional pip"

    ```bash
    # Create virtual environment
    python -m venv venv

    # Activate (Windows)
    venv\Scripts\activate

    # Activate (Linux/Mac)
    source venv/bin/activate
    ```

## Step 2: Install REROUTE

=== "UV"

    ```bash
    uv add reroute[fastapi]
    ```

=== "pip"

    ```bash
    pip install reroute[fastapi]
    ```

## Step 3: Initialize Your Project

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
│   ├── routes/
│   │   ├── __init__.py
│   │   └── root/
│   │       └── page.py         # Root and health endpoints
│   ├── config.py               # Configuration with secure defaults
│   ├── logger.py               # Logging setup
│   └── models/
│       └── __init__.py
├── main.py                     # Clean adapter entry point
├── pyproject.toml              # Modern dependency management
├── .env.example                # Environment template
└── .gitignore
```

## Step 4: Install Dependencies

=== "UV (Default)"

    ```bash
    uv sync
    ```

=== "pip"

    ```bash
    pip install -r requirements.txt
    ```

!!! info "UV is Default"
    The `reroute init` command uses UV as the default package manager. Use `--package-manager pip` for traditional pip.

## Step 5: Configure Environment

Copy the environment template and configure:

```bash
cp .env.example .env
```

The `.env` file includes secure defaults:
```env
# Server Configuration
DEBUG=False
HOST=127.0.0.1

# Security
SECRET_KEY=your-secret-key-here
DATABASE_URL=sqlite:///./app.db

# Prevent .egg-info creation
REROUTE_CREATE_EGG_INFO=False
```

## Step 6: Generate a Route

Use the CLI to generate your first route:

```bash
reroute create route --path /user --name User --methods GET,POST
```

This creates `app/routes/user/page.py` with GET and POST methods:

```python
from reroute import RouteBase
from reroute.decorators import rate_limit, cache
from logger import get_logger

logger = get_logger(__name__)

class UserRoutes(RouteBase):
    """UserRoutes handles /user endpoints."""

    tag = "User"

    def __init__(self):
        super().__init__()
        self.data = []

    @cache(duration=60)
    def get(self):
        """Handle GET requests - retrieve user"""
        logger.info("GET request to /user")
        return {
            "users": [
                {"id": 1, "name": "Alice"},
                {"id": 2, "name": "Bob"}
            ]
        }

    @rate_limit("10/min")
    def post(self):
        """Handle POST requests - create new user"""
        logger.info("POST request to /user")
        return {"message": "User created", "id": 3}
```

**Tip:** Edit the generated file to customize the response data!

## Step 7: Your Application is Ready!

The `reroute init` command already created a clean `main.py` for you:

```python
from fastapi import FastAPI
from reroute.adapters import FastAPIAdapter
from config import AppConfig
from pathlib import Path

# Load configuration from environment
AppConfig.load_from_env()

app = FastAPI(title="My REROUTE App")

# Clean adapter pattern
adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir=Path(__file__).parent / "app",
    config=AppConfig
)

adapter.register_routes()

if __name__ == "__main__":
    adapter.run_server()
```

**No manual setup required!** Everything is configured with secure defaults and ready to run.

## Step 8: Run the Application

=== "UV"

    ```bash
    uv run main.py
    ```

=== "pip"

    ```bash
    python main.py
    ```

Visit:

- API Docs: [http://localhost:7376/docs](http://localhost:7376/docs)
- Root endpoint: [http://localhost:7376/](http://localhost:7376/)
- Health check: [http://localhost:7376/health](http://localhost:7376/health)
- Users endpoint: [http://localhost:7376/user](http://localhost:7376/user)

## Step 9: Test Your API

```bash
# Get users
curl http://localhost:7376/user

# Create user
curl -X POST http://localhost:7376/user

# Health check
curl http://localhost:7376/health

# Root endpoint
curl http://localhost:7376/
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
