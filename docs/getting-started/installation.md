# Installation

Learn how to install REROUTE in your Python project.

!!! info "Current Version"
    **Latest Release**: v0.1.2
    **Release Date**: 20-11-2025
    **Python Support**: 3.8, 3.9, 3.10, 3.11, 3.12

## Requirements

- **Python**: 3.8 or higher
- **Framework**: FastAPI or Flask (at least one)

## Basic Installation

Install REROUTE using pip:

```bash
pip install reroute
```

This installs the core REROUTE package without framework dependencies.

!!! tip "Lazy Loading"
    Since v0.1.2, REROUTE uses lazy imports for framework adapters. You only need to install the framework you're actually using!

## Framework-Specific Installation

Install REROUTE with support for your specific framework:

=== "FastAPI"

    ```bash
    pip install reroute[fastapi]
    ```

    This includes FastAPI and Uvicorn.

=== "Flask"

    ```bash
    pip install reroute[flask]
    ```

    This includes Flask and its dependencies.


## Development Installation

For contributing to REROUTE:

```bash
# Clone the repository
git clone https://github.com/cbsajan/reroute.git
cd reroute

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install in editable mode with dev dependencies
pip install -e ".[dev]"
```

## Verify Installation

Check that REROUTE is installed correctly:

```bash
python -c "import reroute; print(reroute.__version__)"
```

Or use the CLI:

```bash
reroute --version
# Output: REROUTE CLI v0.1.2
```

!!! success "Automatic Update Notifications"
    Since v0.1.2, REROUTE automatically checks for updates when you run `reroute --version` or any CLI command. You'll see a notification if a newer version is available!

## Update REROUTE

To update to the latest version:

```bash
pip install --upgrade reroute
```

REROUTE will notify you when updates are available through the CLI.

## Uninstall

To remove REROUTE:

```bash
pip uninstall reroute
```

## Next Steps

- [Quick Start Guide](quickstart.md) - Build your first app
- [First Route](first-route.md) - Create your first route
