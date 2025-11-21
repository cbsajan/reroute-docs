# Rate Limiting Example

Protect your API with rate limiting.

## Basic Rate Limiting

```python
from reroute import RouteBase
from reroute.decorators import rate_limit

class APIRoutes(RouteBase):
    @rate_limit("5/min")
    def post(self):
        """Max 5 requests per minute"""
        return {"created": True}

    @rate_limit("100/hour")
    def get(self):
        """Max 100 requests per hour"""
        return {"data": "..."}
```

## Different Limits per Method

```python
class UserRoutes(RouteBase):
    @rate_limit("100/min")
    def get(self):
        """Generous limit for reads"""
        return {"users": []}

    @rate_limit("10/min")
    def post(self):
        """Stricter limit for writes"""
        return {"created": True}

    @rate_limit("3/min")
    def delete(self):
        """Very strict for deletions"""
        return {"deleted": True}
```

## Custom Rate Limit Response

```python
class CustomRateLimit(RouteBase):
    def on_error(self, error):
        if isinstance(error, RateLimitExceeded):
            return {
                "error": "Rate limit exceeded",
                "retry_after": error.retry_after
            }
        return {"error": str(error)}
```

## Testing

```bash
# Hit endpoint multiple times
for i in {1..10}; do
    curl http://localhost:8000/api
done
```
