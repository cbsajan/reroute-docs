# Installation

Learn how to install REROUTE in your Python project.

!!! info "Current Version"
    **Latest Release**: v0.1.5
    **Release Date**: 23-11-2025
    **Python Support**: 3.8, 3.9, 3.10, 3.11, 3.12

## Requirements

- **Python**: 3.8 or higher
- **Framework**: FastAPI or Flask (at least one)

## Basic Installation

=== "pip (Traditional)"

    ```bash
    pip install reroute
    ```

    This installs the core REROUTE package without framework dependencies.

=== "uv (Recommended)"

    ```bash
    uv pip install reroute
    ```

    [uv](https://github.com/astral-sh/uv) is an ultra-fast Python package installer written in Rust. It's 10-100x faster than pip!

    **Install uv first:**
    ```bash
    # macOS/Linux
    curl -LsSf https://astral.sh/uv/install.sh | sh

    # Windows
    powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

    # Or with pip
    pip install uv
    ```

!!! tip "Lazy Loading"
    Since v0.1.2, REROUTE uses lazy imports for framework adapters. You only need to install the framework you're actually using!

!!! info "pyproject.toml Support"
    Starting with v0.1.5, REROUTE projects include `pyproject.toml` for modern dependency management with uv. Legacy `requirements.txt` will be removed in v0.3.0.

## Framework-Specific Installation

Install REROUTE with support for your specific framework:

=== "FastAPI (pip)"

    ```bash
    pip install reroute[fastapi]
    ```

    This includes FastAPI and Uvicorn.

=== "FastAPI (uv)"

    ```bash
    uv pip install reroute[fastapi]
    ```

    Faster installation with uv.

=== "Flask (pip)"

    ```bash
    pip install reroute[flask]
    ```

    This includes Flask, Flask-CORS, and Spectree.

=== "Flask (uv)"

    ```bash
    uv pip install reroute[flask]
    ```

    Faster installation with uv.


## Development Installation

For contributing to REROUTE:

=== "pip"

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

=== "uv (Faster)"

    ```bash
    # Clone the repository
    git clone https://github.com/cbsajan/reroute.git
    cd reroute

    # Create virtual environment with uv
    uv venv
    source .venv/bin/activate  # On Windows: .venv\Scripts\activate

    # Install in editable mode with dev dependencies
    uv pip install -e ".[dev]"
    ```

## Verify Installation

Check that REROUTE is installed correctly:

```bash
python -c "import reroute; print(reroute.__version__)"
```

Or use the CLI:

```bash
reroute --version
# Output: REROUTE CLI v0.1.5
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
