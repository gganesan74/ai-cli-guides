# Project Conventions

## Python environment
This project uses **uv** for dependency and environment management.

- Install packages: `uv pip install <pkg>` (never `pip install`)
- Add to pyproject.toml: `uv add <pkg>`
- Add dev dep: `uv add --dev <pkg>`
- Run Python: `uv run python <script>` or `uv run <command>`
- Run tests: `uv run pytest`
- Sync env: `uv sync`

Do not use `pip`, `pip3`, or `python -m pip` directly.
The venv is managed by uv at `.venv/` — do not modify it manually.
