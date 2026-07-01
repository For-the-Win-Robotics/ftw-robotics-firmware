# Firmware Release Process

This process is based on the existing `README.md` and current repository structure.

## Add a New Firmware Version

Before editing manifests or adding release metadata, collect the release details from the user.

Ask:

- Which device is this for: `hopper` or `controller`?
- What version number should be released?
- What status should it use: `beta` or `production`?
- What priority should it use: `optional`, `recommended`, or `critical`?

After the user answers, tell them the exact zip filename and destination:

- Hopper: `hopper/dfu_application-VERSION.zip`
- Controller: `controller/dfu_application-controller-VERSION.zip`

Also tell them the manifest `firmwarePath` that will be used:

- Hopper: `/hopper/dfu_application-VERSION.zip`
- Controller: `/controller/dfu_application-controller-VERSION.zip`

Then proceed:

1. Place the new DFU zip in the correct device directory.
   - Hopper: `hopper/dfu_application-VERSION.zip`
   - Controller: `controller/dfu_application-controller-VERSION.zip`
2. Add an entry to `firmwares.json`.
3. Use the status and priority selected by the user.
4. Verify the zip and manifest with `docs/validation.md`.
5. Commit the zip and manifest change together.
6. Test through the app in developer mode using the firmware update screens.

For first exposure, recommend `status: "beta"` and `priority: "optional"` unless the user has a specific reason to do otherwise.

## Promote Beta to Production

After app testing succeeds:

1. Change the entry's `status` from `beta` to `production`.
2. Choose the lowest appropriate priority.
   - Use `optional` when users must manually seek the update.
   - Use `recommended` when users should be prompted.
   - Use `critical` only when forcing the update is intentional.
3. Validate JSON after editing.
4. Commit the manifest change.
5. Test with non-developer app behavior.

## Priority Escalation

Escalate priority in a separate, deliberate change when possible. This creates a clear audit trail:

1. `beta` + `optional`
2. `production` + `optional` or `recommended`
3. `production` + `critical`, only when required

## Rollback Guidance

There is no dedicated rollback mechanism in this repo. Practical rollback options are manifest-only changes:

- Lower a problematic version from `production` to `beta`.
- Lower priority from `critical` to `recommended` or `optional`.
- Promote a known-good older version if the app accepts that update path.

Do not delete zip files as a rollback. Deleting artifacts can break clients that already fetched or cached manifest entries.

## Release Risks

- `critical` updates can force users to update.
- Changing `key` values may make updates unavailable or incompatible.
- Renaming a zip after publication can break existing manifest URLs.
- Editing DFU zip contents without changing the version makes audits and support harder.
