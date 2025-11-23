# Decorators API

Built-in decorators for REROUTE routes.

## rate_limit

Limit request rate per IP or user.

```python
from reroute.decorators import rate_limit

@rate_limit("5/min")
def post(self):
    return {"created": True}
```

**Parameters:**
- `rate` (str): Rate limit (e.g., "5/min", "100/hour")

## cache

Cache responses for a duration.

```python
from reroute.decorators import cache

@cache(duration=60)
def get(self):
    return {"data": "cached"}
```

**Parameters:**
- `duration` (int): Cache duration in seconds

## Request Validation

REROUTE uses Pydantic models for request validation instead of decorators:

```python
from reroute import RouteBase
from reroute.params import Body
from pydantic import BaseModel, field_validator

class UserCreate(BaseModel):
    name: str
    age: int

    @field_validator('age')
    def validate_age(cls, v):
        if v < 18:
            raise ValueError('User must be 18 or older')
        return v

class MyRoutes(RouteBase):
    def post(self, user: UserCreate = Body()):
        """Request body is automatically validated against UserCreate schema"""
        return {"validated": True, "user": user.dict()}
```

**Advantages:**
- Automatic validation and error messages
- Type hints and IDE autocomplete
- OpenAPI schema generation
- Field validation with `@field_validator`

See [Database Examples](../examples/database.md) for more validation patterns.

## log_requests

Automatic request logging with customizable logger.

```python
from reroute.decorators import log_requests

@log_requests()
def get(self):
    return {"data": "..."}
```

**Example with custom logger:**
```python
def custom_logger(request, response, duration):
    print(f"{request.method} {request.path} - {response.status_code} ({duration}ms)")

@log_requests(logger_func=custom_logger)
def get(self):
    return {"data": "..."}
```

**Parameters:**
- `logger_func` (Callable, optional): Custom logging function that receives (request, response, duration)
