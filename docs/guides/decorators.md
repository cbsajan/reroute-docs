# Decorators

Use decorators to add functionality to your routes.

## Rate Limiting

```python
from reroute import RouteBase
from reroute.decorators import rate_limit

class MyRoutes(RouteBase):
    @rate_limit("5/min")
    def post(self):
        return {"created": True}
```

## Caching

```python
from reroute.decorators import cache

@cache(duration=60)
def get(self):
    return {"data": "cached"}
```

## Authentication

!!! warning "Not Yet Implemented"
    The `@requires` decorator is planned but not yet available.

```python
from reroute.decorators import requires

@requires("admin")
def delete(self):
    return {"deleted": True}
```

## Validation

!!! warning "Not Yet Implemented"
    The `@validate` decorator is planned but not yet available. Use Pydantic models for validation.

```python
from reroute.decorators import validate

@validate(schema)
def post(self):
    return {"validated": True}
```

## Combining Decorators

```python
@rate_limit("10/min")
@cache(duration=30)
@requires("user")
def get(self):
    return {"data": "..."}
```

## Learn More

- [API Reference](../api/decorators.md)
