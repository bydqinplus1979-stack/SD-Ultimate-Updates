# Stable update channel — release contract

## Fixed endpoints

The application may retrieve metadata only from the two fixed raw-content URLs below (same HTTPS host):

- Manifest: https://raw.githubusercontent.com/bydqinplus1979-stack/SD-Ultimate-Updates/main/channels/stable/manifest.txt
- Detached signature: https://raw.githubusercontent.com/bydqinplus1979-stack/SD-Ultimate-Updates/main/channels/stable/manifest.sig

The signed manifest's APK URL must point to a versioned GitHub Release asset in this repository. The client allowlist must be exact and limited to:
- `raw.githubusercontent.com` for manifest/signature;
- `github.com` for the release asset URL;
- `release-assets.githubusercontent.com` for GitHub's release-asset redirect.

Redirects must be revalidated hop-by-hop by the APK downloader. Metadata requests do not follow redirects. Do not add broad wildcard host rules.

## Canonical manifest format

The manifest is ASCII, LF line endings, fixed field order, one trailing LF, and no extra fields:

```text
format=SDU-UPDATE-MANIFEST-V1
package_id=com.aldarawsheh.launcher
version_code=<strictly higher positive integer>
version_name=<release version>
minimum_supported_version_code=<lowest supported installed client version>
apk_filename=<safe APK filename>
apk_size=<exact APK byte count>
apk_sha256=<lowercase 64-character SHA-256>
certificate_sha256=dc2b9f2b1f9c87cb4f47eb0ac832a628ef501890ce53f21cb6231689a79b422e
download_url=https://github.com/bydqinplus1979-stack/SD-Ultimate-Updates/releases/download/<tag>/<apk_filename>
```

The detached signature is RSA-3072 / SHA256withRSA over the exact bytes of `manifest.txt`. Do not normalize line endings, reformat, or edit a signed manifest after signing.

## Release sequence

1. Build from the reviewed source commit in the protected release environment.
2. Verify package ID, versionCode/versionName, APK signer certificate, APK size, and SHA-256.
3. Create the canonical manifest using those measured values.
4. Sign the exact manifest bytes with the offline manifest-signing private key; never upload that key.
5. Upload the APK as a versioned GitHub Release asset and publish the matching manifest and detached signature.
6. Independently download the public metadata and verify the signature, digest, package, version, and signer before enabling the client channel.
7. Test the complete Android PackageInstaller user-confirmation flow on an isolated emulator and then a controlled device. A system confirmation must be shown; no silent/root fallback.

## Current state — version 1.0.53

- The APK `SD-Ultimate-1.0.53.apk` is published under GitHub Release tag `v1.0.53`.
- The canonical manifest and detached signature are published at the fixed stable-channel endpoints.
- The manifest is 470 bytes; the detached RSA-3072 signature is 384 bytes.
- The manifest SHA-256 is `6250844719902eb1c6c53be7d8837c0c1d11fb154b9e94b88cdb1d55fd1d8438`.
- A separate download of both metadata files from the raw-content endpoints passed local verification with the pinned public key (`Verified OK`).
- The complete in-app updater integration and controlled-device installation cycle are still pending verification. Do not describe the complete updater as production-ready until those gates pass.
