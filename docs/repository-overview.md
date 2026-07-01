# Repository Overview

This repository distributes firmware update packages for FTW Robotics Hopper drones and controllers. It is an artifact and manifest repository, not a firmware source repository.

## Top-Level Files

- `README.md`: Original human-facing project documentation.
- `AGENTS.md`: Agent-facing operating guide.
- `firmwares.json`: Main firmware availability manifest.
- `firmwares_v2.json`: Secondary manifest file with the same top-level shape, currently empty.
- `hopper/`: Hopper drone DFU zip artifacts.
- `controller/`: Controller DFU zip artifacts.

## Manifest Files

`firmwares.json` currently has two arrays:

- `hopperFirmwares`
- `controllerFirmwares`

Each entry describes a version, relative download path, audience status, update priority, and compatibility key.

`firmwares_v2.json` currently contains empty arrays:

```json
{
  "hopperFirmwares": [],
  "controllerFirmwares": []
}
```

Do not assume `firmwares_v2.json` is unused. Before removing, renaming, or populating it, verify app/client behavior.

## Current Firmware Inventory

### Hopper

| Version | File | Status | Priority | Key |
| --- | --- | --- | --- | --- |
| `1.0.30` | `hopper/dfu_application-1.0.30.zip` | `beta` | `optional` | `key1` |
| `1.0.32` | `hopper/dfu_application-1.0.32.zip` | `beta` | `optional` | `key1` |
| `1.0.34` | `hopper/dfu_application-1.0.34.zip` | `beta` | `optional` | `key1` |
| `1.0.36` | `hopper/dfu_application-1.0.36.zip` | `production` | `critical` | `key1` |

Current Hopper zip manifests identify the board as `hopper_nrf5340_cpuapp` and the SoC as `nRF5340_CPUAPP_QKAA`.

### Controller

| Version | File | Status | Priority | Key |
| --- | --- | --- | --- | --- |
| `1.0.17` | `controller/dfu_application-controller-1.0.17.zip` | `beta` | `optional` | `controllerKey1` |
| `1.0.20` | `controller/dfu_application-controller-1.0.20.zip` | `beta` | `optional` | `controllerKey1` |
| `1.0.21` | `controller/dfu_application-controller-1.0.21.zip` | `beta` | `optional` | `controllerKey1` |

Current controller zip manifests identify the board as `nRF52832_controller` and the SoC as `nRF52832_QFAA`.

## DFU Zip Contents

Every current DFU zip contains exactly two files:

- `app_update.bin`
- `manifest.json`

The internal `manifest.json` references `app_update.bin` in its `files[].file` field.

## Things This Repo Does Not Contain

- Firmware source code.
- Build scripts for firmware.
- CI configuration.
- App source code that consumes the manifests.

