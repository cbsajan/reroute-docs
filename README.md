# REROUTE Documentation

Official documentation for [REROUTE](https://github.com/cbsajan/reroute) - Modern file-based routing for Python backends.

## 🚀 Latest Version: v0.2.0 - Security Release

REROUTE v0.2.0 is now **production-ready** with comprehensive security hardening!
- ✅ **Security Score: 9/10** (Previously 6.5/10)
- ✅ **All critical vulnerabilities fixed**
- ✅ **42 security tests passing**
- ✅ **New features enabled**

## Live Documentation

Visit the live documentation at: **[cbsajan.github.io/reroute-docs](https://cbsajan.github.io/reroute-docs)**

## About REROUTE

REROUTE brings Next.js-style file-based routing to Python web frameworks like FastAPI, Flask, and Django.

### ✨ Key Features (v0.2.0+)

**🔒 Enterprise-Grade Security**
- SQL injection prevention with column validation
- Command injection protection in CLI
- Template injection prevention with secure Jinja2
- Path traversal protection with symlink validation
- Request size limits for DoS protection
- Secure file permissions (0600)
- Thread-safe operations with atomic locks
- Comprehensive security logging (OWASP A09 compliant)

**🛠️ Developer Experience**
- File-based routing (Next.js style)
- Class-based route handlers with lifecycle hooks
- FastAPI-style parameter injection (Query, Path, Header, Body, etc.)
- Pydantic model generation for data validation
- Powerful decorators (rate limiting, caching, validation)
- Multi-framework support (FastAPI, Flask, Django)
- CLI tools for scaffolding and code generation
- Environment configuration with .env support
- Zero configuration required

**🆕 New in v0.2.0**
- **Database Models**: `reroute create dbmodel` - Generate SQLAlchemy models
- **Auth Scaffolding**: `reroute create auth` - JWT authentication system
- **Database Setup**: `reroute init --database` - Auto-configure database
- **Security Testing**: Comprehensive test suite with 42 tests

## 🚀 Quick Start

```bash
# Install REROUTE
pip install reroute[fastapi]

# Create a new project with database
reroute init my-api --framework fastapi --database postgresql
cd my-api

# Create authentication
reroute create auth --method jwt

# Create a database model
reroute create dbmodel --name User

# Create your first route
reroute create route --path /users --name Users

# Run your app
uvicorn main:app --reload
```

## 📚 Learn More

- **[Getting Started](docs/getting-started/index.md)** - Installation and first route
- **[Security Guide](docs/guides/security.md)** - Enterprise security features
- **[CLI Commands](docs/cli/commands.md)** - All available commands
- **[API Reference](docs/api/index.md)** - Detailed API documentation

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

## 📋 Security Status

[![Security Score](https://img.shields.io/badge/Security-9%2F10-brightgreen)](https://github.com/cbsajan/reroute/security)
[![OWASP Compliant](https://img.shields.io/badge/OWASP-A09%20Compliant-green)](https://owasp.org/www-project-application-security-verification-standard/)
[![42 Tests Passing](https://img.shields.io/badge/Tests-42%2F42-passing-brightgreen)](https://github.com/cbsajan/reroute)

- ✅ SQL Injection Prevention
- ✅ Command Injection Prevention
- ✅ Template Injection Prevention
- ✅ Path Traversal Prevention
- ✅ DoS Protection (Rate Limiting, Request Size Limits)
- ✅ Secure File Permissions
- ✅ Thread-Safe Operations
- ✅ Security Logging (OWASP A09)

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
│   ├── security.md           # 🆕 Enterprise security features
│   ├── decorators.md
│   ├── lifecycle-hooks.md
│   ├── configuration.md
│   └── cors.md
├── adapters/                   # Framework adapters
│   ├── fastapi.md
│   ├── flask.md
│   └── django.md
├── cli/                        # CLI documentation
│   ├── commands.md           # 🆕 Includes dbmodel & auth commands
│   ├── index.md
│   └── scaffolding.md
├── api/                        # API reference
│   ├── routebase.md
│   ├── params.md              # Parameter injection
│   ├── decorators.md
│   ├── config.md              # Configuration & .env
│   └── adapters.md
├── examples/                   # Examples and recipes
│   ├── authentication.md      # 🆕 JWT auth example
│   ├── database.md            # 🆕 Database model example
│   ├── basic-crud.md
│   ├── rate-limiting.md
│   └── caching.md
└── deployment/                 # Deployment guides
    ├── production.md         # 🆕 Security configuration
    └── docker.md
```

## 🎯 v0.2.0 Security Release Highlights

This release focuses on making REROUTE **production-ready** with enterprise-grade security:

### 🔒 Critical Security Fixes
- **SQL Injection Prevention**: Column validation in database queries
- **Command Injection Prevention**: Input sanitization in CLI tools
- **Template Injection Prevention**: Secure Jinja2 configuration
- **Path Traversal Prevention**: Comprehensive file path validation

### 🛡️ New Security Features
- **Request Size Limits**: Prevents DoS attacks (16MB default)
- **Secure File Permissions**: Automatic 0600 for sensitive files
- **Thread-Safe Operations**: Atomic rate limiting and caching
- **Security Logging**: OWASP A09 compliant event logging

### ✨ Unlocked Features
- **Database Models**: `reroute create dbmodel`
- **JWT Authentication**: `reroute create auth`
- **Database Setup**: `reroute init --database`

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

📖 **[Complete Publishing Guide](PUBLISHING.md)** - Detailed deployment instructions

### Automatic Deployment (GitHub Actions)

Documentation is automatically deployed to GitHub Pages when you push to `main`:

```bash
git add .
git commit -m "docs: update feature documentation"
git push origin main
```

**What happens automatically:**
1. GitHub Actions triggers on push to main
2. Builds documentation with MkDocs
3. Deploys with mike for version management
4. Updates `gh-pages` branch
5. Live in ~2-3 minutes at: `https://cbsajan.github.io/reroute-docs`

### Manual Deployment

For manual control or version releases:

```bash
# Deploy new version
mike deploy 0.2.0 latest --update-aliases
mike set-default latest
git push origin gh-pages
```

### Versioning with Mike

The documentation supports multiple versions:
- **Latest**: Always points to the newest version
- **Specific versions**: v0.1.0, v0.2.0, etc.
- **Dev**: Development version from main branch

**Commands:**
```bash
mike list                    # List all versions
mike serve                   # Serve locally with version selector
mike delete 0.1.0            # Delete a version
```

### First-Time Setup

1. **Enable GitHub Pages:**
   - Go to Settings → Pages
   - Source: `gh-pages` branch, `/ (root)` folder
   - Save

2. **First deployment:**
   ```bash
   mike deploy 0.1.0 latest --update-aliases
   mike set-default latest
   git push origin gh-pages
   ```

3. **Update URL placeholders** in reroute repository:
   - Replace `DOCS_URL_PLACEHOLDER` with `https://cbsajan.github.io/reroute-docs`
   - Files: README.md, CONTRIBUTING.md, archive/README.md

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
