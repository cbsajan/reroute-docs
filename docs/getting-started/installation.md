# Installation

Learn how to install REROUTE in your Python project.

## Requirements

- **Python**: 3.8 or higher
- **Framework**: FastAPI, Flask, or Django (at least one)

## Basic Installation

Install REROUTE using pip:

```bash
pip install reroute
```

This installs the core REROUTE package without framework dependencies.

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

=== "Django"

    ```bash
    pip install reroute[django]
    ```

    This includes Django support (coming soon).

## Complete Installation

To install REROUTE with all framework adapters:

```bash
pip install reroute[all]
```

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
```

## Update REROUTE

To update to the latest version:

```bash
pip install --upgrade reroute
```

## Uninstall

To remove REROUTE:

```bash
pip uninstall reroute
```

## Next Steps

- [Quick Start Guide](quickstart.md) - Build your first app
- [First Route](first-route.md) - Create your first route
