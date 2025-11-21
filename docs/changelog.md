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

## [Unreleased]

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

### Fixed
- Documentation now accurately reflects actual REROUTE features
- Removed fake features and methods from examples
- Corrected import statements across all documentation pages

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
