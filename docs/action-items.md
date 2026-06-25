# MP01 LineageOS Action Items

This is the working action list from the June 19, 2026 inventory of the
LineageOS 23.2 update. Keep it ordered by release risk unless new findings
change the priority.

## 2026-06-25 qpublish snapshot

This snapshot records the state after the qpublish upgrade to
`patch-chain-v1`.

Active staged submissions:

- `treble_manifest` creates `lineage-23.2` from `15-los-qpr2` at `f5c12df`
  (`20260624-221327-treble_manifest-f5c12df0f07d`, `patch-chain-v1`).
- `vendor_hardware_overlay` creates `lineage-23.2` from `pie` at `417219f`
  (`20260624-221029-vendor_hardware_overlay-417219f65c23`,
  `patch-chain-v1`).
- `treble_app` targets `master` at `0cb1440`
  (`20260624-221329-treble_app-0cb1440e61c3`, `patch-chain-v1`).
- `MP01-OS` needs a `patch-chain-v1` replacement because the old staged
  `e4ed97f` range contains a merge commit. The replacement is staged from the
  linear `qpublish-main-linear` branch after this tracker update.

Active `needs_changes` submissions:

- `MP01-LineageGSI` at `58f8d27`, targeting `15`. This is superseded by the
  local `lineage-23.2` work at `a3e8352` and should not publish as-is.
- `treble_manifest` at `19c2a81`, targeting `15-los-qpr2`. Do not publish it
  as-is; the later create-target `lineage-23.2` submission is the intended
  shape.

Cleanup already done:

- Cancelled the old `patch-series-v1` staged submissions for `treble_manifest`
  and `treble_app` after temporary patch-chain validation passed.
- Created a new overlay submission after the raised `vendor_hardware_overlay`
  patch limit allowed the 23.2 overlay series.

Known blockers to address next:

- `MP01-LineageGSI -> lineage-23.2` is still not staged. Direct patch-chain
  staging fails on intermediate binary overlay history, and a squashed staging
  branch still fails because
  `patches/trebledroid/platform_prebuilts_vndk_v29/0001-Add-android.hardware.audio.common-util-android.hardw.patch`
  embeds six git binary-patch sections.
- Decide whether to refactor that VNDK prebuilt patch into a qpublish-accepted
  text representation, drop it, or request a qpublish policy exception for
  embedded binary patches.

## 1. Close the release reproducibility gap

The current LineageOS 23.2 integration is split across local branch heads that
are not all available from the registered `MP01-LineageOS` repositories yet.
Stage the committed branch heads through the qpublish flow so a clean checkout
can reproduce the active build inputs without depending on local-only state.

Current branch heads:

- `MP01-LineageGSI`: `mp01-23.2-release-gate` at `a3e8352`, targeting
  `lineage-23.2`.
- `treble_manifest`: `mp01-23.2-manifest-docs` at `f5c12df`, targeting
  `lineage-23.2`.
- `vendor_hardware_overlay`: `mp01-23.2-overlay-hygiene` at `417219f`,
  targeting `lineage-23.2`.
- `MP01-OS`: `qpublish-main-linear` is a linear replacement for the staged
  `main` docs range.
- `treble_app`: `master` is ahead of `origin/master` with the overlay workflow
  branch-target update.

Definition of done:

- Relevant repos are clean before staging.
- `qstage` submissions exist for each branch head needed to reproduce the
  LineageOS 23.2 build.
- The active build result is recorded against the exact staged commits.
- A fresh sync from the staged branches can resolve every MP01 repo named in
  the manifest.

Progress:

- As of 2026-06-25, active `patch-chain-v1` staged submissions exist for
  `treble_manifest -> lineage-23.2`, `vendor_hardware_overlay -> lineage-23.2`,
  and `treble_app -> master`.
- `MP01-OS -> main` is being replaced with a linear `patch-chain-v1`
  submission.
- An active staged submission is still missing for
  `MP01-LineageGSI -> lineage-23.2`.
- The older default-branch `treble_manifest` and `MP01-LineageGSI`
  submissions are in `needs_changes` and should not be published as-is.
- The old `patch-series-v1` staged submissions for `treble_manifest` and
  `treble_app` were cancelled after patch-chain replacements were created.

Open decision:

- Continue using create-target `lineage-23.2` submissions for the 23.2 branch
  heads. Do not publish the Android 16/LineageOS 23.2 content onto the existing
  Android 15 default branches.

## 2. Fix local build tooling blockers

Install or provide the required build validation tools in the development qube:
Java/JDK, `openssl`, `keytool`, and `xmlstarlet`. Re-run Gradle, release input
verification, and overlay tests after the tools are available.

Progress:

- Installed Fedora packages for OpenJDK 21, `openssl`, and `xmlstarlet` in the
  development qube. This provides `java`, `javac`, `keytool`, `openssl`, and
  `xmlstarlet`.
- Installed Google Android SDK command-line tools under
  `~/.local/share/android-sdk`, accepted the Android SDK license with operator
  approval, and installed `platforms;android-34`, `build-tools;34.0.0`, and
  `platform-tools`.
- Installed checksum-verified Eclipse Temurin JDK 17 under
  `~/.local/share/jdks/temurin-17` for Gradle/AGP 8.2 validation. Fedora's JDK
  21 fails the Android JDK image transform with `ModuleTarget is malformed:
  platformString missing delimiter: android`.
- `bash scripts/verify-release-inputs.sh` now passes in `MP01-LineageGSI`.
- `bash tests/tests.sh` now runs in `vendor_hardware_overlay` and reports real
  overlay failures instead of missing-tool noise:
  - `Minimal/MP01/AndroidManifest.xml` priority 21 conflicts with another
    manifest.
  - `overlay.mk` entries are not sorted.
  - `overlay.mk` is missing the required trailing empty line.
- `JAVA_HOME=~/.local/share/jdks/temurin-17 ANDROID_HOME=~/.local/share/android-sdk
  ANDROID_SDK_ROOT=~/.local/share/android-sdk ./gradlew --no-daemon
  assembleDebug` passes for `MP01_accessibility_service`.
- 2026-06-22 recheck: this shell no longer has `openssl`, `xmlstarlet`,
  `java`, `javac`, or `keytool` on `PATH`, and `rpm -q` reports
  `openssl`, `xmlstarlet`, and `java-21-openjdk-devel` missing. Restore these
  before trusting release input verification, overlay tests, or Gradle
  validation.

## 3. Secure the e-ink daemon boundary

The daemon currently accepts commands over an abstract Unix socket without
checking peer credentials. Add explicit client authorization and a dedicated
SELinux domain so direct socket access cannot bypass the permissioned e-ink
automation API.

## 4. Fix SELinux packaging for MP01 services

The MP01 services makefile only includes the public policy directory, and the
e-ink daemon binary is labeled as `phhsu_exec`. Package the intended policy and
replace borrowed labels with MP01-specific types.

## 5. Fix the accessibility service compile risk

`SystemSettingsManager.kt` has no package declaration while nearby Kotlin code
imports package-local classes. Confirm with a JDK-backed build and fix the
package/import structure.

Progress:

- The JDK-backed Gradle debug build now passes, so the suspected package/import
  issue is not a current compile blocker. Leave this item for source cleanup
  only if the Android/Soong build later disagrees.

## 6. Fix MP01 preset validation

The MP01 preset uses `key_misc_mediatek_ged_kp`, while TrebleApp defines
`key_misc_mediatek_ged_kpi`. Correct the preset and extend tests so unknown
preference keys fail CI.

## 7. Update overlay workflow and hygiene

Move overlay CI from the old `pie` target to the LineageOS 23.2 branch, update
workflow action versions, and fix the local overlay hygiene failures for
`overlay.mk` ordering and trailing newline handling.

## 8. Revisit MP01 overlay assumptions

Refresh Android 15-era comments and unresolved TODOs in the MP01 overlay. Check
IMS, VT, WFC, and recents component behavior against the actual LineageOS 23.2
build.

## 9. Quarantine or port signing

`sign.sh` still assumes release keys under `~/.android-certs`. Either port the
signing path for LineageOS 23.2 or move it out of the active release path until
signed artifacts are intentionally supported again.

## 10. Resolve artifact metadata mismatch

The current `images/` directory contains an unsigned image and tarball but no
matching checksum, build-info, or resolved-manifest metadata. Confirm whether
the active build now emits this metadata and document the exact result.

## 11. Run the device gate

After the active build completes, perform a system-only no-wipe flash and run
the hardware test matrix: boot, e-ink refresh modes, keyboard/backlight,
brightness boot clamp, lockscreen clear, Wi-Fi, Bluetooth, suspend/resume,
calls, SMS, Verizon IMS/VoLTE, F-Droid, microG, and permissioned e-ink
automation.
