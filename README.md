# S D ULTIMATE — Public Update Channel

This repository is a public, read-only distribution origin for signed S D ULTIMATE updates.

## Security model

- The Android application embeds the approved RSA-3072 manifest-verification public key as its trust anchor.
- Pinned public-key SHA-256 (DER SubjectPublicKeyInfo): `9aa669bc09682ffc78f6fa5b6e97ac81349182de7a0f5e29cdb0d6d77ddf70a2`.
- The application verifies the detached RSA/SHA-256 signature over the exact canonical manifest bytes **before** trusting any manifest field.
- The public key in `trust/` is published for independent audit only. The app must never download a replacement trust key from this repository.
- The APK must also match the signed package ID, version, exact byte size, SHA-256 digest, and the existing production APK signer certificate.
- Installation must be initiated through Android PackageInstaller and require the device owner's explicit system confirmation. No root/silent-install fallback is permitted.

## Stable channel layout

- `channels/stable/manifest.txt` — canonical ASCII manifest (not published until signed).
- `channels/stable/manifest.sig` — detached RSA/SHA-256 signature over the exact bytes of `manifest.txt`.
- APK — attached to a versioned GitHub Release; its URL is carried inside the signed manifest.

The manifest format and release checklist are documented in `channels/stable/README.md`.

## Important status

The channel is being provisioned. No production manifest, signature, or APK is published yet. Do not treat this repository as an available update until a complete signed release has been independently verified and the application integration and installation tests have passed.

Never upload a private key, keystore, GitHub token, or other secret here.
