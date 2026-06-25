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

- Android base: LineageOS 23.2 / Android 16.
- Active test product: `lineage_arm64_bmN4`.
- Active lunch target: `lineage_arm64_bmN4-bp4a-userdebug`.
- Product brand/model: `Minimal MP01`.
- Filesystem flavor: EXT4.
- Release status: unsigned local microG test image only; signed release, OTA,
  and proprietary GMS packaging remain disabled until the 23.2 image is built
  and install-tested.

Bundled MP01 packages:

- `inkos`
- `finqwerty`
- `MP01AccessibilityService`
- `MP01_eink_server`
- F-Droid and microG packages from the 23.2 microG build graph

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

- The 23.2 image still needs a completed source build and MP01 smoke test.
- The release path for signed images, OTA metadata, and proprietary GMS is not
  ported to 23.2.
- The exact installed working Android 15 image has only a reconstructed source
  map because the inherited build used moving branches and latest-release
  downloads.
- Future reproducible releases still need full source revision locking for
  Android, TrebleDroid, and MP01 patches.

## Fork Layout

Local checkout remotes should be:

- `origin`: active `MP01-LineageOS` fork.
- `mp01experiments`: previous inactive `MP01Experiments` fork.
- `upstream`: original upstream project where one exists.

## Next Decision

The next gate is a completed 23.2 microG build, source-manifest capture, `system`
flash without wiping user data, and hardware smoke-test results.
