# Changelog

All notable changes to REROUTE are documented here.

## [0.1.4] - 2025-11-21

### Added

#### Flask Adapter - OpenAPI Support
- **Spectree Integration** - Automatic OpenAPI documentation with Swagger UI and Scalar UI
- **Dynamic HTTP Method Decorators** - `@adapter.get()`, `@adapter.post()`, `@adapter.put()`, `@adapter.patch()`, `@adapter.delete()`, `@adapter.head()`, `@adapter.options()`, `@adapter.route()`
- **Request/Response Validation** - Automatic validation using Pydantic models via Spectree
- **Documentation UIs**:
  - Swagger UI at `/swagger/`
  - Scalar UI at `/scalar/`
  - OpenAPI JSON at `/openapi.json`
  - ReDoc disabled (broken CDN URL)

#### FastAPI Adapter Enhancements
- **Auto-Reload Detection** - Automatically constructs import string (`main:app`) for uvicorn reload mode
- **Route Removal for Disabled Endpoints** - Properly removes routes when OpenAPI paths set to `None`
- **Explicit None Handling** - Ensures empty strings treated as `None` for OpenAPI paths

#### Configuration
- **OpenAPI Configuration Section**:
  - `OpenAPI.ENABLE` - Enable/disable documentation
  - `OpenAPI.DOCS_PATH` - Swagger UI endpoint (set to `None` to disable)
  - `OpenAPI.REDOC_PATH` - ReDoc endpoint (Flask: `None`, FastAPI: `"/redoc"`)
  - `OpenAPI.JSON_PATH` - OpenAPI JSON spec endpoint
  - `OpenAPI.TITLE` - API title (auto-generated if `None`)
  - `OpenAPI.VERSION` - API version
  - `OpenAPI.DESCRIPTION` - API description (auto-generated if `None`)

#### Parameter Override Support
- **Runtime Configuration** - `run_server()` kwargs override config values
- **Enhanced Documentation** - Comprehensive docstrings with available parameters

### Changed

#### Flask
- **Documentation URLs**:
  - Swagger UI: `/swagger/` (not `/docs`)
  - Scalar UI: `/scalar/` (new)
  - ReDoc: Disabled
- **Decorator Syntax**: Use `@adapter.get()` instead of `@adapter.app.get()`
- **Spectree Registration**: Happens automatically after `register_routes()`

#### FastAPI
- **Parameter Precedence**: Config defaults, kwargs override
- **Startup Messages**: Only show enabled documentation endpoints

#### Dependencies
- Added `spectree>=1.2.0` to Flask extras
- Added `colorama>=0.4.0` for Windows console colors

### Fixed
- Flask parameter override in `run_server()`
- FastAPI parameter override in `run_server()`
- FastAPI ReDoc disable (`REDOC_PATH = None` now removes route completely)
- Colorama initialization for Windows
- Uvicorn reload warning ("must pass application as import string")

### Migration Guide

#### Flask Projects
1. Install: `pip install spectree flask-cors`
2. Add OpenAPI config section
3. Update syntax: `@adapter.app.get()` → `@adapter.get()`
4. Update URLs: `/docs` → `/swagger/`

#### FastAPI Projects
1. Add OpenAPI config (if missing)
2. Optional: Set `REDOC_PATH = None` to disable ReDoc
3. Use parameter overrides: `adapter.run_server(port=8080)`

### Breaking Changes
- **Flask**: Must use `@adapter.get()` instead of `@adapter.app.get()`
- **Flask**: Documentation moved to `/swagger/` and `/scalar/`
- **Configuration**: Must add `OpenAPI` nested class

---

## [0.1.7] - 2025-11-28

### Security Enhancements

#### Column-Validated Database Queries
- **Enhanced**: `Model.get_all()` now includes built-in column validation for `order_by` parameter
- Uses SQLAlchemy's `inspect().mapper.column_attrs` for validation
- Provides helpful error messages listing valid column names
- **Test Coverage**: 4 test cases

#### Input-Validated CLI Commands
- **Enhanced**: All CLI commands now include comprehensive input validation
- `reroute db downgrade --steps` validates as positive integer (1-100)
- Clear error messages for invalid inputs
- **Test Coverage**: 5 test cases

#### Bounded Cache with LRU Eviction
- **Enhanced**: `@cache` decorator now includes automatic memory management
- Maximum cache size: 1000 entries
- LRU (Least Recently Used) eviction strategy
- Background cleanup every 60 seconds
- Full error logging for observability
- **Test Coverage**: 4 test cases

#### Fail-Safe Authorization
- **Enhanced**: `@requires` decorator implements fail-safe authorization pattern
- Access denied by default unless explicitly granted
- Proper HTTP status codes: 401 (auth), 403 (authz), 500 (config error)
- Integrated error logging
- **Breaking Change**: `@requires("role")` now requires explicit `check_func` parameter
- **Test Coverage**: 7 test cases

### Added

#### Security Logging (OWASP A09)
- **New**: `SecurityLogger` class for structured security event logging
- JSON-formatted output for SIEM integration
- Automatic sensitive data redaction (passwords, tokens, API keys)
- Security event types: AUTH_SUCCESS, AUTH_FAILURE, AUTHZ_FAILURE, RATE_LIMIT_EXCEEDED, etc.
- Integrated with `@rate_limit` and `@requires` decorators

#### Health Check Endpoint
- **New**: Built-in `/health` endpoint for monitoring and load balancers
- Configurable via `HEALTH_CHECK_ENABLED` and `HEALTH_CHECK_PATH`
- Returns service status, name, and version in JSON format
- Automatically registered with FastAPI and Flask adapters

#### Error Response Helper
- **New**: `error_response()` helper function in decorators module
- Consistent error response formatting across the application
- Supports custom error types and detailed messages

### Enhanced
- **Rate Limiter**: Added bounded storage with LRU eviction (max 10,000 keys)
- **Cache**: Thread-safe operations with double-checked locking
- **Decorators**: Added `__all__` exports for cleaner imports

### Test Suite
- Comprehensive security test suite (`tests/test_security_fixes.py`)
- 27 test cases covering security features
- Integration tests and platform-specific edge cases

### Documentation
- Added "Security Logging (OWASP A09)" section to security guide
- Updated security best practices with new feature examples
- Added Health Check configuration documentation

### Breaking Changes
- **@requires decorator**: Now requires explicit `check_func` parameter when roles are specified
  - Migration: Add `check_func` parameter with your authorization logic

---

## [0.1.9] - 2025-12-06

### Added

#### CLI Workflow Improvements
- **New Flag**: `--dry-run` for `create route` and `create crud` commands
  - Preview changes without creating files
  - Shows what files would be created and their contents
- **New Flag**: `--auto-migrate` for `create crud` command
  - Automatically creates and applies database migration after CRUD generation
  - Runs `alembic revision --autogenerate` followed by `alembic upgrade head`
  - Graceful error handling with helpful tips if migrations not set up

### Fixed

#### Security Bug Fixes
- **Fixed**: `me.py.j2` - IndexError on malformed Bearer token
  - Added bounds check before splitting Authorization header
  - Prevents crash on headers like "Bearer" (without token)
- **Fixed**: `password.py.j2` - bcrypt ValueError on invalid hash
  - Added try-except around `bcrypt.checkpw()`
  - Returns `False` instead of crashing on invalid hash format
- **Fixed**: `db_commands.py` - Missing `sys.exit(1)` after error handling
  - `current()` and `history()` commands now properly exit on error
- **Fixed**: `create_command.py` - Redundant path operation
  - Cleaned up path handling for cross-platform compatibility

---

## [0.1.8] - 2025-11-30

### Added

#### CLI Improvements
- **Progress Indicators**: All CLI commands now show real-time progress with `[ OK ]` / `[FAIL]` status
- **Actionable Suggestions**: Every error includes a `[TIP]` with specific commands to resolve the issue
- **Destructive Action Warnings**: Commands like `db downgrade` now require confirmation before executing
- **Global Exception Handling**: Consistent error formatting across all commands

#### Database Init Flag (Preview for v0.2.0)
- **New Flag**: `reroute init --database postgresql` (or `-db sqlite`)
- Shows preview message when used (feature flag disabled until v0.2.0)
- Will generate database.py, .env.example, and example User model when enabled

#### DBModel Command (Preview for v0.2.0)
- **New Command**: `reroute create dbmodel --name User` (hidden until v0.2.0)
- Generates SQLAlchemy models inheriting from `reroute.db.models.Model`
- Creates files in `app/db_models/` with id, created_at, updated_at inherited from base
- Shows preview message when run (feature flag disabled)
- **New Utility**: `to_pascal_case()` helper for model class names (without "Routes" suffix)

#### Auth Scaffolding Command (Preview for v0.2.0)
- **New Command**: `reroute create auth --method jwt` (hidden until v0.2.0)
- Generates complete JWT authentication system:
  - `app/auth/` module with jwt.py, password.py utilities
  - `app/models/auth.py` with Pydantic schemas
  - `app/routes/auth/` with login, register, refresh, me routes
  - JWT configuration in config.py with auto-generated secret
- Uses REROUTE's file-based routing pattern (each route has `page.py`)
- Includes `TODO(human)` comments for database integration
- Dependencies: `pyjwt`, `bcrypt`

#### Feature Flags System
- **New**: `FEATURE_FLAGS` dictionary in `reroute/__init__.py`
- Allows features to be implemented but hidden until target release
- Commands can be hidden with `hidden=True` in Click decorator

### Changed
- **CLI Entry Point**: `__main__.py` now uses `main()` function with global error handling
- **DB Commands**: All database commands updated with progress indicators and error codes
- **Init Command**: Now shows step-by-step progress during project creation

### Documentation
- **BREAKING**: Fixed incorrect decorator imports throughout documentation
  - Changed `from reroute import rate_limit, cache` to `from reroute.decorators import`
  - Affects: quickstart, API reference, guides, and all examples
- Updated quickstart guide to use CLI commands instead of manual file creation
  - Replaced `mkdir` commands with `reroute init`
  - Replaced manual code writing with `reroute create route`
  - Reduced setup time from 5 minutes to 2 minutes
- Added warnings for unimplemented decorators (`@requires`, `@validate`, `@timeout`)
- Created IMPLEMENTATION_STATUS.md tracking file for feature accuracy
- **Added**: Documentation for `create dbmodel` command (preview)
- **Added**: Documentation for `create auth` command (preview)
- **Added**: CLI Features section documenting progress indicators and error handling

### Fixed
- Documentation now accurately reflects actual REROUTE features
- Removed fake features and methods from examples
- Corrected import statements across all documentation pages

---

## [0.1.2] - 2025-11-20

### Added
- Lazy imports for optional framework dependencies
- Automatic update notifications in CLI (`reroute --version`)
- Clear error messages for missing framework dependencies

### Changed
- Optimized CI/CD pipeline (15 → 8 test jobs, 47% faster)
- Improved GitHub Actions workflow error handling

### Fixed
- Module import errors for optional dependencies
- Version consistency across configuration files
- GitHub Release 403 authorization errors

## [0.1.1] - 20-11-2025

### Added
- Version display with `--version` and `-V` flags
- Modular CLI architecture

### Fixed
- Duplicate banner display in CLI
- Unicode compatibility for Windows
- pyproject.toml license format

## [0.1.0] - 19-11-2025

### Added
- Initial release
- File-based routing
- FastAPI and Flask adapters
- Built-in decorators (`@rate_limit`, `@cache`, etc.)
- CLI for project scaffolding
- Configuration system
- Parameter injection
- Lifecycle hooks

---

[View full changelog on GitHub](https://github.com/cbsajan/reroute/blob/main/CHANGELOG.md)
