# Reproducible Build Plan

The current build milestone is a traceable LineageOS 23.2 / Android 16 test
image for MP01. It does not claim byte-for-byte reproducibility yet; the first
target is to make every source revision, binary input, build log, and output
checksum available for review.

## Current Build Inputs

- Android source: `https://github.com/LineageOS/android.git`
- Branch: `lineage-23.2`
- Local manifest repo: `https://github.com/MP01-LineageOS/treble_manifest`
- Local manifest branch: `lineage-23.2`
- Support repo: `https://github.com/MP01-LineageOS/MP01-LineageGSI`
- Support branch: `lineage-23.2`
- Active lunch target: `lineage_arm64_bmN4-bp4a-userdebug`
- Output status: unsigned local microG test image

## Current Manifest Inputs

The active manifest pulls:

- `TrebleDroid/device_phh_treble`, revision `android-16.0`
- `MP01-LineageOS/treble_app`, revision `master`
- `MP01-LineageOS/vendor_hardware_overlay`, revision `lineage-23.2`
- `TrebleDroid/vendor_interfaces`, revision `android-16.0`
- `phhusson/vendor_vndk-tests`, revision `master`
- `phhusson/vendor_lptools`, revision `master`
- `phhusson/vendor_magisk`, revision `android-10.0`
- `AndyCGYan/android_packages_apps_QcRilAm`, revision `master`
- VNDK prebuilts v28, v29, and v30
- `MP01-LineageOS/MP01-LineageGSI`, revision `lineage-23.2`
- `lineageos4microg/android_vendor_partner_gms`, fixed revision
  `4b3b48033245800142045ce78038166f8aff6b01`
- `LineageOS/android_hardware_oplus`, revision `lineage-23.2`

`buildmicrog.sh` removes proprietary `vendor/gapps` and the standalone F-Droid
privileged extension from the microG build graph.

## Binary Inputs

Pinned release inputs live in `MP01-LineageGSI/scripts/release-inputs.sh`:

- repo launcher URL and SHA256
- FinQwerty APK URL and SHA256
- F-Droid 1.23.2 APK versioned URL, SHA256, and signing certificate SHA256
- checked-in inkOS v0.1 APK SHA256
- Treble presets commit and `infos.json` SHA256
- Android 15 rollback baseline artifact metadata

Run:

```bash
bash scripts/verify-release-inputs.sh
```

## Build Command

```bash
cd MP01-LineageGSI
bash scripts/verify-release-inputs.sh
bash buildmicrog.sh
```

`build.sh microg` is an equivalent entry point. `buildgms.sh` remains disabled
until proprietary GMS release packaging is ported intentionally.

## Output Requirements

Every successful 23.2 build should produce:

- unsigned `system.img`
- compressed image archive
- SHA256 sums
- resolved `repo manifest -r`
- build-info file with branch, manifest, support repo, target, and output paths
- retained build log

The updated build scripts write SHA256 sums, build info, and the resolved
manifest under the image output directory by default.

## Release Criteria

A 23.2 artifact is only a release candidate after:

1. The source sync/build completes from documented inputs.
2. Output checksums and the resolved source manifest are archived.
3. The image is flashed to MP01 by flashing `system` only.
4. No `userdata`, `metadata`, recovery wipe, or factory reset is performed.
5. The hardware test matrix passes or failures are documented as blocking known
   issues.
6. Signed release and OTA metadata paths are ported and install-tested.

## Rollback Baseline

The current known-good rollback baseline remains release `1755162498`, documented
in [`baseline-source-map-1755162498.md`](baseline-source-map-1755162498.md) and
[`../baselines/working-mp01-2026-05.md`](../baselines/working-mp01-2026-05.md).
