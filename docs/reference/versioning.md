# Versioning

## OpenCastor

Format: `YYYY.MM.DD.iterationnumber`

Examples:
- `v2026.3.21.2` — March 21, 2026, second release that day
- `v2026.3.20.4` — March 20, 2026, fourth release

This format makes the release date immediately obvious and allows multiple releases per day.

## RCAN Spec

Semantic versioning: `MAJOR.MINOR.PATCH`

- `MAJOR` — breaking protocol changes
- `MINOR` — new message types or scope changes (backward compatible)
- `PATCH` — clarifications, editorial fixes

Current: **v1.9.0**

## rcan-py / rcan-ts

Semantic versioning tracking spec minor versions:

- rcan-py `v0.8.0` implements RCAN spec `v1.9`
- rcan-ts `v0.8.0` implements RCAN spec `v1.9`

## opencastor-client

`MAJOR.MINOR.PATCH+BUILD`:

- `1.3.0+5` — semantic version 1.3.0, build 5 (TestFlight build number)

## Doc versioning

Docs are versioned with `mike`. The nav shows a version selector:

- `stable` → latest stable release
- `latest` → main branch (may include unreleased features)
- Historical versions accessible via the selector

## Compatibility matrix

| OpenCastor | RCAN Spec | rcan-py | rcan-ts |
|---|---|---|---|
| v2026.3.21.x | v1.9.0 | v0.8.0 | v0.8.0 |
| v2026.3.20.x | v1.8.0 | v0.7.0 | v0.7.0 |
