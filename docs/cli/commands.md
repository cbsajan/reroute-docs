# CLI Commands

Complete reference for REROUTE CLI commands.

## init

Create a new REROUTE project.

```bash
reroute init [project-name] [OPTIONS]
```

### Options
- `--framework` - Framework to use (fastapi, flask)
- `--config` - Configuration type (dev, prod)
- `--host` - Server host (default: 0.0.0.0)
- `--port` - Server port (default: 7376)
- `--description` - Project description
- `--database`, `-db` - Database type (Coming in v0.2.0)

### Examples
```bash
# Interactive mode (prompts for all options)
reroute init

# Basic project with FastAPI
reroute init my-api --framework fastapi

# With specific framework and config
reroute init my-api --framework flask --config prod
```

### Database Setup (Coming in v0.2.0)

!!! info "Preview Feature"
    The `--database` flag is implemented but will be enabled in v0.2.0. Running it now shows a preview message.

When enabled, you'll be able to use:
```bash
reroute init my-api --framework fastapi --database postgresql
reroute init my-api -db sqlite
```

This will generate:
- `app/database.py` - Database connection configuration
- `app/db_models/user.py` - Example User model
- `.env.example` - Environment template with database URL

## generate / create

REROUTE provides two command groups for code generation:
- `reroute generate <type>` - Original command
- `reroute create <type>` - Alias (more intuitive)

Both work identically. Use whichever you prefer.

## generate route / create route

Generate a new route file.

```bash
reroute generate route [OPTIONS]
reroute create route [OPTIONS]
```

### Options
- `--path` - Route path (e.g., /users or /api/posts)
- `--name` - Route name (e.g., Users or Posts)
- `--methods` - HTTP methods (comma-separated, default: GET,POST,PUT,DELETE)
- `--http-test` - Generate HTTP test file
- `--dry-run` - Preview changes without creating files

### Examples
```bash
# Interactive mode
reroute create route

# With options
reroute create route --path /users --name Users

# Nested route
reroute create route --path /api/v1/users --name Users

# With specific methods
reroute create route --path /users --name Users --methods GET,POST

# With HTTP test file
reroute create route --path /users --name Users --http-test

# Preview changes without creating files
reroute create route --path /users --dry-run
```

## generate crud / create crud

Generate a full CRUD route with Create, Read, Update, Delete operations.

```bash
reroute generate crud [OPTIONS]
reroute create crud [OPTIONS]
```

### Options
- `--path` - Route path (e.g., /users or /api/posts)
- `--name` - Resource name (singular, e.g., User or Post)
- `--http-test` - Generate HTTP test file
- `--dry-run` - Preview changes without creating files
- `--auto-migrate` - Automatically create and apply database migration

### Examples
```bash
# Interactive mode
reroute create crud

# With options
reroute create crud --path /users --name User

# With HTTP test file
reroute create crud --path /users --name User --http-test

# Preview changes without creating files
reroute create crud --path /products --name Product --dry-run

# Create CRUD and auto-run migrations
reroute create crud --path /orders --name Order --auto-migrate
```

### Auto-Migrate Feature

The `--auto-migrate` flag automatically:

1. Creates a new Alembic migration with message "Add {resource} CRUD"
2. Applies the migration to your database

**Requirements:**
- Alembic must be installed (`pip install alembic`)
- Migrations must be initialized (`reroute db init`)
- Database connection must be configured

```bash
# Full workflow with auto-migrate
reroute create crud --path /products --name Product --auto-migrate --http-test
```

### Generated CRUD Operations

The CRUD command generates a route with these operations:

| Method | Endpoint | Operation |
|--------|----------|-----------|
| GET | `/users` | List all users |
| POST | `/users` | Create new user |
| GET | `/users/:id` | Get user by ID |
| PUT | `/users/:id` | Update user |
| DELETE | `/users/:id` | Delete user |

## generate model / create model

Generate Pydantic models for data validation.

```bash
reroute generate model [OPTIONS]
reroute create model [OPTIONS]
```

### Options
- `--name` - Model name (singular, e.g., User or Post)

### Examples
```bash
# Interactive mode
reroute create model

# With name
reroute create model --name User
reroute generate model --name Post
```

## create dbmodel (Coming in v0.2.0)

!!! info "Preview Feature"
    This command is implemented but hidden until v0.2.0 release. Running it shows a preview message.

Generate SQLAlchemy database models that inherit from `reroute.db.models.Model`.

```bash
reroute create dbmodel [OPTIONS]
```

### Options
- `--name` - Model name (singular, e.g., User or Product)

### Examples
```bash
# Will show preview message until v0.2.0
reroute create dbmodel --name User
reroute create dbmodel --name Product
```

### What It Will Generate

When enabled in v0.2.0, this command creates database models in `app/db_models/`:

```python
# app/db_models/user.py
from sqlalchemy import Column, String, Integer, Boolean
from reroute.db.models import Model

class User(Model):
    """User database model"""
    __tablename__ = 'users'

    # Inherited from Model: id, created_at, updated_at
    name = Column(String(100), nullable=False)
    email = Column(String(255), unique=True, index=True)
```

### Difference from `create model`

| Command | Purpose | Output |
|---------|---------|--------|
| `create model` | Pydantic validation schemas | `app/models/user.py` (UserBase, UserCreate, etc.) |
| `create dbmodel` | SQLAlchemy ORM models | `app/db_models/user.py` (database table definition) |

### Generated Model Schemas

Creates 5 Pydantic model schemas in `app/models/<name>.py`:

| Schema | Purpose | Usage |
|--------|---------|-------|
| `ModelBase` | Base schema with common fields | Shared fields |
| `ModelCreate` | For POST requests | Creating resources |
| `ModelUpdate` | For PUT/PATCH requests | Updating (all fields optional) |
| `ModelInDB` | Database representation | With id, timestamps |
| `ModelResponse` | API response schema | GET requests |

### Example Output

```bash
$ reroute create model --name User

[OK] Model created: D:\project\app\models\user.py
     Model: User
     Schemas: UserBase, UserCreate, UserUpdate, UserInDB, UserResponse

[NOTE] Generated with default fields: name, description, is_active
[TIP] Customize the fields in the generated file to match your requirements.

[TIP] Import with: from app.models.user import UserCreate, UserResponse
```

### Using Generated Models

```python
from app.models.user import UserCreate, UserResponse
from reroute import RouteBase
from reroute.params import Body

class UsersRoutes(RouteBase):
    def post(self, user: UserCreate = Body(...)):
        # Automatic validation with Pydantic
        return {"user": user.model_dump()}
```

## create auth (Coming in v0.2.0)

!!! info "Preview Feature"
    This command is implemented but hidden until v0.2.0 release. Running it shows a preview message.

Generate JWT authentication scaffolding with complete login, register, token refresh, and profile routes.

```bash
reroute create auth [OPTIONS]
```

### Options
- `--method`, `-m` - Authentication method (currently: `jwt`)

### Examples
```bash
# Will show preview message until v0.2.0
reroute create auth --method jwt
reroute create auth -m jwt
```

### What It Will Generate

When enabled in v0.2.0, this command creates a complete auth system:

**Auth Module** (`app/auth/`):

| File | Purpose |
|------|---------|
| `__init__.py` | Exports all auth functions |
| `jwt.py` | Token creation and verification |
| `password.py` | bcrypt password hashing |

**Auth Models** (`app/models/`):

| Schema | Purpose |
|--------|---------|
| `UserRegister` | Registration request |
| `UserLogin` | Login request |
| `TokenResponse` | JWT token response |
| `RefreshTokenRequest` | Token refresh request |
| `UserResponse` | User profile response |

**Auth Routes** (`app/routes/auth/`):

| Route | Method | Purpose |
|-------|--------|---------|
| `/auth/login` | POST | Authenticate user, return tokens |
| `/auth/register` | POST | Create new user account |
| `/auth/refresh` | POST | Refresh access token |
| `/auth/me` | GET | Get current user profile (protected) |

**Config Update**:

Adds JWT configuration to `config.py`:

```python
class AppConfig(Config):
    class JWT:
        SECRET = "generated-secret-here"
        ALGORITHM = "HS256"
        ACCESS_TOKEN_EXPIRE_MINUTES = 30
        REFRESH_TOKEN_EXPIRE_DAYS = 7
```

### Dependencies

```bash
pip install pyjwt bcrypt
```

### Structure After Generation

```
app/
  auth/
    __init__.py
    jwt.py
    password.py
  models/
    auth.py
  routes/
    auth/
      login/
        page.py       # POST /auth/login
      register/
        page.py       # POST /auth/register
      refresh/
        page.py       # POST /auth/refresh
      me/
        page.py       # GET /auth/me
```

### Customization

Each generated route contains `TODO(human)` comments indicating where to implement your database logic:

```python
# TODO(human): Implement user lookup from database
# Example:
# user = self.db.query(User).filter(User.email == credentials.email).first()
```

## db

Database management commands for migrations and versioning.

```bash
reroute db [COMMAND]
```

### Available Commands

| Command | Description |
|---------|-------------|
| `init` | Initialize database migrations |
| `migrate` | Create a new migration |
| `upgrade` | Apply all pending migrations |
| `downgrade` | Rollback migrations |
| `current` | Show current migration version |
| `history` | Show migration history |

### Examples

```bash
# Initialize database
reroute db init

# Create a migration
reroute db migrate "Add users table"

# Apply migrations
reroute db upgrade

# Check current version
reroute db current

# View migration history
reroute db history

# Rollback last migration
reroute db downgrade
```

## version

Show REROUTE version.

```bash
reroute --version
```

## help

Show help for commands.

```bash
reroute --help
reroute <command> --help
```

---

## CLI Features

### Progress Indicators

All CLI commands now show progress indicators during execution:

```
Creating project: my-api

  [ OK ] Creating project structure
  [ OK ] Creating config.py
  [ OK ] Creating logger.py
  [ OK ] Generating FASTAPI application
  [ OK ] Creating example route
  [ OK ] Creating .env.example
  [ OK ] Creating requirements.txt
  [ OK ] Creating pyproject.toml

==================================================
[SUCCESS] Project created successfully!
==================================================
```

### Helpful Error Messages

CLI errors include actionable suggestions to help you resolve issues quickly:

```
[ERROR] Migrations not initialized

[TIP] Run 'reroute db init' first to initialize migrations.
```

Errors provide clear guidance on what went wrong and how to fix it.

### Destructive Action Warnings

Commands that can cause data loss require confirmation:

```bash
$ reroute db downgrade --steps 2

==================================================
Rolling Back Database Migrations
==================================================

[WARNING] This will rollback 2 migration(s).
          This action may result in data loss!

Are you sure you want to proceed? [y/N]:
```
