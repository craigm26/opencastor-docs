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

Current: **v2.2.0**

## rcan-py / rcan-ts

Semantic versioning tracking spec minor versions:

- rcan-py `v1.2.1` implements RCAN spec `v2.2`
- rcan-ts `v1.2.1` implements RCAN spec `v2.2`

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
| v2026.3.27.x | v2.2.0 | v1.2.1 | v1.2.1 |
| v2026.3.20.x | v1.8.0 | v1.1.0 | v1.1.0 |
