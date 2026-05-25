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
- The exact installed working image has only a reconstructed source map because
  the inherited build used moving branches and latest-release downloads.

## Fork Layout

Local checkout remotes should be:

- `origin`: active `MP01-LineageOS` fork.
- `mp01experiments`: previous inactive `MP01Experiments` fork.
- `upstream`: original upstream project where one exists.

## Next Decision

Do not change Android behavior until the inherited release inputs and fork drift
are documented. The first code change should be a narrow release hygiene pass
in `MP01-LineageGSI`, followed by low-risk defaults that remove manual setup
steps.
