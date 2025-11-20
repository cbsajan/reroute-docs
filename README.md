# REROUTE Documentation

Official documentation for [REROUTE](https://github.com/cbsajan/reroute) - Modern file-based routing for Python backends.

## Live Documentation

Visit the live documentation at: **[cbsajan.github.io/reroute-docs](https://cbsajan.github.io/reroute-docs)**

## About REROUTE

REROUTE brings Next.js-style file-based routing to Python web frameworks like FastAPI, Flask, and Django.

**Key Features:**
- File-based routing (Next.js style)
- Class-based route handlers
- Powerful decorators (rate limiting, caching, validation)
- Multi-framework support
- CLI tools for scaffolding
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
pip install mkdocs mkdocs-material mkdocs-i18n
```

3. Run local server:
```bash
mkdocs serve
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
│   ├── decorators.md
│   └── lifecycle-hooks.md
├── adapters/                   # Framework adapters
│   ├── fastapi.md
│   ├── flask.md
│   └── django.md
├── cli/                        # CLI documentation
│   └── commands.md
├── api/                        # API reference
│   └── reference.md
├── examples/                   # Examples and recipes
│   └── basic-crud.md
└── deployment/                 # Deployment guides
    └── production.md
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

## Multi-language Support

This documentation supports multiple languages:

- English (default)
- Spanish
- French
- German
- Chinese
- Japanese

To add a new language, see the [Translation Guide](TRANSLATING.md).

## Deployment

Documentation is automatically deployed to GitHub Pages when changes are pushed to the `main` branch.

The deployment workflow:
1. Builds the documentation with MkDocs
2. Deploys to the `gh-pages` branch
3. GitHub Pages serves the content

## Technology Stack

- **MkDocs**: Documentation framework
- **Material for MkDocs**: Theme
- **GitHub Pages**: Hosting
- **GitHub Actions**: CI/CD

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
