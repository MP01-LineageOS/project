# Working MP01 Baseline - May 2026

Status: accepted known-good baseline. ADB baseline was captured on May 22,
2026; raw logs are local-only because they contain device identifiers.

## Summary

- Device: Minimal Phone MP01
- Installed OS: LineageOS GSI from the latest original release at install time
- Android base: LineageOS 22.2 / Android 15
- Build variant: `treble_arm64_bvN-userdebug`
- Install verified by: maintainer report and ADB snapshot from the working phone
- Install date:
- Current maintainer:

## Image Details

- Image filename: `MP01-Lineage-1755162498-signed.tar.gz`
- Image URL: https://github.com/MP01-LineageOS/MP01-LineageGSI/releases/download/1755162498/MP01-Lineage-1755162498-signed.tar.gz
- Original source URL: https://github.com/MP01Experiments/MP01-LineageGSI/releases/download/1755162498/MP01-Lineage-1755162498-signed.tar.gz
- Image SHA256: `d6b3f74d30ca84a186b926027afa7340a15450c5fd05720919cde57e1a887b1f`
- Release/tag: `1755162498`; device reports `22.2-20250814-VANILLA-EXT4-GSI`
- Release page: https://github.com/MP01-LineageOS/MP01-LineageGSI/releases/tag/1755162498
- Original release page: https://github.com/MP01Experiments/MP01-LineageGSI/releases/tag/1755162498
- Release published: 2025-08-14T10:44:07Z
- Build date shown on device: Thu Aug 14 08:30:17 UTC 2025
- Security patch level: system 2025-07-01; vendor 2025-11-05
- GMS or vanilla: VANILLA; no `com.google.android.gms` or Play Store package observed
- Signed or unsigned: signed release archive; device reports `release-keys` tags on a `userdebug` build

## Source Mapping

- `MP01-LineageGSI` commit: `99bd410eb7e620998db4be5246b23f36f531d4fe` (`1755162498` tag)
- `treble_manifest` commit:
- `device_phh_treble` commit:
- `vendor_hardware_overlay` commit:
- `treble_app` commit:
- `treble_presets` commit:
- `finqwerty` commit:
- `Phone` commit:
- `Messages` commit:

## Flashing Notes

- Host OS:
- Tools used:
- Unlock state:
- Flashing guide: https://chardidath.ing/posts/mp01-flashing-guide/
- Commands used:
  - `adb reboot bootloader`
  - `fastboot flashing unlock`
  - `fastboot reboot fastboot` or `adb reboot fastboot`
  - `fastboot flash system <path to system.img>`
  - `fastboot erase userdata`
  - `fastboot erase metadata`
  - `fastboot reboot`
- Recovery/rollback notes:

The guide instructs users to download the latest image from the original
`MP01Experiments/MP01-LineageGSI` releases page, extract the `.tar.gz`, flash
the resulting `.img` from `fastbootd`, then erase `userdata` and `metadata`.
The phone was installed from the current latest original release,
`1755162498`.

This records the historical clean-install path for the accepted baseline. For
future upgrades of an in-use MP01, do not erase `userdata`, erase `metadata`,
run `fastboot -w`, or factory reset unless the operator explicitly confirms a
data wipe for that session.

## Hardware Notes

- Screen firmware version:
- Carrier: tested by maintainer; exact carrier not recorded in this public doc
- SIM type: tested by maintainer; exact SIM details not recorded in this public doc
- Region/country: locale `en-US`
- Bootloader state: unlocked; verified boot state `orange`
- Storage size: `/data` reports 228G total, 226G available at capture time
- Kernel: 5.10.233 Android 12 vendor kernel, built Feb 20 2025
- SoC: MediaTek MT6789

## Manual Workarounds Applied

- PHH presets: MP01 overlay enabled (`me.phh.treble.overlay.minimal.mp01`)
- IMS APN:
- IMS APK: MTK IMS telephony overlay enabled
- FinQwerty layout: package installed; selected physical layout still needs manual verification
- Default launcher: `app.inkos/com.github.gezimos.inkos.MainActivity`
- Light theme: enabled (`ui_night_mode=1`)
- E-ink refresh setting:

## Test Results

| Area | Status | Notes |
| --- | --- | --- |
| Boot/setup | Pass | User reported working install; ADB reports setup complete |
| Wi-Fi | Unknown | Wi-Fi was off during ADB capture |
| Bluetooth | Unknown |  |
| Mobile data | Pass | Tested by maintainer on the accepted baseline image |
| Calls | Pass | Tested by maintainer on the accepted baseline image |
| SMS | Pass | Tested by maintainer on the accepted baseline image |
| MMS | Unknown | Not separately recorded |
| VoLTE/IMS | Pass | Radio/SIM behavior reported working on the accepted baseline image |
| Physical keyboard | Unknown | FinQwerty is installed; layout still needs manual verification |
| E-ink refresh | Unknown |  |
| Suspend/resume | Unknown |  |
| Charging | Unknown | Battery service reported charging and 13% level during capture |
| OTA | Unknown | Installed baseline image still points OTA URL at `MP01Experiments/MP01-LineageGSI`; repository OTA metadata now points future builds at `MP01-LineageOS/MP01-LineageGSI` |

## Raw Evidence

Keep raw bugreports private unless redacted.

- ADB snapshot folder: `/Users/j/Code/MP01/mp01-baseline`
- Bugreport filename: not captured
- Photos/screenshots:
- Notes: captured `getprop`, package list, settings, mount/df, input, display,
  power, battery, telecom, telephony registry, overlays, launcher resolution,
  and recent activity state.
