# Hardware Test Matrix

Use this checklist for the known-good baseline and every release candidate.

## Device Identity

- Device boots to setup.
- About phone shows expected Android and LineageOS version.
- Build fingerprint and vendor fingerprint are recorded.
- `ro.product.brand`, `ro.product.model`, and `ro.product.device` look sane.
- Screen firmware version is recorded.

## Setup Flow

- First boot completes without crash loops.
- Default launcher resolves to inkOS.
- Light theme is active and readable on e-ink.
- `settings get secure mp01_defaults_version` returns the expected defaults version.
- PHH Settings opens.
- My Device preset entry appears for Minimal Phone MP01.

## Display And E-Ink

- Text is readable in Settings, launcher, Phone, and Messages.
- Light theme is usable outdoors and indoors.
- E-ink refresh button works.
- Per-app refresh mode can be changed.
- Ghosting is acceptable after normal navigation.
- Lock screen and always-on behavior are acceptable.
- Rotation and resolution are stable.

## Keyboard And Buttons

- Physical keyboard types expected letters.
- Shift, symbol, enter, delete, space, and punctuation work.
- System input files for `aw9523b-key` are active.
- FinQwerty MP01 layout is available as a fallback.
- Hardware refresh button behavior is documented.
- Volume/power buttons work.

## Connectivity

- Wi-Fi connects and survives suspend/resume.
- Bluetooth can pair with a basic accessory.
- Mobile data works.
- Airplane mode toggles cleanly.
- Hotspot behavior is tested or marked untested.

## Telephony

- SIM is detected.
- Incoming call works.
- Outgoing call works.
- Audio routing works for earpiece and speaker.
- SMS send and receive work.
- MMS send and receive are tested or marked untested.
- VoLTE/IMS status is recorded.
- IMS APN creation behavior is recorded.
- Carrier and country are recorded.

## Power

- Charging works while powered on.
- Offline charging behavior is tested.
- Battery percentage updates.
- Suspend/resume works.
- Overnight idle drain is measured.
- Device does not overheat during normal use.

## Apps

- Phone app opens and can be set as default.
- Messages app opens and can be set as default.
- inkOS opens and can be set as default launcher.
- F-Droid opens.
- TrebleDroid Settings opens.
- FinQwerty opens.

## OTA And Recovery

- Current slot/partition state is recorded.
- OTA updater opens.
- OTA metadata URL points at the intended org.
- Update install is tested or marked untested.
- Rollback/reflash path is documented.

## Pass Criteria

A build can be called a release candidate only when all required sections are
pass or explicitly marked as known issue. Telephony and OTA failures should
block public release unless the release notes make the limitation unmistakable.
