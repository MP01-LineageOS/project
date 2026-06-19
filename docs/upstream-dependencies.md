# Upstream Dependencies

This records the current fork and upstream map for the active LineageOS 23.2 /
Android 16 migration. The original Android 15 release `1755162498` remains the
known-good rollback baseline.

## Maintained Forks

| Repo | Purpose | Active branch | Current local commit | Upstream |
| --- | --- | --- | --- | --- |
| `MP01-LineageGSI` | Primary build, release, patch, OTA, and MP01 product integration | `lineage-23.2` | `4ace87727bf7a6d19ae37b4bacf6c4b6e0e4a51c` | `MisterZtr/LineageOS_gsi` |
| `treble_manifest` | Local manifest for Treble/Lineage dependency sync | `lineage-23.2` | `19c2a81830fc43dcbe10a7f24293db4d51d42f7e` | `MisterZtr/treble_manifest` |
| `device_phh_treble` | PHH/TrebleDroid generic device tree and GSI target support | `android-16.0` | `76d1a8549290bdbc154ed8a7445719016adb7068` | `TrebleDroid/device_phh_treble` |
| `vendor_hardware_overlay` | Runtime resource overlays for vendor/device quirks, IMS, telephony, Wi-Fi, and Treble app wiring | `lineage-23.2` | `987a98f2dd5c49bb1663e06a4ddf224f1e22feb8` | `TrebleDroid/vendor_hardware_overlay` |
| `treble_app` | Privileged TrebleDroid settings app and preset application logic | `master` | `db1556678bcbc8fea08005cf3b26da8d627879c1` | `TrebleDroid/treble_app` |
| `treble_presets` | Device preset database, including Minimal Phone MP01 presets | `master` | `09fdae135930b553c54aba7aa9a07b105132b6ff` | `TrebleDroid/treble_presets` |
| `finqwerty` | Physical keyboard layout app with MP01 keymap support | `master` | `76cef2d7c3577cb24d271e9c8de5fd430b535745` | `vbbot/finqwerty` |
| `Phone` | Fossify Phone fork tracked for possible MP01 image integration | `main` | `aa1bde9909effc5a5b7a3f3d528e15c4377a5b54` | `FossifyOrg/Phone` |
| `Messages` | Fossify Messages fork tracked for possible MP01 image integration | `main` | `9cbf3e46042bfaef02c7b28283c6e5ec50b42ec7` | `FossifyOrg/Messages` |
| `MP01-OS` | Coordination, roadmap, release notes, and maintainer docs | `main` | active docs repo | none |

## Manifest Dependencies

`treble_manifest/manifest.xml` currently pulls these external projects into the
LineageOS 23.2 Android tree:

| Path | Project | Revision |
| --- | --- | --- |
| `device/phh/treble` | `TrebleDroid/device_phh_treble` | `android-16.0` |
| `treble_app` | `MP01-LineageOS/treble_app` | `master` |
| `vendor/hardware_overlay` | `MP01-LineageOS/vendor_hardware_overlay` | `lineage-23.2` |
| `vendor/vndk-tests` | `phhusson/vendor_vndk-tests` | `master` |
| `vendor/interfaces` | `TrebleDroid/vendor_interfaces` | `android-16.0` |
| `vendor/lptools` | `phhusson/vendor_lptools` | `master` |
| `vendor/magisk` | `phhusson/vendor_magisk` | `android-10.0` |
| `packages/apps/QcRilAm` | `AndyCGYan/android_packages_apps_QcRilAm` | `master` |
| `prebuilts/vndk/v28` | `naz664/prebuilts_vndk_v28` | `master` |
| `prebuilts/vndk/v29` | `platform/prebuilts/vndk/v29` | `bef5d37dda9360940964f097d612c8032e140961` |
| `prebuilts/vndk/v30` | `platform/prebuilts/vndk/v30` | `5f9884aa352825291757dfd6694b874ad8c1805e` |
| `LineageOS_gsi` | `MP01-LineageOS/MP01-LineageGSI` | `lineage-23.2` |
| `vendor/gapps` | `MindTheGapps/vendor_gapps` | `baklava` |
| `vendor/partner_gms` | `lineageos4microg/android_vendor_partner_gms` | `4b3b48033245800142045ce78038166f8aff6b01` |
| `hardware/oplus` | `LineageOS/android_hardware_oplus` | `lineage-23.2` |

The current microG build removes proprietary `vendor/gapps` and the standalone
F-Droid privileged extension from the local build graph before syncing/building.

## App Integration Notes

- The active 23.2 image currently packages `inkos`, `finqwerty`,
  `MP01AccessibilityService`, `MP01_eink_server`, and microG/F-Droid packages.
- The `Phone` and `Messages` Fossify forks are not active manifest or product
  inputs for 23.2. The image uses the dialer and messaging packages inherited
  from LineageOS/Treble until those forks are added as explicit, tested build
  inputs.
- Do not bulk-merge Fossify mainline or add these apps to the image without
  MP01 e-ink readability, default-app, calls, SMS, and MMS testing.

## Maintenance Notes

- Keep `vendor/hardware_overlay` and `LineageOS_gsi` on MP01-owned branches
  unless intentionally testing upstream drift.
- TrebleDroid components should be updated conservatively and together, because
  `device_phh_treble`, `vendor_hardware_overlay`, `treble_app`, and
  `treble_presets` interact at build and runtime.
- Release candidates must archive `repo manifest -r`, image checksums, build
  logs, and flashed-device test results before OTA metadata is updated.

See [`upstream-update-policy.md`](upstream-update-policy.md) for the update
cadence, smoke test requirements, and rollback expectations for these
dependencies.
