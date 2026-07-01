# Validation

Run these commands from the repository root before committing firmware manifest or artifact changes.

## JSON Syntax

```sh
jq . firmwares.json >/dev/null
jq . firmwares_v2.json >/dev/null
```

## Manifest Paths Exist

```sh
jq -r '.hopperFirmwares[].firmwarePath, .controllerFirmwares[].firmwarePath' firmwares.json \
  | sed 's#^/##' \
  | while read -r path; do
      test -f "$path" || echo "Missing artifact: $path"
    done
```

Expected result: no output.

## Zip Integrity

```sh
for z in controller/*.zip hopper/*.zip; do
  unzip -t "$z"
done
```

Expected result: each archive reports `No errors detected`.

## Zip Manifest JSON

```sh
for z in controller/*.zip hopper/*.zip; do
  unzip -p "$z" manifest.json | jq . >/dev/null || echo "Invalid manifest: $z"
done
```

Expected result: no output.

## Inspect Zip Contents

```sh
for z in controller/*.zip hopper/*.zip; do
  echo "$z"
  unzip -l "$z"
done
```

Current expected contents:

- `app_update.bin`
- `manifest.json`

## Compare Manifest Versions to Zip Manifests

Use this command to print each zip path and its internal MCUBoot version:

```sh
for z in controller/*.zip hopper/*.zip; do
  printf '%s ' "$z"
  unzip -p "$z" manifest.json | jq -r '.files[].version_MCUBOOT'
done
```

The version in `firmwares.json` should match the zip's internal version before the `+0` suffix.

## Current Known Documentation Mismatch

`README.md` says the zip should contain `application.bin`, but the current shipped zip files contain `app_update.bin`. The internal zip manifests also reference `app_update.bin`.

Before changing this artifact naming pattern, confirm the updater implementation and DFU tooling expectations.

