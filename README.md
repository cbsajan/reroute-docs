<div align="center">
  <img src="docs/assets/logo-dark.svg" alt="REROUTE Logo" width="200">
  <h1>REROUTE</h1>
  <p><em>Modern file-based routing for Python backends</em></p>
</div>

# REROUTE Documentation

Official documentation for [REROUTE](https://github.com/cbsajan/reroute) - Modern file-based routing for Python backends.

## Live Documentation

Visit the live documentation at: **[cbsajan.github.io/reroute-docs](https://cbsajan.github.io/reroute-docs)**

## About REROUTE

REROUTE brings Next.js-style file-based routing to Python web frameworks like FastAPI, Flask, and Django.

**Key Features:**
- File-based routing (Next.js style)
- Class-based route handlers with lifecycle hooks
- FastAPI-style parameter injection (Query, Path, Header, Body, etc.)
- Pydantic model generation for data validation
- Powerful decorators (rate limiting, caching, validation)
- Multi-framework support (FastAPI, Flask, Django)
- CLI tools for scaffolding and code generation
- Environment configuration with .env support
- Zero configuration required

## Local Development

### Prerequisites

- Python 3.8+
- pip

### Setup

1. Clone this repository:
```bash
git clone https://github.com/cbsajan/reroute-docs.git
cd reroute-docs
```

2. Install dependencies:
```bash
pip install -r requirements.txt
# Or manually:
pip install mkdocs mkdocs-material mike
```

3. Run local server:
```bash
mkdocs serve
# Or with versioning:
mike serve
```

4. Visit [http://localhost:8000](http://localhost:8000)

### Building

Build the static site:
```bash
mkdocs build
```

Output will be in the `site/` directory.

## Documentation Structure

```
docs/
├── index.md                    # Home page
├── getting-started/            # Getting started guides
│   ├── installation.md
│   ├── quickstart.md
│   └── first-route.md
├── guides/                     # Feature guides
│   ├── file-routing.md
│   ├── class-routes.md
│   ├── decorators.md
│   ├── lifecycle-hooks.md
│   ├── configuration.md
│   └── cors.md
├── adapters/                   # Framework adapters
│   ├── fastapi.md
│   ├── flask.md
│   └── django.md
├── cli/                        # CLI documentation
│   ├── commands.md
│   └── scaffolding.md
├── api/                        # API reference
│   ├── routebase.md
│   ├── params.md              # Parameter injection
│   ├── decorators.md
│   ├── config.md              # Configuration & .env
│   └── adapters.md
├── examples/                   # Examples and recipes
│   ├── basic-crud.md
│   ├── authentication.md
│   ├── rate-limiting.md
│   └── caching.md
└── deployment/                 # Deployment guides
    ├── production.md
    └── docker.md
```

## Contributing

Contributions are welcome! To contribute to the documentation:

1. Fork this repository
2. Create a new branch (`git checkout -b docs/your-improvement`)
3. Make your changes
4. Test locally with `mkdocs serve`
5. Submit a pull request

### Documentation Guidelines

- Use clear, concise language
- Include code examples
- Test all code snippets
- Add screenshots where helpful
- Follow existing formatting style
- Keep pages focused and organized

## Deployment

Documentation is automatically deployed when a new version is released to PyPI from the main reroute repo.

**Manual deployment:** Go to Actions → "Deploy Documentation" → "Run workflow"

## Technology Stack

- **MkDocs**: Documentation framework
- **Material for MkDocs**: Beautiful, responsive theme
- **Mike**: Version management for documentation
- **GitHub Pages**: Hosting
- **GitHub Actions**: Automated CI/CD deployment

## Links

- **Main Repository**: [github.com/cbsajan/reroute](https://github.com/cbsajan/reroute)
- **PyPI Package**: [pypi.org/project/reroute](https://pypi.org/project/reroute)
- **Issues**: [github.com/cbsajan/reroute/issues](https://github.com/cbsajan/reroute/issues)

## License

This documentation is licensed under the [Apache License 2.0](LICENSE).

## Maintainer

**C B Sajan**
- GitHub: [@cbsajan](https://github.com/cbsajan)
- Email: cloud.ckhathri@gmail.com
