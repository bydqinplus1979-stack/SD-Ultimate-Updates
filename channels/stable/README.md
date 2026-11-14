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

## Release sequence (not yet authorized for production)

1. Build from the reviewed source commit in the protected release environment.
2. Verify package ID, versionCode/versionName, APK signer certificate, APK size, and SHA-256.
3. Create the canonical manifest using those measured values.
4. Sign the exact manifest bytes with the offline manifest-signing private key; never upload that key.
5. Upload the APK as a versioned GitHub Release asset and publish the matching manifest and detached signature.
6. Independently download all three public artifacts and verify the signature, digest, package, version, and signer before enabling the client channel.
7. Test the complete Android PackageInstaller user-confirmation flow on an isolated emulator and then a controlled device. A system confirmation must be shown; no silent/root fallback.

## Current state

This file defines the protocol only. The stable manifest and signature are intentionally absent until a real signed APK release has passed all gates. No production update is currently available.
