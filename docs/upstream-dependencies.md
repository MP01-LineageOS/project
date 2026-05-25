# Upstream Dependencies

This records the current fork and upstream map for `MP01-LineageOS`. Counts are
from local fetches on May 24, 2026.

## Maintained Forks

| Repo | Purpose | Branch | Upstream | Drift |
| --- | --- | --- | --- | --- |
| `MP01-LineageGSI` | Primary build, release, signing, OTA, and MP01 product integration | `15` | `MisterZtr/LineageOS_gsi` | `85` ahead / `51` behind upstream default `lineage-23.2`; identical to `MP01Experiments/15` |
| `treble_manifest` | Local manifest for Treble/Lineage dependency sync | `15-los-qpr2` | `MisterZtr/treble_manifest` | `12` ahead / `6` behind upstream default `lineage-23.2`; identical to `MP01Experiments/15-los-qpr2` |
| `device_phh_treble` | PHH/TrebleDroid generic device tree and GSI targets | `android-16.0` | `TrebleDroid/device_phh_treble` | `0` ahead / `4` behind upstream `android-16.0`; identical to `MP01Experiments/android-16.0` |
| `vendor_hardware_overlay` | Runtime resource overlays for vendor/device quirks, IMS, telephony, Wi-Fi, and Treble app wiring | `pie` | `TrebleDroid/vendor_hardware_overlay` | `9` ahead / `9` behind upstream `pie`; identical to `MP01Experiments/pie` |
| `treble_app` | Privileged TrebleDroid settings app and preset application logic | `master` | `TrebleDroid/treble_app` | `7` ahead / `2` behind upstream `master`; identical to `MP01Experiments/master` |
| `treble_presets` | Device preset database, including Minimal Phone MP01 presets | `master` | `TrebleDroid/treble_presets` | `1` ahead / `4` behind upstream `master`; identical to `MP01Experiments/master` |
| `finqwerty` | Physical keyboard layout app with MP01 keymap support | `master` | `vbbot/finqwerty` | `6` ahead / `0` behind upstream `master`; identical to `MP01Experiments/master` |
| `Phone` | Fossify Phone fork used in the MP01 image | `main` | `FossifyOrg/Phone` | `0` ahead / `244` behind upstream `main`; identical to `MP01Experiments/main` |
| `Messages` | Fossify Messages fork used in the MP01 image | `main` | `FossifyOrg/Messages` | `0` ahead / `255` behind upstream `main`; identical to `MP01Experiments/main` |
| `MP01-OS` | Coordination, roadmap, release notes, and maintainer docs | `main` | none | Active tracker repo |

## Manifest Dependencies

`treble_manifest/manifest.xml` currently pulls these external projects into
the Android tree:

| Path | Project | Revision |
| --- | --- | --- |
| `device/phh/treble` | `TrebleDroid/device_phh_treble` | `android-15.0` |
| `vendor/hardware_overlay` | `MP01Experiments/vendor_hardware_overlay` | `pie` |
| `vendor/vndk-tests` | `phhusson/vendor_vndk-tests` | `master` |
| `vendor/interfaces` | `TrebleDroid/vendor_interfaces` | `android-15.0` |
| `vendor/lptools` | `phhusson/vendor_lptools` | `master` |
| `vendor/magisk` | `phhusson/vendor_magisk` | `android-10.0` |
| `packages/apps/QcRilAm` | `AndyCGYan/android_packages_apps_QcRilAm` | `master` |
| `prebuilts/vndk/v28` | `naz664/prebuilts_vndk_v28` | `master` |
| `prebuilts/vndk/v29` | `platform/prebuilts/vndk/v29` | `bef5d37dda9360940964f097d612c8032e140961` |
| `treble_adapter` | `ponces/treble_adapter` | `master` |
| `vendor/gapps` | `MisterZtr/vendor_gapps` | `vic` |
| `vendor/partner_gms` | `lineageos4microg/android_vendor_partner_gms` | `4b3b48033245800142045ce78038166f8aff6b01` |
| `packages/apps/FaceUnlock` | `Evolution-X/packages_apps_FaceUnlock` | `vic` |
| `vendor/F-DroidPrivilegedExtension` | `privileged-extension.git` from F-Droid GitLab | `refs/tags/0.2.13` |

## Maintenance Notes

- The accepted baseline image is the original developer release
  `1755162498`; do not treat current branch heads as a cryptographic rebuild
  lock for that image.
- `treble_manifest` still references `MP01Experiments/vendor_hardware_overlay`.
  Move this to `MP01-LineageOS/vendor_hardware_overlay` as part of release
  hygiene.
- `Phone` and `Messages` are far behind Fossify upstream. Do not bulk-merge
  them without MP01 UI/e-ink testing.
- TrebleDroid components should be updated conservatively and together, because
  `device_phh_treble`, `vendor_hardware_overlay`, `treble_app`, and
  `treble_presets` interact at build and runtime.
- `MP01-LineageGSI` and `treble_manifest` upstream defaults have moved toward
  newer Lineage/Treble branches. Keep Android 15 maintenance separate from any
  future Android 16/Lineage 23 migration.
