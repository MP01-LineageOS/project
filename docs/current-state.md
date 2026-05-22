# Current State

This captures the state inherited from `MP01Experiments` before starting active
maintenance under `MP01-LineageOS`.

## Verified Hardware Baseline

An MP01 phone has already been flashed with the existing OS and verified to
boot and work at a basic level. The exact image, source revisions, flashing
steps, carrier, and post-install settings still need to be recorded in
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

## Known Manual Workarounds

These are documented in the inherited `MP01-LineageGSI` README and should be
converted into build or first-boot defaults:

1. Apply PHH device presets manually from Settings.
2. Create IMS APN and install the MediaTek IMS APK manually from PHH settings.
3. Select `QWERTY English Layout for Minimal Phone MP01` in FinQwerty.
4. Set inkOS as the default launcher manually.
5. Switch from dark theme to light theme for e-ink readability.
6. Tune or disable automatic e-ink per-app refresh behavior when it misbehaves.

## Immediate Maintenance Risks

- Build scripts still clone from `MP01Experiments` instead of
  `MP01-LineageOS`.
- `treble_arm64_bvN.mk` still points OTA metadata at the old
  `MP01Experiments` raw GitHub URL.
- `treble_arm64_bgN.mk` contains `WITH_ADB_INSECURE := true`.
- Release scripts download the latest FinQwerty release dynamically instead of
  pinning an exact version and checksum.
- F-Droid APK verification currently logs a mismatch but does not fail closed.
- The release path assumes private signing materials exist in `~/.android-certs`.
- The exact installed working image has not been mapped back to source
  revisions yet.

## Fork Layout

Local checkout remotes should be:

- `origin`: active `MP01-LineageOS` fork.
- `mp01experiments`: previous inactive `MP01Experiments` fork.
- `upstream`: original upstream project where one exists.

## Next Decision

Do not change Android behavior until the working phone baseline is captured.
The first code change should be a narrow release hygiene pass in
`MP01-LineageGSI`, after the current working image and source inputs are known.
