# LineageOS 23.2 Migration

This records the first source-prep pass for moving MP01 from LineageOS 22.2 /
Android 15 to LineageOS 23.2 / Android 16.

## Prepared Branches

| Repo | Branch | Local commit | Purpose |
| --- | --- | --- | --- |
| `MP01-LineageGSI` | `lineage-23.2` | `4ace87727bf7a6d19ae37b4bacf6c4b6e0e4a51c` | Android 16 GSI wrapper, patch set, and MP01 microG target. |
| `treble_manifest` | `lineage-23.2` | `19c2a81830fc43dcbe10a7f24293db4d51d42f7e` | Android 16 local manifest with MP01-owned support repos. |
| `vendor_hardware_overlay` | `lineage-23.2` | `987a98f2dd5c49bb1663e06a4ddf224f1e22feb8` | Android 16 overlay base plus MP01 runtime overlay. |

These branches are local in the development qube until qpublish/qadmin has
branch registry entries for them. Do not push from this qube.

## Upstream Bases

| Input | Revision used for the migration pass |
| --- | --- |
| `LineageOS/android` | `lineage-23.2` at `02b3ba6a1790153e4d07dfae4b0226a4ef5ac97c` |
| `MisterZtr/LineageOS_gsi` | `lineage-23.2` at `7bf212f449d1aa253ebc97b1ab3c4924be88effa` |
| `MisterZtr/treble_manifest` | `lineage-23.2` at `565a797840c5aace0a7c6e3d52773af80fee2802` |
| `MisterZtr/vendor_hardware_overlay` | `main` at `95799cd35f6c829bfc7e1713aaeb48d70a8a47f8` |
| `TrebleDroid/device_phh_treble` | `android-16.0` at `39ba82ef89ff5bf6b22ca8f147dbf317bffa20ed` |

## Current Build Path

The active Android 16 target is the unsigned local microG image:

```bash
cd MP01-LineageGSI
bash scripts/verify-release-inputs.sh
bash buildmicrog.sh
```

`buildmicrog.sh` uses `.android-build/los23.2-microg` by default and lunches
`lineage_arm64_bmN4-bp4a-userdebug`. `build.sh microg` is the generic entry
point for the same unsigned local test image. `buildgms.sh` stays disabled
until proprietary GMS release packaging is ported intentionally.

## Completed Checks

- Shell syntax check passed for the build scripts, patch runner, and release
  input scripts.
- XML parse check passed for the Android 16 manifest and MP01 overlay XML.
- Non-patch `git diff --check` passed. Imported upstream `.patch` payloads keep
  their upstream whitespace.
- `scripts/verify-release-inputs.sh` passed when run with the Android prebuilt
  JDK `keytool` on `PATH`.

## Remaining Before Flashing

- Run a fresh `buildmicrog.sh` sync/build from the `lineage-23.2` branches.
- Confirm the 23.2 patch stack applies without ignored patch failures.
- Archive `repo manifest -r`, image SHA256, and build log. New 23.2 build
  scripts write SHA256 sums, build info, and the resolved manifest under the
  image output directory on successful builds.
- Flash `system` only. Do not wipe `userdata` or `metadata`.
- Run the MP01 smoke test from `hardware-test-matrix.md`, with extra focus on
  Verizon IMS/VoLTE, keyboard input, e-ink refresh, boot brightness, and lock
  screen clear.
