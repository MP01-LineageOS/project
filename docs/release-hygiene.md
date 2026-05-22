# Release Hygiene

Release hygiene comes before feature work. The MP01 image affects a real phone,
telephony, OTA behavior, and user data, so releases need to be boring,
traceable, and reversible.

## Blocking Issues Before Public Release

- Remove `WITH_ADB_INSECURE := true` from release builds.
- Move all `MP01Experiments` build, OTA, and release URLs to `MP01-LineageOS`.
- Pin every downloaded artifact by version and SHA256.
- Make APK signature or checksum verification fail closed.
- Ensure private signing keys are never committed, printed, uploaded, or
  required from a repo checkout.
- Document whether a release is `user`, `userdebug`, vanilla, or GMS.
- Separate dev/test artifacts from signed release artifacts.

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
```

Add `dev`, `userdebug`, or `test` to non-release builds.

## Immediate Follow-Up PRs

1. Update org URLs in build scripts, product makefiles, README, and OTA JSON.
2. Remove insecure GMS release defaults.
3. Pin FinQwerty and F-Droid downloads.
4. Convert release script into explicit build/sign/publish stages.
5. Add source revision capture to release output.
