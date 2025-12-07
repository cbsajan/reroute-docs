# Migration Guide

This guide covers important migration steps and fixes for common issues.

## Common Issues and Solutions

### 1. Config Key Whitelist Error

**Problem**: `Config key not in whitelist: REROUTE_SECRET_KEY`

**Solution**: This error occurs when using an old version of REROUTE. Update to the latest version:

```bash
pip install --upgrade reroute
```

**Note**: REROUTE no longer uses whitelists - ANY `REROUTE_` prefixed environment variable is supported.

### 2. Adapter Architecture

**Problem**: Too much code in `main.py` makes it hard to maintain.

**Solution**: Use the adapter pattern with clean separation:

```python
# main.py
from reroute.adapters import FastAPIAdapter
from config import AppConfig

# Clean adapter initialization
app = FastAPI()
adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir="app",
    config=AppConfig
)

adapter.register_routes()
adapter.run_server()
```

### 3. Environment Loading

**Problem**: Configuration not loading from .env file.

**Solution**: Use explicit environment loading:

```python
# config.py
from reroute import Config

class AppConfig(Config):
    PORT = 8000
    DEBUG = False
    SECRET_KEY = None  # Will be loaded from .env

# Explicitly load from environment
AppConfig.load_from_env()
```

### 4. .egg-info Directory Creation

**Problem**: Unwanted `.egg-info` directory creation.

**Solution**: Control .egg-info creation with environment variable:

```env
# .env
REROUTE_CREATE_EGG_INFO=False
```

Or in code:

```python
import os
os.environ["REROUTE_CREATE_EGG_INFO"] = "False"
```

### 5. UV Package Manager Setup

**Problem**: Missing virtual environment setup for UV.

**Solution**: Use the complete UV workflow:

```bash
# Create virtual environment
uv venv

# Activate (Windows)
.venv\Scripts\activate

# Activate (Linux/Mac)
source .venv/bin/activate

# Create project
reroute init myapi

# Install dependencies
cd myapi
uv sync

# Run application
uv run main.py
```

### 6. User-Defined Configuration Variables

**Problem**: Need to add custom configuration variables.

**Solution**: Add any variables to config class - no whitelist restrictions:

```python
# config.py
from reroute import Config

class AppConfig(Config):
    # Standard REROUTE variables
    PORT = 8000
    DEBUG = False

    # Custom variables (any prefix allowed)
    MAX_FILE_SIZE: int = 10485760  # 10MB
    CACHE_TTL: int = 300
    API_RATE_LIMIT: str = "100/hour"

    # Empty/None values supported
    TESTING_VAR: str = ""  # Empty string
    OPTIONAL_SETTING: str = None  # None value
```

**Environment Variable Override**:
```env
# .env file overrides config.py values
REROUTE_PORT=9000
REROUTE_DEBUG=False
MAX_FILE_SIZE=20971520  # 20MB override
CACHE_TTL=600
TESTING_VAR=test_value  # Override empty string
```

### 7. Empty Data Support

**Problem**: Configuration variables need to support empty values.

**Solution**: REROUTE supports empty strings and null values:

```python
# config.py
class AppConfig(Config):
    EMPTY_STRING: str = ""      # Supported
    NONE_VALUE: str = None      # Supported
    OPTIONAL_VAR: str = ""      # Default empty
```

```env
# .env
EMPTY_VAR=""                   # Empty string
NULL_VAR=null                  # None value
UNDEFINED_VAR=                 # Empty string
```

## Migration Steps

### From v0.1.x to v0.2.x

1. **Update Dependencies**:
```bash
pip install --upgrade reroute
```

2. **Update Config Loading**:
```python
# Add this to your config.py
AppConfig.load_from_env()
```

3. **Update .env File**:
```env
# Add security settings
REROUTE_DEBUG=False
REROUTE_HOST=127.0.0.1
REROUTE_SECRET_KEY=your-secure-secret-key

# Control .egg-info creation
REROUTE_CREATE_EGG_INFO=False
```

4. **Update Main Application**:
```python
# Use clean adapter pattern
from reroute.adapters import FastAPIAdapter  # or FlaskAdapter

adapter = FastAPIAdapter(
    fastapi_app=app,
    app_dir="app",
    config=AppConfig
)
```

5. **Update Setup Commands**:
```bash
# Add virtual environment creation
uv venv

# Use uv sync instead of pip install
uv sync

# Use uv run
uv run main.py
```

## Environment Variable Priority

REROUTE follows this priority order:

1. **Environment Variables** (highest)
2. **.env File**
3. **Config Class** (lowest)

```python
# config.py
class AppConfig(Config):
    PORT = 8000  # Lowest priority
```

```env
# .env
PORT = 9000  # Medium priority
```

```bash
# Environment variable
export REROUTE_PORT=7000  # Highest priority
```

Result: `PORT = 7000`

## Type Detection

REROUTE automatically detects types from environment variables:

```env
REROUTE_PORT=8000                    # -> int
REROUTE_DEBUG=true                   # -> bool
REROUTE_CORS_ORIGINS=["*"]           # -> list
REROUTE_TIMEOUT=30.5                 # -> float
REROUTE_NAME="MyApp"                 # -> str
REROUTE_EMPTY=""                     # -> str (empty)
REROUTE_NULL=null                    # -> None
```

## Security Migration

### Enable Secure Defaults

```env
# Production settings
REROUTE_DEBUG=False
REROUTE_HOST=127.0.0.1
REROUTE_CORS_ORIGINS=https://yourdomain.com
REROUTE_MAX_REQUEST_SIZE=16777216    # 16MB
```

### Generate Secure Keys

```python
# Generate secret key in Python
import secrets
print(secrets.token_urlsafe(32))

# Or let REROUTE auto-generate
SECRET_KEY = None  # REROUTE will generate one
```

## Troubleshooting

### Common Error Messages

1. **"Config key not in whitelist"**
   - Update REROUTE version
   - Old versions had whitelist restrictions

2. **"ModuleNotFoundError: reroute.adapters"**
   - Update to latest version
   - Ensure proper installation: `pip install reroute`

3. **Environment variables not loading**
   - Add `AppConfig.load_from_env()` to config.py
   - Check .env file is in project root

4. **.egg-info directory created**
   - Set `REROUTE_CREATE_EGG_INFO=False`
   - Add to environment variables or .env file

### Debug Mode

Enable debug mode to troubleshoot:

```python
# config.py
class AppConfig(Config):
    DEBUG = True  # Enable for development

# or
AppConfig.load_from_env()
```

```env
REROUTE_DEBUG=True  # Enable via environment
```

### Getting Help

- **Documentation**: [https://cbsajan.github.io/reroute-docs](https://cbsajan.github.io/reroute-docs)
- **Issues**: [GitHub Issues](https://github.com/cbsajan/reroute/issues)
- **Changelog**: [CHANGELOG.md](https://github.com/cbsajan/reroute/blob/main/CHANGELOG.md)