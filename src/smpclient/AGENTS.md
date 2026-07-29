## Context

Core library: `SMPClient` async API, request/response generics, exceptions, MCUBoot image inspection (`mcuimg` CLI), and Intercreate upload extension.

## Tech

- Python AsyncIO client over `SMPTransport`
- Depends on `smp` for wire message types; `intelhex` for HEX image load
- Optional transports gated by package extras (see `src/smpclient/transport/AGENTS.md`)

## Architecture

`SMPClient(transport, address)` → `connect` / `request` / `upload` / file helpers. `request` sends `SMPRequest.BYTES`, validates sequence, and parses `_Response` / `_ErrorV1` / `_ErrorV2`. Narrow results with `success` / `error` from `smpclient.generics`.

## Patterns

- DO: wrap `smp` request classes with `_Response` / `_ErrorV1` / `_ErrorV2` bindings — see `src/smpclient/requests/image_management.py`
- DO: branch on `success(response)` / `error(response)` after `client.request` — see `examples/ble/helloworld.py`
- DO: use `async with SMPClient(...)` for connect/disconnect lifecycle — see `src/smpclient/__init__.py`
- DON'T: mark images confirmed via `upload(..., upgrade=True)` for normal DFU — see docstring on `SMPClient.upload`
- DON'T: invent new wire schemas here; extend via `smp` / user-group wrappers under `src/smpclient/requests/user/`

## Key Files

- `src/smpclient/__init__.py` — `SMPClient`
- `src/smpclient/generics.py` — `SMPRequest`, `success` / `error` narrowing
- `src/smpclient/mcuboot.py` — `ImageInfo`, `mcuimg` CLI
- `src/smpclient/extensions/intercreate.py` — `ICUploadClient`
- `src/smpclient/exceptions.py` — client/transport error types
