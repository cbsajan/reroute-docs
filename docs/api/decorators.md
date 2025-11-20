# Decorators API

Built-in decorators for REROUTE routes.

## rate_limit

Limit request rate per IP or user.

```python
from reroute import rate_limit

@rate_limit("5/min")
def post(self):
    return {"created": True}
```

**Parameters:**
- `rate` (str): Rate limit (e.g., "5/min", "100/hour")

## cache

Cache responses for a duration.

```python
from reroute import cache

@cache(duration=60)
def get(self):
    return {"data": "cached"}
```

**Parameters:**
- `duration` (int): Cache duration in seconds

## requires

Require specific permissions or roles.

```python
from reroute import requires

@requires("admin")
def delete(self):
    return {"deleted": True}
```

**Parameters:**
- `role` (str): Required role

## validate

Validate request data against a schema.

```python
from reroute import validate

@validate(schema)
def post(self):
    return {"validated": True}
```

**Parameters:**
- `schema`: Validation schema

## timeout

Set request timeout.

```python
from reroute import timeout

@timeout(5)
def get(self):
    return {"data": "..."}
```

**Parameters:**
- `seconds` (int): Timeout in seconds

## log_requests

Automatic request logging.

```python
from reroute import log_requests

@log_requests()
def get(self):
    return {"data": "..."}
```
