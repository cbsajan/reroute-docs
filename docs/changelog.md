# Changelog

All notable changes to REROUTE are documented here.

## [Unreleased] - 2025-11-21

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
