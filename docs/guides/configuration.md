# Configuration

Customize REROUTE's behavior with configuration options.

## Basic Configuration

```python
from reroute import Config

class MyConfig(Config):
    PORT = 8000
    HOST = "0.0.0.0"
    DEBUG = True
```

## Available Options

### Server Settings
- `PORT` - Server port (default: 8000)
- `HOST` - Server host (default: "127.0.0.1")
- `DEBUG` - Debug mode (default: False)
- `AUTO_RELOAD` - Auto-reload on changes (default: True)

### Routing
- `API_BASE_PATH` - Base path for routes (default: "/")
- `ROUTES_DIR` - Routes directory (default: "app/routes")

### CORS
- `ENABLE_CORS` - Enable CORS (default: False)
- `CORS_ALLOW_ORIGINS` - Allowed origins
- `CORS_ALLOW_METHODS` - Allowed methods
- `CORS_ALLOW_HEADERS` - Allowed headers

### Logging
- `VERBOSE_LOGGING` - Verbose logs (default: False)
- `LOG_LEVEL` - Log level (default: "INFO")

## Usage

```python
from reroute.adapters import FastAPIAdapter

adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir=path,
    config=MyConfig
)
```
