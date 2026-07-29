## Purpose

Python library implementing Simple Management Protocol (SMP) transports and a high-level async client so apps can manage MCU firmware, files, and configuration over serial, BLE, or UDP.

## Project Snapshot

- Tech: Python 3.10–3.14, AsyncIO, uv, hatchling, pytest, ruff, mypy
- Repo type: single package (`src/smpclient`) with examples, DUT firmware helpers, and MkDocs
- Package extras: `serial`, `ble`, `bumble`, `hci_firmware`, `all` (UDP always available)

## Commands

```
uv sync
uv run task fix
uv run task all
uv run task test
uv run task coverage
uv run task matrix
uv build
```

Task bodies live in `pyproject.toml` under `[tool.taskipy.tasks]`. CI runs lint/typecheck/test across OS × Python matrix in `.github/workflows/test.yaml`.

## Conventions

- Enable hooks with `git config core.hooksPath .githooks` (hook runs `uv run task fix`)
- Add runtime deps with `uv add <pkg>`; dev deps with `uv add --group dev <pkg>`
- Docs use Google-style docstrings; ruff/pydoclint enforce them on `src/smpclient`
- `dutfirmware/` is excluded from ruff, mypy, and pytest recursion
- Coverage gate: `fail_under = 91` in `pyproject.toml`

## Directory Map

- `src/smpclient/` → see `src/smpclient/AGENTS.md`
- `src/smpclient/transport/` → see `src/smpclient/transport/AGENTS.md`
- `examples/` → see `examples/AGENTS.md`
- `tests/` → see `tests/AGENTS.md`
- `dutfirmware/` → see `dutfirmware/AGENTS.md`
- `docs/` → MkDocs sources; build via `.github/workflows/test-docs.yaml`

## Gotchas

- **Do not** pass `upgrade=True` to `SMPClient.upload` unless you accept brick risk — confirm the image from the upgraded app instead (`src/smpclient/__init__.py`)
- Small MTUs: first upload packet with `use_sha=True` can yield `MGMT_ERR.EINVAL`; raise MTU or set `use_sha=False`
- Importing serial/BLE/bumble/hci without the matching extra raises `ImportError` at import time
