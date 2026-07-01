# AGENTS.md

Guidance for AI agents and maintainers working in this repository.

## Repository Purpose

This repository is a firmware distribution repository for FTW Robotics Hopper drone and controller DFU packages. It does not contain firmware source code. It contains:

- JSON manifests that tell apps which firmware versions are available.
- Versioned Nordic/Zephyr DFU zip artifacts for the Hopper and controller.
- Documentation for safely adding, promoting, and validating firmware releases.

The public GitHub repository is:

`https://github.com/For-the-Win-Robotics/ftw-robotics-firmware`

## Start Here

Read these files before changing anything:

- `README.md`: Original project notes and release flow.
- `docs/repository-overview.md`: Current repo layout and artifact inventory.
- `docs/manifest-schema.md`: Manifest fields, allowed values, and examples.
- `docs/release-process.md`: Safe beta-to-production workflow.
- `docs/validation.md`: Commands to verify JSON and DFU artifacts.

## Important Conventions

- `firmwares.json` is the active main manifest described by the README.
- `firmwares_v2.json` currently exists but contains empty firmware arrays. Do not populate or remove it unless the consuming app behavior is known.
- Hopper firmware artifacts live in `hopper/`.
- Controller firmware artifacts live in `controller/`.
- Manifest `firmwarePath` values are absolute web paths from the repo/site root, such as `/hopper/dfu_application-1.0.36.zip`.
- Keep firmware zip filenames stable after publication. Apps or users may already reference them.
- Do not modify zip contents in place. Add a new versioned zip instead.
- This repository stores release artifacts, so binary zip files are expected.

## Current Artifact Pattern

Current DFU zip files contain:

- `manifest.json`
- `app_update.bin`

The README says the firmware zip should contain `application.bin`, but every current artifact uses `app_update.bin`, and each zip manifest also references `app_update.bin`. Treat the current artifact pattern as authoritative unless the mobile/app updater requirements are confirmed.

## Safe Change Checklist

When adding or promoting firmware:

1. Ask the release intake questions in `docs/release-process.md` before editing files.
2. Tell the user the exact zip filename and directory to use.
3. Add the new zip under `hopper/` or `controller/` only after the user provides or places it.
4. Add or update the matching entry in `firmwares.json`.
5. Keep `version` aligned with the zip filename and the zip's internal `version_MCUBOOT`.
6. Use the user-selected `status` and `priority`; call out risk when `production` or `critical` is selected.
7. Promote to `production` only after testing through the app, unless the user explicitly chooses production for the release.
8. Increase `priority` deliberately. `critical` may force user updates.
9. Run the validation commands in `docs/validation.md`.

## New Release Intake

When the user wants to create a new firmware release, ask for these values before making manifest changes:

- Device: `hopper` or `controller`.
- Version number, such as `1.0.37`.
- Status: `beta` or `production`.
- Priority: `optional`, `recommended`, or `critical`.

After the user answers, tell them exactly what to call the zip file and where to put it:

- Hopper: `hopper/dfu_application-VERSION.zip`
- Controller: `controller/dfu_application-controller-VERSION.zip`

Also tell the user the matching `firmwarePath` that will go in `firmwares.json`:

- Hopper: `/hopper/dfu_application-VERSION.zip`
- Controller: `/controller/dfu_application-controller-VERSION.zip`

## Validation Commands

From the repository root:

```sh
jq . firmwares.json >/dev/null
jq . firmwares_v2.json >/dev/null
for z in controller/*.zip hopper/*.zip; do unzip -t "$z"; done
for z in controller/*.zip hopper/*.zip; do unzip -p "$z" manifest.json | jq . >/dev/null; done
jq -r '.hopperFirmwares[].firmwarePath, .controllerFirmwares[].firmwarePath' firmwares.json \
  | sed 's#^/##' \
  | while read -r path; do test -f "$path" || echo "Missing artifact: $path"; done
```

No output from the final missing-artifact check means every manifest path exists locally.

## Agent Behavior

- Prefer small, explicit documentation or manifest edits.
- Preserve existing JSON formatting style: two-space indentation and arrays grouped by device.
- Do not reformat binary zip artifacts or regenerate them unless explicitly asked.
- Do not delete old firmware artifacts merely because they are no longer production.
- If a requested change affects app update behavior, call out the risk in the final response.
