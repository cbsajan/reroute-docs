# Changelog

All notable changes to REROUTE are documented here.

## [0.1.2] - 20-11-2025

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
