# Repositories

A complete map of the OpenCastor ecosystem repositories and their visibility.

🟢 **Public** — open source, Apache 2.0 or MIT licensed, contributions welcome  
🔒 **Private** — internal / business / ops only, not publicly accessible

## Open Source (Public)

Pinned protocol versions for the RCAN-coupled repos (OpenCastor, rcan-spec, rcan-py, rcan-ts) are published live at [rcan.dev/compatibility](https://rcan.dev/compatibility). The table below is the membership map; current version pairings come from the matrix.

| Repo | Description | Language |
|---|---|---|
| [craigm26/OpenCastor](https://github.com/craigm26/OpenCastor) 🟢 | Core runtime, API, CLI, harness engine | Python |
| [continuonai/rcan-spec](https://github.com/continuonai/rcan-spec) 🟢 | RCAN protocol specification | Markdown |
| [continuonai/rcan-py](https://github.com/continuonai/rcan-py) 🟢 | Python RCAN SDK | Python |
| [continuonai/rcan-ts](https://github.com/continuonai/rcan-ts) 🟢 | TypeScript RCAN SDK | TypeScript |
| [craigm26/RobotRegistryFoundation](https://github.com/craigm26/RobotRegistryFoundation) 🟢 | Robot Registry Foundation (RRF) — robot identity registry | TypeScript/Astro |
| [craigm26/opencastor-autoresearch](https://github.com/craigm26/opencastor-autoresearch) 🟢 | Harness evaluation pipeline | Python |
| [craigm26/opencastor-client](https://github.com/craigm26/opencastor-client) 🟢 | Flutter fleet management app | Dart |
| [craigm26/opencastor-docs](https://github.com/craigm26/opencastor-docs) 🟢 | This documentation site | Markdown |

## Private

| Repo | Description | Why private |
|---|---|---|
| `craigm26/opencastor-ops` 🔒 | Business, legal, compliance, roadmap | Contains pricing, contracts, internal financials |

## Contributing

All 🟢 Public repos accept contributions. See the `CONTRIBUTING.md` in each repo, or start with the [OpenCastor runtime](https://github.com/craigm26/OpenCastor).

!!! tip "Where to open issues"
    - **Runtime bugs / features** → [craigm26/OpenCastor](https://github.com/craigm26/OpenCastor/issues)
    - **Protocol questions** → [continuonai/rcan-spec](https://github.com/continuonai/rcan-spec/issues)
    - **SDK bugs** → [continuonai/rcan-py](https://github.com/continuonai/rcan-py/issues) or [continuonai/rcan-ts](https://github.com/continuonai/rcan-ts/issues)
    - **App bugs** → [craigm26/opencastor-client](https://github.com/craigm26/opencastor-client/issues)
    - **Docs improvements** → [craigm26/opencastor-docs](https://github.com/craigm26/opencastor-docs/issues)

## Org structure

The RCAN protocol repos live under the `continuonai` GitHub org (the standards body).  
OpenCastor runtime, app, research, and docs live under `craigm26` (the implementation).  
This mirrors how the IETF holds RFCs separately from any given implementation.
