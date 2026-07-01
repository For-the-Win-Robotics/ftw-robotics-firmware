# Firmware Manifest Schema

The active manifest is `firmwares.json`. It is JSON with two top-level arrays:

```json
{
  "hopperFirmwares": [],
  "controllerFirmwares": []
}
```

## Entry Shape

Each firmware entry has this shape:

```json
{
  "version": "1.0.36",
  "firmwarePath": "/hopper/dfu_application-1.0.36.zip",
  "status": "production",
  "priority": "critical",
  "key": "key1"
}
```

## Fields

### `version`

Firmware version displayed and compared by the consuming app. Keep it aligned with:

- The DFU zip filename.
- The zip's internal `manifest.json` value at `files[].version_MCUBOOT`, ignoring the `+0` build metadata suffix when present.

### `firmwarePath`

Absolute web path to the firmware zip from the repo/site root.

Examples:

- `/hopper/dfu_application-1.0.36.zip`
- `/controller/dfu_application-controller-1.0.21.zip`

Even though this looks absolute, it is not a local filesystem path. For local validation, strip the leading `/`.

### `status`

Audience availability:

- `beta`: For developers and testers.
- `production`: For general users.

Use `beta` for first exposure of any new firmware.

### `priority`

How aggressively the app should ask users to update:

- `optional`: Update is available but not necessary.
- `recommended`: App should prompt for update, but it is not required.
- `critical`: App may force the update.

Treat `critical` as high-impact because it can block normal use until users update.

### `key`

Compatibility identifier used to ensure updates only occur within the same firmware family.

Current keys:

- Hopper: `key1`
- Controller: `controllerKey1`

Do not change keys for an existing firmware line without confirming app and bootloader expectations.

## Sorting

Current entries are grouped by device and sorted ascending by version. Preserve that convention unless there is a confirmed consumer requirement to sort differently.

## Current Allowed Values

No formal JSON Schema file exists in the repository. The practical allowed values are:

```json
{
  "status": ["beta", "production"],
  "priority": ["optional", "recommended", "critical"]
}
```

