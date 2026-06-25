# Upstream Update Policy

This records issue `#11`: upstream changes should be deliberate, testable, and
reversible. The MP01 image affects telephony, OTA behavior, keyboard input, and
e-ink readability, so bulk merges are not accepted without a release-candidate
test pass on hardware.

## Branch Policy

The active migration line is Android 16 / LineageOS 23.2. Keep Android 15 /
LineageOS 22.2 as the rollback baseline and do not mix it with 23.2 release
candidates. Newer Android, LineageOS, or TrebleDroid migration work should stay
on separate branches until the 23.2 image has a reproducible build and a
passing MP01 smoke test.

Do not mix these in one release candidate:

- Android base updates.
- TrebleDroid component updates.
- Fossify app updates.
- MP01 behavior changes.
- Signing, OTA, or release-script changes.

Each update should have a source revision note, expected user-visible effect,
rollback path, and hardware test result before release.

## Update Classes

| Component | Security sensitivity | Update cadence | Merge rule |
| --- | --- | --- | --- |
| LineageOS Android base | High | Track Android security releases, but stage them as explicit release candidates. | Update only with full image build, flash, and hardware smoke test. |
| `MP01-LineageGSI` patches and product files | High | Update when fixing MP01 defaults, build reproducibility, release hygiene, or security-sensitive packaging. | Keep changes small; require script checks and flashed-image validation for behavior changes. |
| `treble_manifest` | High | Update when pinning source revisions, moving maintained forks, or intentionally changing Android/Treble inputs. | Record every changed project revision; avoid floating-branch changes in releases. |
| `device_phh_treble` | High | Conservative, coordinated with `vendor_hardware_overlay`, `treble_app`, and `treble_presets`. | No blind merge. Build and flash before release because boot, sepolicy, and telephony can regress. |
| `vendor_hardware_overlay` | High | Conservative, MP01 overlay changes only when tied to a testable device outcome. | Require MP01 hardware smoke test for IMS, display, navigation, launcher, or e-ink-affecting overlays. |
| `treble_app` | Medium-high | Update for preset handling, IMS controls, and MP01 source URL fixes. | Static build checks are enough for URL-only changes; flashed test required for behavior changes. |
| `treble_presets` | Medium-high | Update MP01 preset only when a setting is understood and testable. | IMS and radio preferences require SIM/radio regression testing. |
| `finqwerty` | Medium | Update when MP01 keymap changes or an upstream fix is needed. | Compare key maps and test physical typing on hardware before release. |
| `Phone` and `Messages` | Medium | Track for possible future MP01 image integration. | Not active 23.2 image inputs. Do not bulk-merge or add to the image without MP01 UI/e-ink review, default-app testing, calls, SMS, and MMS coverage. |
| Downloaded APK inputs | Medium-high | Pin by URL, version, SHA256, and signing certificate where available. | Verification failures must fail closed. Update only with a release-input commit. |

## Minimum Smoke Test

Before accepting an upstream update into a release candidate:

- Build the intended variant from recorded source revisions.
- Flash the image to MP01 or explicitly mark the artifact unflashed and
  non-release.
- Confirm setup completes and home resolves to inkOS.
- Confirm light mode and `mp01_defaults_version` are set on a fresh setup.
- Confirm physical keyboard input, including symbol/alt mappings.
- Confirm TrebleDroid Settings opens and the Minimal Phone MP01 preset is
  available.
- Confirm mobile data, incoming call, outgoing call, SMS, and IMS/VoLTE status
  when a SIM is available.
- Confirm the inherited dialer, inherited messaging app, F-Droid, FinQwerty,
  and inkOS open. If the Fossify forks are later integrated, test those exact
  packages as replacements.
- Confirm e-ink refresh button behavior and per-app refresh mode are not worse
  than the accepted baseline.
- Confirm OTA metadata points at the intended `MP01-LineageOS` release location
  or is explicitly disabled for the test artifact.

Use [`hardware-test-matrix.md`](hardware-test-matrix.md) for the full release
candidate checklist.

## Rollback Expectations

Every release candidate should keep enough information to reflash the previous
known-good image:

- Previous release tag, image filename, and SHA256.
- Fastboot flashing notes and whether userdata wipe is required.
- Source revisions for the candidate and the previous known-good build.
- Known signing key identity without private key material.
- User-visible rollback risk, especially data wipe, telephony state, and OTA
  compatibility.

The current rollback baseline is release `1755162498`, documented in
[`../baselines/working-mp01-2026-05.md`](../baselines/working-mp01-2026-05.md).

## Issue 11 Status

This policy defines the initial update categories, minimum smoke test, and
rollback expectations. Keep issue `#11` open until the policy is applied to the
next actual upstream update and the result is recorded in release notes.
