# Authentication Example

Implement authentication with REROUTE.

## Secure Route

```python
from reroute import RouteBase, requires

class SecureRoutes(RouteBase):
    tag = "Secure Endpoints"

    def before_request(self):
        """Check authentication"""
        auth_header = self.get_header("Authorization")
        if not auth_header:
            raise Unauthorized("Missing authentication")

        token = auth_header.replace("Bearer ", "")
        if not self.verify_token(token):
            raise Unauthorized("Invalid token")

        return None

    def verify_token(self, token):
        """Verify JWT token"""
        # Implementation here
        return token == "valid_token"

    @requires("admin")
    def delete(self):
        """Admin-only endpoint"""
        return {"deleted": True}

    def get(self):
        """Authenticated endpoint"""
        return {"data": "secure"}
```

## JWT Example

```python
import jwt
from datetime import datetime, timedelta

class AuthRoutes(RouteBase):
    def post(self):
        """Login endpoint"""
        # Verify credentials
        token = jwt.encode({
            "user_id": 1,
            "exp": datetime.utcnow() + timedelta(hours=24)
        }, "secret_key", algorithm="HS256")

        return {"token": token}
```

## Usage

```bash
# Login
curl -X POST http://localhost:8000/auth

# Access secure endpoint
curl -H "Authorization: Bearer <token>" http://localhost:8000/secure
```
