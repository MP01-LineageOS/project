# MP01 Device Defaults

This tracks issue `#10`: turning inherited manual setup workarounds into
defaults without changing radio-sensitive behavior before device testing.

## Ownership Matrix

| Default | Owning repo | Current state |
| --- | --- | --- |
| Physical keyboard | `MP01-LineageGSI` | `vendor/MP01_services` installs `aw9523b-key.idc`, `.kl`, and `.kcm` into `system/usr`. The KCM matches FinQwerty's MP01 layout, so manual FinQwerty selection is not expected when system files are present. |
| Default launcher | `MP01-LineageGSI` | SetupWizard assigns `app.inkos` as home and launches `app.inkos/com.github.gezimos.inkos.MainActivity` after setup. |
| Light theme | `MP01-LineageGSI` | `MP01AccessibilityService` seeds `ui_night_mode=1` before setup completes. It records `mp01_defaults_version=1` and does not overwrite configured phones unless `persist.mp01.defaults.force=1`. |
| PHH presets | `treble_app`, `treble_presets`, `MP01-LineageGSI` | MP01 product makefiles point `ro.system.treble.presets` at a pinned `MP01-LineageOS/treble_presets` commit. The app fallback URL also points at `MP01-LineageOS`. |
| IMS setup | `treble_presets`, `vendor_hardware_overlay`, `treble_app` | Inventoried only. Existing SIM/radio behavior is accepted from baseline testing; further IMS defaults need flashed-image testing. |
| E-ink refresh tuning | `MP01-LineageGSI` | Still manual. Current service defaults and README guidance do not agree strongly enough to change without hardware testing. |

## Issue 10 Progress

- Implemented the lowest-risk defaults first: keyboard ownership documentation,
  inkOS home component cleanup, light theme seeding, and pinned PHH preset
  source.
- Deferred IMS and e-ink behavior changes because they can affect telephony or
  user-visible display behavior and need a flashed test image.
- Do not close issue `#10` until a test image passes the device acceptance
  checklist.

## Device Acceptance Checklist

- Fresh setup boots into light mode; `settings get secure ui_night_mode`
  returns `1`.
- `settings get secure mp01_defaults_version` returns `1` after first normal
  boot.
- Home resolves to `app.inkos/com.github.gezimos.inkos.MainActivity`.
- `dumpsys input` shows `aw9523b-key.idc`, `aw9523b-key.kl`, and
  `aw9523b-key.kcm`.
- MP01 accessibility service remains enabled.
- Basic typing works, including MP01 symbol and alt mappings.
- PHH preset source property points at the pinned `MP01-LineageOS` raw URL.
- SIM/radio behavior has no regression when a SIM is available.
