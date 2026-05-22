# Working MP01 Baseline - May 2026

Status: incomplete. Fill this from the already-working phone before changing
OS behavior.

## Summary

- Device: Minimal Phone MP01
- Installed OS: unknown exact image
- Android base: likely LineageOS 22.2 / Android 15
- Build variant: unknown
- Install verified by: user report that the OS is installed and working on MP01
- Install date:
- Current maintainer:

## Image Details

- Image filename:
- Image URL:
- Image SHA256:
- Release/tag:
- Build date shown on device:
- Security patch level:
- GMS or vanilla:
- Signed or unsigned:

## Source Mapping

- `MP01-LineageGSI` commit:
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
`chardidathing/MP01-LineageGSI` releases page, extract the `.tar.gz`, flash the
resulting `.img` from `fastbootd`, then erase `userdata` and `metadata`.
The exact release asset used for this phone is still unknown.

## Hardware Notes

- Screen firmware version:
- Carrier: not tested; no SIM installed yet
- SIM type: none installed yet
- Region/country:
- Bootloader state:
- Storage size:

## Manual Workarounds Applied

- PHH presets:
- IMS APN:
- IMS APK:
- FinQwerty layout:
- Default launcher:
- Light theme:
- E-ink refresh setting:

## Test Results

| Area | Status | Notes |
| --- | --- | --- |
| Boot/setup | Unknown |  |
| Wi-Fi | Unknown |  |
| Bluetooth | Unknown |  |
| Mobile data | Not tested | No SIM installed yet |
| Calls | Not tested | No SIM installed yet |
| SMS | Not tested | No SIM installed yet |
| MMS | Not tested | No SIM installed yet |
| VoLTE/IMS | Not tested | No SIM installed yet |
| Physical keyboard | Unknown |  |
| E-ink refresh | Unknown |  |
| Suspend/resume | Unknown |  |
| Charging | Unknown |  |
| OTA | Unknown |  |

## Raw Evidence

Keep raw bugreports private unless redacted.

- ADB snapshot folder:
- Bugreport filename:
- Photos/screenshots:
- Notes:
