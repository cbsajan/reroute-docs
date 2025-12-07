# Configuration

Customize REROUTE's behavior with configuration options.

## Basic Configuration

```python
from reroute import Config

class MyConfig(Config):
    PORT = 8000
    HOST = "127.0.0.1"  # 🔒 SECURITY: Localhost only by default
    DEBUG = False  # 🔒 SECURITY: Disabled in production
```

## Environment Variables Support

REROUTE automatically supports **ANY** environment variable with the `REROUTE_` prefix. There's no whitelist - you can add any custom configuration variables you need.

### Priority Order
1. **Environment Variables** (highest priority)
2. **Config Class** (fallback)

### Auto Type Detection
REROUTE automatically detects and converts environment variable types:

```python
# Environment variables
REROUTE_PORT=8000                    # -> int
REROUTE_DEBUG=true                   # -> bool
REROUTE_CORS_ORIGINS=["http://localhost:3000"]  # -> list
REROUTE_TIMEOUT=30.5                 # -> float
REROUTE_APP_NAME="MyAPI"             # -> str
REROUTE_EMPTY_VALUE=""               # -> str (empty)
REROUTE_NULL_VALUE=null              # -> None
```

### User-Defined Variables
You can add any custom variables to your config:

```python
from reroute import Config

class AppConfig(Config):
    # Custom variables
    MAX_FILE_SIZE: int = 10485760  # 10MB
    CACHE_TTL: int = 300
    FEATURE_FLAGS: str = ""  # Empty string supported

    # Variables can be empty/None
    TESTING_VAR: str = ""  # Empty string allowed
    OPTIONAL_CONFIG: str = None  # None allowed
```

## Available Options

### Server Settings
- `PORT` - Server port (default: 7376)
- `HOST` - Server host (default: "127.0.0.1")
- `DEBUG` - Debug mode (default: False)
- `AUTO_RELOAD` - Auto-reload on changes (default: True)

### Security
- `SECRET_KEY` - JWT secret key (auto-generated if empty)
- `DATABASE_URL` - Database connection string
- `MAX_REQUEST_SIZE` - Maximum request size in bytes (default: 16MB)

### Routing
- `API_BASE_PATH` - Base path for routes (default: "/")
- `ROUTES_DIR` - Routes directory (default: "app/routes")

### CORS
- `ENABLE_CORS` - Enable CORS (default: False)
- `CORS_ALLOW_ORIGINS` - Allowed origins (default: localhost only)
- `CORS_ALLOW_METHODS` - Allowed methods
- `CORS_ALLOW_HEADERS` - Allowed headers

### OpenAPI Documentation
- `OpenAPI.ENABLE` - Enable/disable documentation (default: True)
- `OpenAPI.DOCS_PATH` - Swagger UI endpoint (default: "/docs")
- `OpenAPI.REDOC_PATH` - ReDoc endpoint (default: "/redoc")
- `OpenAPI.JSON_PATH` - OpenAPI JSON spec (default: "/openapi.json")
- `OpenAPI.TITLE` - API title
- `OpenAPI.VERSION` - API version (default: "1.0.0")
- `OpenAPI.DESCRIPTION` - API description

### Health Check
- `HEALTH_CHECK_ENABLED` - Enable health endpoint (default: True)
- `HEALTH_CHECK_PATH` - Health endpoint path (default: "/health")

### Logging
- `VERBOSE_LOGGING` - Verbose logs (default: False)
- `LOG_LEVEL` - Log level (default: "INFO")

### CLI Settings
- `REROUTE_CREATE_EGG_INFO` - Control .egg-info creation (default: False)

## Project Setup

### UV Package Manager (Default)
```bash
# Create virtual environment
uv venv

# Create project
reroute init myapi

# Install dependencies
cd myapi
uv sync

# Run application
uv run main.py
```

### Traditional Setup
```bash
# Create project
reroute init myapi --package-manager pip

# Install dependencies
cd myapi
pip install -r requirements.txt

# Run application
python main.py
```

## Usage

```python
from reroute.adapters import FastAPIAdapter

adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir=path,
    config=AppConfig
)
```

## Adapter Architecture

REROUTE uses a clean adapter architecture to support multiple web frameworks:

### FastAPI Adapter
```python
from reroute.adapters import FastAPIAdapter

# Direct adapter usage
adapter = FastAPIAdapter(
    fastapi_app=FastAPI(),
    app_dir="app",
    config=AppConfig
)

adapter.register_routes()
adapter.run_server()
```

### Flask Adapter
```python
from reroute.adapters import FlaskAdapter

# Direct adapter usage
adapter = FlaskAdapter(
    flask_app=Flask(__name__),
    app_dir="app",
    config=AppConfig
)

adapter.register_routes()
adapter.run_server()
```

## Environment Loading

### Explicit Environment Loading
The recommended approach is to explicitly load environment variables:

```python
# config.py
from reroute import Config

class AppConfig(Config):
    PORT = 8000
    DEBUG = False

# Load from environment variables (explicit)
AppConfig.load_from_env()
```

### .env File Support
Create a `.env` file in your project root:

```env
# Server Configuration
PORT=8000
DEBUG=False
HOST=127.0.0.1

# Security
SECRET_KEY=your-secret-key-here
DATABASE_URL=sqlite:///./app.db

# CORS
ENABLE_CORS=True
CORS_ALLOW_ORIGINS=http://localhost:3000

# Custom Variables
MAX_FILE_SIZE=10485760
CACHE_TTL=300
EMPTY_VALUE=
NULL_VALUE=null
```

## Secure Defaults

REROUTE implements secure-by-default configuration:

### Development vs Production
```python
# Development (config.py)
DEBUG = True          # Enabled for development
HOST = "127.0.0.1"    # Localhost only

# Production (environment variables)
REROUTE_DEBUG=False   # Disabled in production
REROUTE_HOST=127.0.0.1  # Localhost binding
REROUTE_CORS_ORIGINS=https://yourdomain.com  # Restrictive CORS
```

### Security Best Practices
- Debug mode disabled by default
- Localhost-only binding for development
- Restrictive CORS origins
- Auto-generated secure secret keys
- Request size limiting to prevent DoS attacks
