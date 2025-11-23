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
from reroute import RouteBase
from reroute.decorators import cache

class MyRoutes(RouteBase):
    @cache(duration=60)
    def get(self):
        return {"data": "cached"}
```

## Validation

```python
from reroute import RouteBase
from reroute.decorators import validate

class MyRoutes(RouteBase):
    @validate(schema={"name": str, "age": int})
    def post(self):
        return {"validated": True}
```

For complex validation, use Pydantic models with `Body()` parameter.

## Combining Decorators

Stack multiple decorators for powerful route protection:

```python
from reroute import RouteBase
from reroute.decorators import rate_limit, cache, validate

class MyRoutes(RouteBase):
    @rate_limit("10/min")
    @cache(duration=30)
    def get(self):
        """Rate-limited and cached endpoint"""
        return {"data": "..."}

    @rate_limit("5/min")
    @validate(schema={"email": str, "name": str})
    def post(self):
        """Rate-limited and validated endpoint"""
        return {"created": True}
```

Decorators are applied from bottom to top (validate → rate_limit).

## Learn More

- [API Reference](../api/decorators.md)
