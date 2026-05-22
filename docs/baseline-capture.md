# Baseline Capture

Capture this before making OS changes. The goal is to preserve a known-good
fallback point and enough evidence to reproduce the current working image.

Some commands can expose private data. Keep raw logs local unless they have
been reviewed and redacted.

## Device Notes

Record manually:

- Device: Minimal Phone MP01.
- Screen firmware version.
- Installed image filename or release URL.
- Install date.
- Flashing guide and exact commands used.
- Host OS used for flashing.
- Carrier and SIM type.
- Whether GMS build or vanilla build is installed.
- Any first-boot setup choices.
- Current workarounds applied.

Use [`../baselines/working-mp01-2026-05.md`](../baselines/working-mp01-2026-05.md)
as the baseline record.

The current known install was performed using:

```text
https://chardidath.ing/posts/mp01-flashing-guide/
```

That guide uses `fastbootd`, flashes `system.img`, erases `userdata`, erases
`metadata`, and reboots. The exact release asset still needs to be identified.

## Low-Risk ADB Snapshot

Run from the project workspace with the phone connected and USB debugging
enabled. Keep captured artifacts under `/Users/j/Code/MP01/mp01-baseline`;
do not put project artifacts at filesystem root or elsewhere on the host.

```bash
mkdir -p /Users/j/Code/MP01/mp01-baseline
cd /Users/j/Code/MP01/mp01-baseline

adb devices -l | tee adb-devices.txt
adb shell getprop | tee getprop.txt
adb shell uname -a | tee uname.txt
adb shell cat /proc/version | tee proc-version.txt
adb shell df -h | tee df-h.txt
adb shell mount | tee mount.txt
adb shell settings list global | tee settings-global.txt
adb shell settings list secure | tee settings-secure.txt
adb shell settings list system | tee settings-system.txt
adb shell cmd package list packages -f | tee packages.txt
adb shell dumpsys battery | tee dumpsys-battery.txt
adb shell dumpsys display | tee dumpsys-display.txt
adb shell dumpsys power | tee dumpsys-power.txt
adb shell dumpsys input | tee dumpsys-input.txt
adb shell dumpsys telecom | tee dumpsys-telecom.txt
adb shell dumpsys telephony.registry | tee dumpsys-telephony-registry.txt
```

## Optional Raw Bugreport

`bugreportz` is useful but privacy-sensitive. It can contain phone numbers,
network identifiers, accounts, recent app activity, and logs. Do not publish it
unreviewed.

```bash
adb bugreportz
```

After the command finishes, pull the generated zip from the path printed by
`adb`.

## Photos To Capture

Take simple photos of:

- About phone page.
- Build number and Android version.
- PHH Settings -> My device.
- PHH Settings -> IMS features after setup.
- FinQwerty selected physical keyboard layout.
- Default launcher prompt or selected launcher.
- Light/dark theme setting.
- E-ink refresh controls.

## Baseline Tagging

After the exact source revision and image are known, tag the relevant repos:

```bash
git tag baseline-2026-05-working-mp01
git push origin baseline-2026-05-working-mp01
```

Only tag after confirming the tag points at the source revision that produced
the working image. If that cannot be proven, create a documentation-only
baseline instead and label it as "installed image unknown source".
