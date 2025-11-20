# Caching Example

Improve performance with response caching.

## Basic Caching

```python
from reroute import RouteBase, cache

class ProductRoutes(RouteBase):
    @cache(duration=300)
    def get(self):
        """Cached for 5 minutes"""
        # Expensive operation
        products = self.fetch_from_database()
        return {"products": products}
```

## Different Cache Durations

```python
class DataRoutes(RouteBase):
    @cache(duration=60)
    def get_recent(self):
        """Cache for 1 minute"""
        return {"data": "recent"}

    @cache(duration=3600)
    def get_static(self):
        """Cache for 1 hour"""
        return {"data": "static"}
```

## Conditional Caching

```python
class SmartCache(RouteBase):
    @cache(duration=60)
    def get(self):
        """Only cache if query param says so"""
        if self.get_param("no_cache"):
            # Bypass cache
            return self.fresh_data()
        return {"data": "cached"}
```

## Cache Invalidation

```python
class UserRoutes(RouteBase):
    @cache(duration=300)
    def get(self):
        return {"users": self.users}

    def post(self):
        """Creating user invalidates cache"""
        self.users.append(new_user)
        self.invalidate_cache()  # Clear cache
        return {"created": True}
```

## Testing

```bash
# First request - slow (cache miss)
time curl http://localhost:8000/products

# Second request - fast (cache hit)
time curl http://localhost:8000/products
```
