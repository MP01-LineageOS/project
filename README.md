# MP01 LineageOS

This organization continues the Minimal Phone MP01 LineageOS/Treble GSI work
started in `MP01Experiments`.

## Repositories

- [`MP01-LineageGSI`](https://github.com/MP01-LineageOS/MP01-LineageGSI): primary build, release, patch, signing, and OTA integration repo.
- [`treble_manifest`](https://github.com/MP01-LineageOS/treble_manifest): local manifest for syncing LineageOS/Treble dependencies.
- [`device_phh_treble`](https://github.com/MP01-LineageOS/device_phh_treble): PHH/TrebleDroid device tree and GSI target support.
- [`vendor_hardware_overlay`](https://github.com/MP01-LineageOS/vendor_hardware_overlay): Android runtime resource overlays for device/vendor quirks.
- [`treble_app`](https://github.com/MP01-LineageOS/treble_app): privileged TrebleDroid settings app and preset application logic.
- [`treble_presets`](https://github.com/MP01-LineageOS/treble_presets): device preset database, including the Minimal Phone MP01 entry.
- [`finqwerty`](https://github.com/MP01-LineageOS/finqwerty): physical keyboard layout app with the MP01 keymap.
- [`Phone`](https://github.com/MP01-LineageOS/Phone): Fossify Phone fork for the MP01 image.
- [`Messages`](https://github.com/MP01-LineageOS/Messages): Fossify Messages fork for the MP01 image.

## Current Priorities

1. Reproduce the existing Android 15 / LineageOS 22.2 build from source.
2. Document the exact build inputs, release artifacts, and flashing path.
3. Remove insecure release defaults and pin downloaded release dependencies.
4. Turn known setup workarounds into defaults for keyboard, IMS, launcher, and e-ink readability.
5. Establish a small hardware test matrix for boot, OTA, keyboard, telephony, SMS/MMS, IMS, suspend/resume, and e-ink refresh behavior.

## Maintainer Docs

- [Current state](docs/current-state.md)
- [Baseline capture](docs/baseline-capture.md)
- [Upstream dependencies](docs/upstream-dependencies.md)
- [Reproducible build plan](docs/reproducible-build.md)
- [Release hygiene](docs/release-hygiene.md)
- [Device defaults](docs/device-defaults.md)
- [Hardware test matrix](docs/hardware-test-matrix.md)
- [Working MP01 baseline template](baselines/working-mp01-2026-05.md)

## Remote Layout

Local checkouts should use:

- `origin`: active `MP01-LineageOS` fork.
- `mp01experiments`: previous inactive `MP01Experiments` fork.
- `upstream`: original upstream project, such as TrebleDroid, Fossify, FinQwerty, or MisterZtr.

## Release Policy

Release builds should be reproducible, signed with non-committed private keys,
and published with checksums, source revisions, build notes, and known issues.
Development/userdebug images must be clearly labeled and kept separate from
signed user releases.
