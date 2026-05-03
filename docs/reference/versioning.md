# Versioning

## OpenCastor

Format: `YYYY.MM.DD.iterationnumber`

Examples:
- `v2026.3.27.1` — March 27, 2026, first release that day
- `v2026.3.20.4` — March 20, 2026, fourth release

This format makes the release date immediately obvious and allows multiple releases per day.

## RCAN Spec

Semantic versioning: `MAJOR.MINOR.PATCH`

- `MAJOR` — breaking protocol changes
- `MINOR` — new message types or scope changes (backward compatible)
- `PATCH` — clarifications, editorial fixes

Pinned protocol version: see the [live compatibility matrix](https://rcan.dev/compatibility).

## rcan-py / rcan-ts

Semantic versioning tracking spec minor versions. The SDK ↔ spec pairing currently in effect is published in the [live compatibility matrix](https://rcan.dev/compatibility); both SDKs target the same RCAN protocol version at any given release.

## opencastor-client

`MAJOR.MINOR.PATCH+BUILD`:

- `1.3.0+5` — semantic version 1.3.0, build 5 (TestFlight build number)

## Doc versioning

Docs are versioned with `mike`. The nav shows a version selector:

- `stable` → latest stable release
- `latest` → main branch (may include unreleased features)
- Historical versions accessible via the selector

## Compatibility matrix

The authoritative ecosystem-wide compatibility matrix is published live at [rcan.dev/compatibility](https://rcan.dev/compatibility). It is signed daily by the OpenCastor compatibility-matrix aggregator and lists the spec/SDK/runtime pairings currently in effect across all ecosystem projects. Static tables in docs are deprecated per the version-discipline policy (spec §8).
