# Getting Started

**Welcome to REROUTE!**

---

Here's where you'll learn how to get started with **REROUTE** and build your first file-based routed API.

This tutorial will guide you through installation, your first route, and core concepts step by step.

## What is REROUTE?

REROUTE is a modern routing framework that brings Next.js-style file-based routing to Python web frameworks. Instead of manually registering routes, you organize them in folders and files - REROUTE handles the rest.

## Prerequisites

- Python 3.8 or higher
- A web framework (FastAPI, Flask, or Django)
- Basic understanding of REST APIs

## Installation Options

### From PyPI (Recommended)

```bash
pip install reroute
```

### With specific framework support

=== "FastAPI"
    ```bash
    pip install reroute[fastapi]
    ```

=== "Flask"
    ```bash
    pip install reroute[flask]
    ```

=== "All frameworks"
    ```bash
    pip install reroute[all]
    ```

### From Source

```bash
git clone https://github.com/cbsajan/reroute.git
cd reroute
pip install -e .
```

## Next Steps

<div class="grid cards" markdown>

-   :rocket: [**Quick Start**](quickstart.md)

    ---

    Build your first REROUTE app in 5 minutes

-   :material-file-tree: [**First Route**](first-route.md)

    ---

    Create your first file-based route

</div>
