# S D ULTIMATE — Public Update Channel

This repository is the public distribution origin for signed S D ULTIMATE updates.

## Security model

- The Android application embeds the approved RSA-3072 manifest-verification public key as its trust anchor.
- Pinned public-key SHA-256 (DER SubjectPublicKeyInfo): `9aa669bc09682ffc78f6fa5b6e97ac81349182de7a0f5e29cdb0d6d77ddf70a2`.
- The application verifies the detached RSA/SHA-256 signature over the exact canonical manifest bytes **before** trusting any manifest field.
- The public key in `trust/` is published for independent audit only. The app must never download a replacement trust key from this repository.
- The APK must also match the signed package ID, version, exact byte size, SHA-256 digest, and the existing production APK signer certificate.
- Installation must be initiated through Android PackageInstaller and require the device owner's explicit system confirmation. No root/silent-install fallback is permitted.

## Stable channel layout

- `channels/stable/manifest.txt` — canonical ASCII manifest.
- `channels/stable/manifest.sig` — detached RSA/SHA-256 signature over the exact bytes of `manifest.txt`.
- APK — attached to a versioned GitHub Release; its URL is carried inside the signed manifest.

The manifest format and release checklist are documented in `channels/stable/README.md`.

## Current status — 2026-11-15

- Version 1.0.53 APK is published as the `v1.0.53` GitHub Release asset.
- The stable manifest and detached signature for version 1.0.53 are published.
- The manifest signature was independently verified on a separate Termux download from the fixed raw-content URLs using the approved public key (`Verified OK`); the downloaded manifest SHA-256 matched the locally signed canonical file.
- The APK's published size and SHA-256 were checked against the release candidate, and its signer certificate was checked in the build/release validation.
- The complete in-app updater integration and controlled-device installation cycle remain separate release gates. The app must continue to require Android's explicit installation confirmation; do not claim end-to-end production readiness until those gates pass.

Never upload a private key, keystore, GitHub token, or other secret here.
