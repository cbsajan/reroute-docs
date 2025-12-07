# Getting Started

**Welcome to REROUTE!**

---

Here's where you'll learn how to get started with **REROUTE** and build your first file-based routed API.

This tutorial will guide you through installation, your first route, and core concepts step by step.

## What is REROUTE?

REROUTE is a modern routing framework that brings Next.js-style file-based routing to Python web frameworks. Instead of manually registering routes, you organize them in folders and files - REROUTE handles the rest.

## Prerequisites

- Python 3.8 or higher
- UV package manager (recommended) or pip
- A web framework (FastAPI or Flask)
- Basic understanding of REST APIs

## Installation Options

### Installation

=== "UV (Recommended)"
    ```bash
    # Install UV first (if not already installed)
    pip install uv

    # Install REROUTE with UV
    uv add reroute
    ```

=== "Traditional pip"
    ```bash
    pip install reroute
    ```

### Framework Support

=== "FastAPI"
    ```bash
    # UV
    uv add reroute[fastapi]

    # pip
    pip install reroute[fastapi]
    ```

=== "Flask"
    ```bash
    # UV
    uv add reroute[flask]

    # pip
    pip install reroute[flask]
    ```

=== "All frameworks"
    ```bash
    # UV
    uv add reroute[all]

    # pip
    pip install reroute[all]
    ```

### From Source

```bash
git clone https://github.com/cbsajan/reroute.git
cd reroute
pip install -e .
```

## Quick Project Setup

### Using UV (Recommended)

```bash
# Create virtual environment
uv venv

# Activate (Windows)
.venv\Scripts\activate

# Activate (Linux/Mac)
source .venv/bin/activate

# Create new project
reroute init myapi

# Install dependencies
cd myapi
uv sync

# Run the application
uv run main.py
```

### Using Traditional pip

```bash
# Create virtual environment
python -m venv venv

# Activate (Windows)
venv\Scripts\activate

# Activate (Linux/Mac)
source venv/bin/activate

# Create new project
reroute init myapi --package-manager pip

# Install dependencies
cd myapi
pip install -r requirements.txt

# Run the application
python main.py
```

**Note**: UV is the default package manager. Use `--package-manager pip` to use traditional pip.

## Next Steps

<div class="grid cards" markdown>

-   :rocket: [**Quick Start**](quickstart.md)

    ---

    Build your first REROUTE app in 5 minutes

-   :material-file-tree: [**First Route**](first-route.md)

    ---

    Create your first file-based route

</div>
