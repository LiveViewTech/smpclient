## Context

Pytest suite for client, transports, requests, MCUBoot tools, HCI firmware import, and Intercreate extension. Fixtures include sample MCUBoot images and filesystem blobs under `tests/fixtures/`.

## Tech

- `pytest`, `pytest-asyncio`, `pytest-cov`
- Invoke via `uv run task test` or `uv run task coverage`
- `dutfirmware/*` and `.claude/*` are not collected (`norecursedirs`)

## Architecture

Transport tests mock or unit-test framing/helpers without requiring physical radios. Client tests exercise upload/download routines and sequence handling in `tests/test_smp_client.py`.

## Patterns

- DO: keep new library code covered — coverage `fail_under = 91`
- DO: place transport-specific cases beside existing `tests/test_smp_*_transport.py` files
- DON'T: put Zephyr west trees or DUT builds under `tests/` (those belong in `dutfirmware/`)

## Key Files

- `tests/test_smp_client.py`
- `tests/test_smp_serial_transport.py`
- `tests/test_smp_ble_transport.py`
- `tests/test_smp_udp_transport.py`
- `tests/test_smp_bumble_transport.py`
- `tests/test_mcuboot_tools.py`
- `tests/extensions/test_intercreate.py`
