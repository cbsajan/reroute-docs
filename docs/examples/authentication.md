# Authentication Example

Implement JWT authentication with REROUTE.

## Quick Start with Auth Scaffolding

!!! tip "Recommended Approach"
    Use the CLI to generate a complete auth system in seconds.

```bash
# Generate JWT authentication scaffolding
reroute create auth --method jwt
```

This generates:

| File | Purpose |
|------|---------|
| `app/auth/jwt.py` | Token creation & verification |
| `app/auth/password.py` | bcrypt password hashing |
| `app/models/auth.py` | Pydantic schemas |
| `app/routes/auth/login/page.py` | POST /auth/login |
| `app/routes/auth/register/page.py` | POST /auth/register |
| `app/routes/auth/refresh/page.py` | POST /auth/refresh |
| `app/routes/auth/me/page.py` | GET /auth/me (protected) |

### Install Dependencies

```bash
pip install pyjwt bcrypt
```

### Configure JWT Secret

Update `config.py` with a secure secret:

```python
class AppConfig(Config):
    class JWT:
        # Generate with: python -c "import secrets; print(secrets.token_hex(32))"
        SECRET = "your-secure-secret-here"
        ALGORITHM = "HS256"
        ACCESS_TOKEN_EXPIRE_MINUTES = 30
        REFRESH_TOKEN_EXPIRE_DAYS = 7
```

---

## Generated Auth Routes

### Login (`POST /auth/login`)

```python
# app/routes/auth/login/page.py
from reroute import RouteBase
from reroute.decorators import rate_limit
from reroute.params import Body

from app.auth import create_access_token, create_refresh_token, verify_password
from app.models.auth import UserLogin

class LoginRoutes(RouteBase):
    tag = "Auth"

    @rate_limit("5/min")
    def post(self, credentials: UserLogin = Body(...)):
        """Authenticate user and return JWT tokens."""
        # TODO: Implement user lookup from your database
        user = get_user_by_email(credentials.email)

        if not user or not verify_password(credentials.password, user.password_hash):
            return {"error": "Invalid email or password"}, 401

        token_data = {"sub": str(user.id), "email": user.email}

        return {
            "access_token": create_access_token(token_data),
            "refresh_token": create_refresh_token(token_data),
            "token_type": "bearer",
            "expires_in": 1800
        }
```

### Register (`POST /auth/register`)

```python
# app/routes/auth/register/page.py
from reroute import RouteBase
from reroute.decorators import rate_limit
from reroute.params import Body

from app.auth import hash_password
from app.models.auth import UserRegister

class RegisterRoutes(RouteBase):
    tag = "Auth"

    @rate_limit("3/min")
    def post(self, user_data: UserRegister = Body(...)):
        """Register a new user account."""
        # TODO: Check if email already exists
        # TODO: Save user to database

        password_hash = hash_password(user_data.password)

        # Create user in database
        new_user = create_user(
            email=user_data.email,
            password_hash=password_hash,
            name=user_data.name
        )

        return {
            "message": "User registered successfully",
            "user": {"id": new_user.id, "email": new_user.email}
        }
```

### Protected Route (`GET /auth/me`)

```python
# app/routes/auth/me/page.py
from reroute import RouteBase
from reroute.decorators import requires

from app.auth import verify_token

def check_auth(self):
    """Check if request has valid authorization token."""
    auth_header = self.get_header("Authorization")
    if not auth_header or not auth_header.startswith("Bearer "):
        return False

    token = auth_header.split(" ")[1]
    payload = verify_token(token, token_type="access")
    if payload:
        self.current_user = payload
        return True
    return False

class MeRoutes(RouteBase):
    tag = "Auth"

    @requires(check_func=check_auth)
    def get(self):
        """Get current authenticated user profile."""
        return {
            "id": self.current_user.get("sub"),
            "email": self.current_user.get("email")
        }
```

---

## Auth Utilities

### JWT Token Functions

```python
# app/auth/jwt.py
from datetime import datetime, timedelta, timezone
import jwt
from config import AppConfig

def create_access_token(data: dict) -> str:
    """Create a JWT access token."""
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + timedelta(
        minutes=AppConfig.JWT.ACCESS_TOKEN_EXPIRE_MINUTES
    )
    to_encode.update({"exp": expire, "type": "access"})
    return jwt.encode(to_encode, AppConfig.JWT.SECRET, algorithm=AppConfig.JWT.ALGORITHM)

def verify_token(token: str, token_type: str = "access") -> dict | None:
    """Verify and decode a JWT token."""
    try:
        payload = jwt.decode(
            token,
            AppConfig.JWT.SECRET,
            algorithms=[AppConfig.JWT.ALGORITHM]
        )
        if payload.get("type") != token_type:
            return None
        return payload
    except jwt.ExpiredSignatureError:
        return None
    except jwt.InvalidTokenError:
        return None
```

### Password Hashing

```python
# app/auth/password.py
import bcrypt

def hash_password(password: str) -> str:
    """Hash a password using bcrypt."""
    salt = bcrypt.gensalt()
    return bcrypt.hashpw(password.encode('utf-8'), salt).decode('utf-8')

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """Verify a password against its hash."""
    return bcrypt.checkpw(
        plain_password.encode('utf-8'),
        hashed_password.encode('utf-8')
    )
```

---

## Pydantic Models

```python
# app/models/auth.py
from pydantic import BaseModel, EmailStr, Field

class UserRegister(BaseModel):
    email: EmailStr
    password: str = Field(..., min_length=8)
    name: str | None = None

class UserLogin(BaseModel):
    email: EmailStr
    password: str

class TokenResponse(BaseModel):
    access_token: str
    refresh_token: str
    token_type: str = "bearer"
    expires_in: int = 1800

class RefreshTokenRequest(BaseModel):
    refresh_token: str
```

---

## API Usage

### Register a New User

```bash
curl -X POST http://localhost:7376/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "securepass123", "name": "John"}'
```

### Login

```bash
curl -X POST http://localhost:7376/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "securepass123"}'
```

Response:
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "token_type": "bearer",
  "expires_in": 1800
}
```

### Access Protected Endpoint

```bash
curl http://localhost:7376/auth/me \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
```

### Refresh Token

```bash
curl -X POST http://localhost:7376/auth/refresh \
  -H "Content-Type: application/json" \
  -d '{"refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."}'
```

---

## HTTP Test File

```http
### Register User
POST http://localhost:7376/auth/register
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "password123",
  "name": "Test User"
}

### Login
POST http://localhost:7376/auth/login
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "password123"
}

### Get Current User (Protected)
GET http://localhost:7376/auth/me
Authorization: Bearer {{access_token}}

### Refresh Token
POST http://localhost:7376/auth/refresh
Content-Type: application/json

{
  "refresh_token": "{{refresh_token}}"
}
```

---

## Security Best Practices

1. **Use Strong Secrets**: Generate with `python -c "import secrets; print(secrets.token_hex(32))"`
2. **Store in Environment**: Never commit secrets to version control
3. **Use HTTPS**: Always use HTTPS in production
4. **Short Token Expiry**: Keep access tokens short-lived (15-30 minutes)
5. **Rotate Secrets**: Rotate JWT secrets periodically
6. **Rate Limit**: Use `@rate_limit` on auth endpoints to prevent brute force
7. **Validate Input**: Use Pydantic models for input validation

---

## Database Integration

The generated auth routes include `TODO(human)` comments showing where to add your database logic:

```python
# TODO(human): Implement user lookup from database
# Example:
# user = self.db.query(User).filter(User.email == credentials.email).first()
```

See the [Database Guide](database.md) for setting up SQLAlchemy or other ORMs.
