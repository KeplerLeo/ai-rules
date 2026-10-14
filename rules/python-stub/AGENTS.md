# AGENTS.md

Guidance for AI coding agents working in this repository (Python project).

## Environment

- Always work inside a `venv` (`python -m venv venv`). Keep `requirements.txt` pinned.
- Never commit `*.db`, `*.sqlite`, `__pycache__/`, `venv/`, `.env`, or credentials/certificates.

## Code Style

- `snake_case` for modules, functions, variables; `PascalCase` for classes.
- Entry points: `main.py` / `app.py`. Business logic goes in `services/`, HTTP routes in `routers/`, data models in `models.py` — don't pile everything into the entry file.
- Use the `logging` module, never `print()`, outside of one-off scripts.
- Configuration via `.env` + `os.environ`; validate required vars at startup and fail fast naming the missing one.
- Two blank lines between top-level defs; keep files readable top-to-bottom: constants → public functions/classes → private helpers.

## Errors

- `try/except` at boundaries only (route handlers, scripts). Never swallow exceptions silently — log or rethrow. Catch specific exceptions, not bare `except:`.

## Git Hygiene

- Small commits, imperative subject lines, match the repo's existing style.
- Never commit generated artifacts, logs, or database dumps.
