# CLI

REROUTE includes a command-line interface for scaffolding and managing your file-based routes.

## Installation

The CLI is installed automatically with REROUTE:

```bash
pip install reroute
```

Verify installation:

```bash
reroute --version
```

## Quick Start

### Create a New Project

```bash
reroute init my-project --framework fastapi
cd my-project
```

### Create a Route

```bash
reroute create route user
```

This creates `app/routes/user/page.py` with boilerplate code.

### Create a Nested Route

```bash
reroute create route user/profile
```

Creates `app/routes/user/profile/page.py`.

## Available Commands

<div class="grid cards" markdown>

-   :material-plus-circle: [**init**](commands.md#init)

    ---

    Create a new REROUTE project with FastAPI/Flask

-   :material-file-plus: [**create route**](commands.md#generate-route-create-route)

    ---

    Generate a new route file with HTTP methods

-   :material-table: [**create crud**](commands.md#generate-crud-create-crud)

    ---

    Generate full CRUD operations (Create, Read, Update, Delete)

-   :material-code-braces: [**create model**](commands.md#generate-model-create-model)

    ---

    Generate Pydantic models for data validation

</div>

## CLI Options

All commands support:

- `--help` - Show command help
- `--verbose` - Enable verbose output
- `--version` - Show REROUTE version

## Examples

### Initialize with FastAPI

```bash
reroute init my-api --framework fastapi
```

### Initialize with Flask

```bash
reroute init my-app --framework flask
```

### Create Multiple Routes

```bash
reroute create route user
reroute create route product
reroute create route order
```

### Create with Custom Methods

```bash
reroute create route user --methods GET POST PUT DELETE
```

## Interactive Mode

The CLI features an interactive mode with auto-completion:

```bash
reroute
```

Navigate through options with arrow keys and select with Enter.

## Configuration

CLI behavior can be customized via `.rerouterc` file:

```json
{
  "default_framework": "fastapi",
  "routes_dir": "app/routes",
  "template_style": "minimal"
}
```

## Learn More

- [Command Reference](commands.md) - Detailed command documentation
- [Scaffolding](scaffolding.md) - Project structure and templates
