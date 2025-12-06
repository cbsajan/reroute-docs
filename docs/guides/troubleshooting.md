# Troubleshooting

Quick fixes for common REROUTE issues.

---

## Routes Not Appearing in Swagger UI

**Fix:** Swagger UI is at `/docs/swagger/` (not `/swagger/`)

```python
# config.py
class AppConfig(Config):
    class OpenAPI:
        ENABLE = True
        DOCS_PATH = "/docs"  # Swagger UI at /docs/swagger/
```

---

## IndentationError in config.py

**Fix:** Ensure each config line is separate:

```python
# Correct format
REDOC_PATH = None
JSON_PATH = "/openapi.json"
```

---

## ModuleNotFoundError: spectree

**Fix:**
```bash
pip install spectree
```

---

## CORS Errors in Browser

**Fix:**
```python
# config.py
class AppConfig(Config):
    ENABLE_CORS = True
    CORS_ALLOW_ORIGINS = ["http://localhost:3000"]
    CORS_ALLOW_CREDENTIALS = True
```

```bash
pip install flask-cors
```

---

## Routes Return 404

**Fix:** Check file structure:

```
app/
  routes/
    hello/
      page.py    # Must contain RouteBase class
```

```python
# page.py
from reroute import RouteBase

class HelloRoutes(RouteBase):
    def get(self):
        return {"message": "Hello"}
```

---

## Port Already in Use

**Fix:** Change port or kill process:

```python
# config.py
PORT = 8080
```

Or kill the process:
```bash
# Windows
netstat -ano | findstr :7376
taskkill /PID <pid> /F

# Linux/Mac
lsof -ti:7376 | xargs kill -9
```

---

## Import Errors

**Fix:** Use correct import paths:

```python
# Decorators
from reroute.decorators import rate_limit, cache, requires

# Parameters
from reroute.params import Query, Body, Header, Path

# Core
from reroute import RouteBase, Config
```

---

## Need Help?

- [Examples](../examples/index.md)
- [API Reference](../api/index.md)
- [GitHub Issues](https://github.com/cbsajan/reroute/issues)
