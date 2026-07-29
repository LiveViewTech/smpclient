## Context

Runnable samples for BLE, USB/serial, and UDP against SMP servers. Prebuilt DUT assets live under `examples/duts/`; firmware build overlays are in `dutfirmware/`.

## Tech

- Requires matching transport extras (`ble`, `serial`, etc.)
- Scripts are asyncio entrypoints (`asyncio.run(main)`)

## Architecture

Examples construct a concrete transport, open `SMPClient`, then call `request` or higher-level upload/upgrade helpers. Hardware-oriented USB upgrade flow is in `examples/usb/upgrade.py`.

## Patterns

- DO: start from `examples/ble/helloworld.py` or `examples/usb/helloworld.py` for minimal request/response
- DO: follow `examples/ble/upgrade.py` / `examples/usb/upgrade.py` for DFU sequencing (state read, upload, test/confirm, reset)
- DON'T: assume a DUT is present — many scripts expect hardware or files under `examples/duts/`

## Key Files

- `examples/ble/helloworld.py`
- `examples/ble/upgrade.py`
- `examples/usb/upgrade.py`
- `examples/usb/upload_file.py`
- `examples/udp/helloworld.py`
- `examples/README.md`
