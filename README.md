# Test Remote Server

An asynchronous Model Context Protocol (MCP) server for tracking expenses. The project is built with Python, FastMCP, and SQLite accessed through `aiosqlite`.

## Project contents

| File | Purpose |
| --- | --- |
| `main.py` | MCP server entry point and expense-related server logic. |
| `categories.json` | Expense category and subcategory definitions used by the project. |
| `expenses.db` | SQLite database file for expense data; it may be created or updated when the server runs. |
| `pyproject.toml` | Python project metadata, dependencies, build configuration, and declared command-line entry point. |

## Architecture

```text
MCP client
   │ MCP requests
   ▼
FastMCP server (main.py)
   ├── loads category definitions from categories.json
   └── performs asynchronous database access with aiosqlite
             │
             ▼
         expenses.db
```

The MCP client sends requests to the FastMCP server. The server provides the expense functionality and uses the category data to classify expenses. Persistent expense records are stored in SQLite, with asynchronous database operations handled by `aiosqlite`.

## Requirements

- Python 3.11 or newer
- [`uv`](https://docs.astral.sh/uv/) for dependency and project management

The dependencies declared in `pyproject.toml` are `fastmcp` and `aiosqlite`.

## Install

From the project directory, sync the environment and dependencies:

```bash
uv sync
```

## Run

Run the Python entry-point file directly:

```bash
uv run python main.py
```

The project also declares a `test-remote-server` command in `pyproject.toml`. That command targets `test_remote_server:main`; it will work when the corresponding importable package/module is present in the project.

To use the server, configure an MCP client to launch or connect to it using the transport supported by the server and client. The exact client configuration depends on where the server is hosted and which MCP transport `main.py` configures.

## Expense categories

`categories.json` groups subcategories under top-level categories such as food, transport, housing, utilities, health, education, travel, and investments. Update this file to adjust the available category vocabulary. Keep the JSON valid when editing it.

## ChatGPT and subscription note

This repository contains the MCP server; it is separate from ChatGPT. A ChatGPT subscription is not inherently needed to install or run the Python server locally. Connecting to it through ChatGPT may depend on the ChatGPT account, available MCP features, and whether the server is reachable using a supported connection method. You can still develop and run the server locally and use another compatible MCP client.

## Development notes

- Do not commit real financial records or secrets. Treat `expenses.db` as private user data.
- Back up the database before manually changing or replacing it.
- The precise MCP tools, database schema, transport, and environment variables are defined by `main.py`; consult that file when extending the server.
