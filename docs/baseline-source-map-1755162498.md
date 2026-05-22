# Baseline Source Map - 1755162498

This records the best available source map for the known-good baseline release:

- Release: `1755162498`
- Release page: https://github.com/MP01Experiments/MP01-LineageGSI/releases/tag/1755162498
- Release asset: `MP01-Lineage-1755162498-signed.tar.gz`
- Asset SHA256:
  `d6b3f74d30ca84a186b926027afa7340a15450c5fd05720919cde57e1a887b1f`
- Published: 2025-08-14T10:44:07Z
- Support repo tag commit:
  `99bd410eb7e620998db4be5246b23f36f531d4fe`

## Important Caveat

The inherited build script used moving branches and "latest release" downloads.
That means this is not a complete, cryptographic source lock. These commits are
the branch heads at or before the GitHub release publish time, which is the best
available reconstruction without an archived `repo manifest -r` output from the
original build.

The original signed tarball also cannot be reproduced byte-for-byte without the
original private signing keys.

## Reconstructed Inputs

| Input | Ref used by inherited build | Reconstructed commit or digest |
| --- | --- | --- |
| `LineageOS/android` | `lineage-22.2` | `e64bf2e7417450cb61c27a6a8ab45b569bbe7355` |
| `MP01Experiments/treble_manifest` | `15-los-qpr2` | `14b70f55973219b9fb752b4759d178b022906c4e` |
| `TrebleDroid/device_phh_treble` | `android-15.0` | `117638105138feabe2c10c0bf5fccc078dc6b4e5` |
| `MP01Experiments/vendor_hardware_overlay` | `pie` | `12732841e0f1f9764244f10e8d274e2fa761a069` |
| `phhusson/vendor_vndk-tests` | `master` | `533390a1d6bc98d86de6b9aab56825c1f03fddcb` |
| `TrebleDroid/vendor_interfaces` | `android-15.0` | `2d4ede0aee64ae34d3b6ec0815e69928aff055b9` |
| `phhusson/vendor_lptools` | `master` | `c8be7de57b80eab61a6f94ec86464a01fb9056f2` |
| `phhusson/vendor_magisk` | `android-10.0` | `d8056f8032a0f60f365ddfe5e9fccd7eaf3a655d` |
| `AndyCGYan/android_packages_apps_QcRilAm` | `master` | `dc599b67cc1e7e9a62ca15eef605e3b2546d0d42` |
| `naz664/prebuilts_vndk_v28` | `master` | `507526a5b27a170aea338ff747c7d6ecc9bb91bb` |
| `platform/prebuilts/vndk/v29` | fixed revision | `bef5d37dda9360940964f097d612c8032e140961` |
| `ponces/treble_adapter` | `master` | `d8d0eece896bd2b3957bdee893c5f46aa92b211f` |
| `MisterZtr/vendor_gapps` | `vic` | `bad4f18dd566cb6299f40e0f09bbf37a5e5100ac` |
| `Evolution-X/packages_apps_FaceUnlock` | `vic` | `24df19177f0dda70274a1602ecf82dee03c4d44c` |
| F-Droid privileged extension | `refs/tags/0.2.13` | fixed tag |
| `MP01Experiments/finqwerty` | latest release | `76cef2d`; asset SHA256 `d5fedb270671d13fa02177c53191bab6d76f99de133bac4c48efec65ed8d683e` |

## Local Artifacts

- Detached support worktree:
  `/Users/j/Code/MP01/.worktrees/reproduce-1755162498`
- Local known-good release archive:
  `/Users/j/Code/MP01/mp01-baseline/release-assets/MP01-Lineage-1755162498-signed.tar.gz`
- Local source-map TSV:
  `/Users/j/Code/MP01/mp01-baseline/source-map-1755162498.tsv`
