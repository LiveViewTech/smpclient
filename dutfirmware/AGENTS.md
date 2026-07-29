## Context

Zephyr/west helpers and Kconfig overlays to build SMP DUT firmware for boards used with `examples/`. Not part of the Python package build or pytest collection.

## Tech

- Separate venv + `west` workflow (see `dutfirmware/README.md`)
- Activate with `. ./envr.ps1` from `dutfirmware/`
- Board overlays/conf files for Nordic and other targets

## Architecture

Build `zephyr/samples/subsys/mgmt/mcumgr/smp_svr` (or NRF SDK fork) with `EXTRA_CONF_FILE` pointing at overlays in this folder. Flash with `west flash`. Resulting images are consumed by examples under `examples/duts/`.

## Patterns

- DO: run all west/env commands from `dutfirmware/` as documented in `dutfirmware/README.md`
- DO: compose overlays (e.g. `ble_a_smp_dut.conf`, `usb_smp_dut_512_8_4096.conf`) via `-DEXTRA_CONF_FILE`
- DON'T: expect `uv run task test` to build or flash these images

## Key Files

- `dutfirmware/README.md`
- `dutfirmware/envr-default`
- `dutfirmware/envr.ps1`
- `dutfirmware/ble_a_smp_dut.conf`
- `dutfirmware/usb_a_smp_dut.conf`
- `dutfirmware/mcuboot_usb.conf`
