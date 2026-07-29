## Context

`SMPTransport` Protocol plus serial, BLE (bleak), UDP, and bumble (external HCI) implementations. Shared GATT UUIDs live in `src/smpclient/transport/__init__.py`.

## Tech

- Serial: `pyserial` via extra `smpclient[serial]` — UART/USB CDC/CAN framing
- BLE: `bleak` via `smpclient[ble]`
- UDP: no extra; default SMP port `1337` in `SMPUDPTransport.connect`
- Bumble: `bumble` (+ libusb/platformdirs) via `smpclient[bumble]`; CLI `smpbumble`
- HCI firmware re-export: `smpclient[hci_firmware]` → `src/smpclient/transport/firmware/hci.py`

## Architecture

Transports implement `connect` / `disconnect` / `send` / `receive` / `send_and_receive`, expose `mtu` / `max_unencoded_size`, and accept `initialize(buffer_size)` from the client after OS management probing.

## Patterns

- DO: install only needed extras; missing deps raise `ImportError` naming the extra — see `src/smpclient/transport/serial.py`, `ble.py`, `bumble/__init__.py`
- DO: treat `SMP_SERVICE_UUID` / `SMP_CHARACTERISTIC_UUID` as the single GATT source of truth — `src/smpclient/transport/__init__.py`
- DO: use `SMPBumbleTransport` when driving an external HCI controller — `src/smpclient/transport/bumble/__init__.py`
- DON'T: assume UDP MTU equals max payload; IP/UDP overhead is subtracted per RFC 8085 — `src/smpclient/transport/udp.py`

## Key Files

- `src/smpclient/transport/__init__.py` — Protocol + UUIDs
- `src/smpclient/transport/serial.py` — `SMPSerialTransport`
- `src/smpclient/transport/ble.py` — `SMPBLETransport`
- `src/smpclient/transport/udp.py` — `SMPUDPTransport`
- `src/smpclient/transport/bumble/__main__.py` — `smpbumble` CLI
