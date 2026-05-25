# Current State

This captures the state inherited from `MP01Experiments` before starting active
maintenance under `MP01-LineageOS`.

## Accepted Hardware Baseline

An MP01 phone has already been flashed with the original developer release
`1755162498` and verified by the maintainer as the known-good baseline. The
install followed the original
[Minimal Phone MP01 Unlock & Flashing Guide](https://chardidath.ing/posts/mp01-flashing-guide/).
The phone boots, the baseline image is accepted, and SIM/radio behavior has
been tested successfully on that image.

The baseline record lives in
[`../baselines/working-mp01-2026-05.md`](../baselines/working-mp01-2026-05.md).

## Primary Integration Repo

`MP01-LineageGSI` is the current build and release integration repo.

Current build target family:

- Android base: LineageOS 22.2 / Android 15.
- Vanilla product: `treble_arm64_bvN`.
- GMS product: `treble_arm64_bgN`.
- Product device: `tdgsi_arm64_ab`.
- Product brand/model: `Minimal MP01`.
- Filesystem flavor: EXT4.

Bundled MP01 packages:

- `inkos`
- `finqwerty`
- `F-DroidPrivilegedExtension`

## Device Defaults Work

Issue `#10` tracks inherited manual setup steps that should become build or
first-boot defaults. The first pass converted the lowest-risk defaults and
documented ownership in [`device-defaults.md`](device-defaults.md).

Completed in this pass:

- MP01 keyboard files are treated as the system default; FinQwerty manual
  selection is no longer documented as required when those files are present.
- SetupWizard targets the real inkOS home component.
- `MP01AccessibilityService` seeds light mode before setup completes.
- PHH preset downloads use a pinned `MP01-LineageOS/treble_presets` source.

Still deferred:

- IMS behavior beyond the existing preset and overlay inventory.
- E-ink per-app refresh tuning.

## Resolved Release Hygiene

The first release-hygiene pass landed on May 25, 2026:

- `MP01-LineageGSI` commit `e070c60` moves active build, support, OTA, and
  release URLs to `MP01-LineageOS`.
- `treble_manifest` commit `bb7584f` moves `vendor_hardware_overlay` to the
  `MP01-LineageOS` fork.
- FinQwerty, F-Droid, and the repo launcher are pinned by URL and SHA256 in
  `MP01-LineageGSI/scripts/release-inputs.sh`.
- F-Droid APK content and signing certificate verification now fail closed.
- The GMS product makefile no longer sets `WITH_ADB_INSECURE := true`.
- Baseline release `1755162498` and FinQwerty release `76cef2d` are mirrored
  into `MP01-LineageOS` releases.

## Remaining Maintenance Risks

- The release path assumes private signing materials exist in `~/.android-certs`.
- The exact installed working image has only a reconstructed source map because
  the inherited build used moving branches and latest-release downloads.
- Future reproducible builds still need full source revision locking for
  Android, TrebleDroid, and MP01 patches.

## Fork Layout

Local checkout remotes should be:

- `origin`: active `MP01-LineageOS` fork.
- `mp01experiments`: previous inactive `MP01Experiments` fork.
- `upstream`: original upstream project where one exists.

## Next Decision

The next code work should be low-risk defaults that remove manual setup steps,
plus an upstream update policy for Android, TrebleDroid, Fossify, and MP01
patches.
