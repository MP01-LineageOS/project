# Reproducible Build Plan

The first build milestone is not a new feature. It is a reproducible build that
can regenerate the currently working Android 15 image.

## Current Build Inputs

From the inherited scripts:

- Android source: `https://github.com/LineageOS/android.git`
- Branch: `lineage-22.2`
- Local manifest branch: `15-los-qpr2`
- Current manifest repo should be `https://github.com/MP01-LineageOS/treble_manifest`
- Current support repo should be `https://github.com/MP01-LineageOS/MP01-LineageGSI`
- Treble target generation: `device/phh/treble/generate.sh lineage`
- Vanilla lunch target: `treble_arm64_bvN-bp1a-userdebug`
- GMS lunch target: `treble_arm64_bgN-bp1a-user`

## Current Manifest Inputs

The current `treble_manifest` pulls:

- `TrebleDroid/device_phh_treble`, revision `android-15.0`
- `MP01Experiments/vendor_hardware_overlay`, revision `pie`
- `TrebleDroid/vendor_interfaces`, revision `android-15.0`
- `phhusson/vendor_vndk-tests`, revision `master`
- `phhusson/vendor_lptools`, revision `master`
- `phhusson/vendor_magisk`, revision `android-10.0`
- `AndyCGYan/android_packages_apps_QcRilAm`, revision `master`
- `naz664/prebuilts_vndk_v28`, revision `master`
- `platform/prebuilts/vndk/v29`, fixed revision `bef5d37dda9360940964f097d612c8032e140961`
- `ponces/treble_adapter`, revision `master`
- `MisterZtr/vendor_gapps`, revision `vic`
- `Evolution-X/packages_apps_FaceUnlock`, revision `vic`
- F-Droid privileged extension tag `0.2.13`

The MP01 fork should replace the `vendor_hardware_overlay` owner with
`MP01-LineageOS` before publishing new builds.

## Reproducibility Requirements

Before a release is considered reproducible:

- All project remotes point at `MP01-LineageOS`.
- Every floating branch used by the manifest is recorded as a commit SHA.
- Downloaded APKs are versioned and checksum-verified.
- The output image SHA256 is recorded.
- The build log is archived.
- The exact signing key identity is recorded without exposing private keys.
- The release notes list every source repo and commit used.

## First Build Target

Start with the vanilla target:

```bash
lunch treble_arm64_bvN-bp1a-userdebug
make target-files-package otatools -j$(nproc --all)
```

Defer GMS release work until the vanilla build is reproducible and the
`WITH_ADB_INSECURE` issue in the GMS product is resolved.

## Build Script Cleanup Before Use

The inherited scripts should be updated before becoming official release
scripts:

- Replace `MP01Experiments` URLs with `MP01-LineageOS`.
- Remove destructive assumptions or make them explicit behind a flag.
- Split sync, patch, build, sign, package, and publish into separate steps.
- Pin FinQwerty and F-Droid APK downloads.
- Fail closed on APK verification mismatch.
- Keep release publishing separate from local build generation.
- Avoid writing OTA metadata until artifacts and checksums are final.

## Done Criteria

The first reproducible milestone is complete when a maintainer can:

1. Sync source from documented revisions.
2. Build the image.
3. Produce the same image checksum from the same inputs.
4. Flash the image to MP01.
5. Confirm it matches the known working baseline checklist.
