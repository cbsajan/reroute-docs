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

REROUTE uses Pydantic models for request validation:

```python
from reroute import RouteBase
from reroute.params import Body
from pydantic import BaseModel

class UserCreate(BaseModel):
    name: str
    age: int

class MyRoutes(RouteBase):
    def post(self, user: UserCreate = Body()):
        """Automatically validates request body against UserCreate schema"""
        return {"validated": True, "user": user.dict()}
```

## Combining Decorators

Stack multiple decorators for powerful route protection:

```python
from reroute import RouteBase
from reroute.decorators import rate_limit, cache
from reroute.params import Body
from pydantic import BaseModel

class UserCreate(BaseModel):
    email: str
    name: str

class MyRoutes(RouteBase):
    @rate_limit("10/min")
    @cache(duration=30)
    def get(self):
        """Rate-limited and cached endpoint"""
        return {"data": "..."}

    @rate_limit("5/min")
    def post(self, user: UserCreate = Body()):
        """Rate-limited endpoint with Pydantic validation"""
        return {"created": True, "user": user.dict()}
```

Decorators are applied from bottom to top (innermost first).

## Learn More

- [API Reference](../api/decorators.md)
