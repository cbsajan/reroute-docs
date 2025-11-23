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

## validate

Validate request data against a schema.

```python
from reroute.decorators import validate

@validate(schema={"name": str, "age": int})
def post(self):
    return {"validated": True}
```

**Parameters:**
- `schema` (Dict[str, type], optional): Schema defining expected field types
- `validator_func` (Callable, optional): Custom validation function

**Example with custom validator:**
```python
def validate_user_data(data):
    if data.get("age", 0) < 18:
        raise ValueError("User must be 18 or older")
    return True

@validate(validator_func=validate_user_data)
def post(self):
    return {"validated": True}
```

**Note:** For complex validation, consider using Pydantic models with `Body()` parameter instead.

## timeout

Set request timeout to prevent long-running requests.

```python
from reroute.decorators import timeout

@timeout(5)
def get(self):
    # This request will timeout after 5 seconds
    return {"data": "..."}
```

**Parameters:**
- `seconds` (int): Timeout in seconds

**Note:** If request exceeds timeout, it will be terminated and return a timeout error.

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
