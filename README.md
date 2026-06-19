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
- [`Phone`](https://github.com/MP01-LineageOS/Phone): Fossify Phone fork tracked for possible MP01 image integration.
- [`Messages`](https://github.com/MP01-LineageOS/Messages): Fossify Messages fork tracked for possible MP01 image integration.

## Current Priorities

1. Finish the LineageOS 23.2 / Android 16 microG build and archive the resolved source manifest, build log, and SHA256 sums.
2. Flash only `system` for the first MP01 smoke test; do not wipe userdata or metadata.
3. Keep signed release, OTA metadata, and proprietary GMS packaging disabled until the 23.2 test image boots and is install-tested.
4. Tighten release-input verification for every downloaded or checked-in binary.
5. Validate boot, OTA, keyboard, telephony, SMS/MMS, IMS, suspend/resume, and e-ink refresh behavior on hardware.

## Maintainer Docs

- [Current state](docs/current-state.md)
- [Baseline capture](docs/baseline-capture.md)
- [Upstream dependencies](docs/upstream-dependencies.md)
- [Upstream update policy](docs/upstream-update-policy.md)
- [LineageOS 23.2 migration](docs/lineage-23.2-migration.md)
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
