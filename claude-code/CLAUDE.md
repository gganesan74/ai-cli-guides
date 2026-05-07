# Project conventions

## Python environment
This project uses **uv**. Do NOT use `pip` or `python -m pip` directly.

### Commands
- Run a Python script: `uv run python <script>`
- Run pytest: `uv run pytest`
- Add a dependency: `uv add <package>`
- Add a dev dependency: `uv add --dev <package>`
- Sync the environment: `uv sync`

### Forbidden
- `pip install ...` — use `uv add` instead
- `python -m pytest` — use `uv run pytest` instead
- Activating `.venv` manually — `uv run` handles this

If you find yourself typing `pip`, stop and use `uv` instead.
