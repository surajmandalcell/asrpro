# Repository Guidelines

## Project Structure & Module Organization
- `asrpro/`: Application package (entry: `__main__.py`, bootstrap: `main.py`).
- `asrpro/ui/`: PySide6 UI (window, tray, overlay, title bar).
- `asrpro/server.py`: FastAPI server (OpenAI‑compatible endpoints).
- `assets/`: App assets (e.g., icons).
- `run.py`: Simple launcher; mirrors `python -m asrpro`.
- `build_nuitka.py`: One‑file Windows build using Nuitka.

## Build, Test, and Development Commands
- Install: `python -m pip install -r requirements.txt`.
- Run (module): `python -m asrpro`.
- Run (script): `python run.py`.
- Build (Nuitka): `python build_nuitka.py` → outputs `dist/asrpro.exe`.
- Server check: `curl http://127.0.0.1:7341/health` (when server enabled).

## Coding Style & Naming Conventions
- Python 3.11+, 4‑space indentation, PEP 8 naming: `snake_case` for functions/modules, `CapWords` for classes.
- Prefer type hints and concise docstrings; keep UI logic in `asrpro/ui/` and model/server logic in dedicated modules.
- No formatter config is enforced; if used locally, prefer Black + isort defaults.

## Testing Guidelines
- No formal test suite yet. For new code, add `pytest` tests under `tests/` with `test_*.py` and unit‑test pure logic (e.g., config merges, model selection).
- Manual checks: launch app, verify tray icon, hotkey record/transcribe, and server endpoints: `/v1/models`, `/v1/audio/transcriptions`, `/health`.

## Commit & Pull Request Guidelines
- Commits: prefer Conventional Commits (e.g., `feat:`, `fix:`, `chore:`). Keep messages imperative and scoped (e.g., `feat(ui): add dark tray icon`).
- PRs: include a clear description, linked issues, reproduction steps, and screenshots/GIFs for UI. Note OS/GPU context when relevant.
- Keep PRs focused; update README/AGENTS.md when behavior or commands change.

## Security & Configuration Tips
- Config lives at `~/.asrpro/config.json` (created on first run). Avoid committing local config.
- Default server binds `127.0.0.1:7341`. Do not expose publicly without a reverse proxy and auth.
- Single‑instance lock: `~/.asrpro.lock`. If the app crashes, it is safe to delete this file.

