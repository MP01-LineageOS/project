# Release Hygiene

Release hygiene comes before feature work. The MP01 image affects a real phone,
telephony, OTA behavior, and user data, so releases need to be boring,
traceable, and reversible.

## Completed Hygiene

- `MP01-LineageGSI` commit `e070c60` removes the GMS insecure ADB product
  default, moves active URLs to `MP01-LineageOS`, and pins downloaded release
  inputs.
- `treble_manifest` commit `bb7584f` moves the MP01 hardware overlay manifest
  dependency to `MP01-LineageOS/vendor_hardware_overlay`.
- FinQwerty release `76cef2d` and baseline OS release `1755162498` are mirrored
  into `MP01-LineageOS` GitHub releases with matching SHA256 digests.
- The LineageOS 23.2 build path keeps signed release, OTA metadata, and
  proprietary GMS packaging disabled until an unsigned microG image is built
  and install-tested.
- The 23.2 build path records SHA256 sums, build info, and a resolved source
  manifest on successful local test builds.

## Remaining Issues Before Public Release

- Ensure private signing keys are never committed, printed, uploaded, or
  required from a repo checkout.
- Document whether a release is `user`, `userdebug`, vanilla, microG, or GMS.
- Separate dev/test artifacts from signed release artifacts.
- Port signed release and OTA packaging after the 23.2 system image has passed
  hardware smoke testing.

## Release Artifact Checklist

Every release should include:

- Image file.
- OTA package if supported.
- SHA256 checksums.
- Source revision manifest.
- Build log or summarized build environment.
- Install and rollback notes.
- Known issues.
- Manual setup steps that still remain.
- Carrier/telephony test status.

## Signing Policy

- Private keys live outside git.
- Release keys are generated and backed up intentionally.
- Debug or test keys are labeled as such.
- Key identity or public cert fingerprint can be documented.
- Private key paths can be documented, but private key material must not be.

## OTA Policy

OTA metadata should only be updated after:

1. The release artifact is uploaded.
2. Checksums are generated.
3. The download URL is verified.
4. The release is install-tested or explicitly marked as untested.

Do not publish OTA metadata for experimental builds unless users are expected
to receive them.

## Versioning

Use date-based versions until the project has stable release cadence:

```text
MP01-LineageOS-YYYY.MM.DD-android15-vanilla
MP01-LineageOS-YYYY.MM.DD-android15-gms
MP01-LineageOS-YYYY.MM.DD-android16-microg
```

Add `dev`, `userdebug`, or `test` to non-release builds.

## Immediate Follow-Up Work

1. Complete the 23.2 microG build from current branches.
2. Flash `system` only and run the MP01 hardware smoke test.
3. Port signed release packaging as a separate stage after the test image
   boots.
4. Keep OTA metadata disabled until artifact URLs, checksums, and install
   behavior are verified.
