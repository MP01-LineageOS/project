# MP01 LineageOS Action Items

This is the working action list from the June 19, 2026 inventory of the
LineageOS 23.2 update. Keep it ordered by release risk unless new findings
change the priority.

## 1. Close the release reproducibility gap

The current LineageOS 23.2 integration is split across local branch heads that
are not all available from the registered `MP01-LineageOS` repositories yet.
Stage the committed branch heads through the qpublish flow so a clean checkout
can reproduce the active build inputs without depending on local-only state.

Current branch heads:

- `MP01-LineageGSI`: `mp01-23.2-release-gate` at `4e9a6c4`, targeting
  `lineage-23.2`.
- `treble_manifest`: `mp01-23.2-manifest-docs` at `f5c12df`, targeting
  `lineage-23.2`.
- `vendor_hardware_overlay`: `mp01-23.2-overlay-hygiene` at `987a98f`,
  targeting `lineage-23.2`.
- `MP01-OS`: `main` is ahead of `origin/main` with migration docs and this
  tracker.
- `treble_app`: `master` is ahead of `origin/master` with the overlay workflow
  branch-target update.

Definition of done:

- Relevant repos are clean before staging.
- `qstage` submissions exist for each branch head needed to reproduce the
  LineageOS 23.2 build.
- The active build result is recorded against the exact staged commits.
- A fresh sync from the staged branches can resolve every MP01 repo named in
  the manifest.

## 2. Fix local build tooling blockers

Install or provide the required build validation tools in the development qube:
Java/JDK, `openssl`, `keytool`, and `xmlstarlet`. Re-run Gradle, release input
verification, and overlay tests after the tools are available.

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
