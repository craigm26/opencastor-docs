# OpenCastor Documentation

**OpenCastor** is an open-source runtime layer that sits between a robot's hardware and its AI. It handles authentication, safety invariants, telemetry, agent harness management, and fleet coordination — so you can focus on what your robot does, not how it stays safe.

<div class="grid cards" markdown>

-   :material-rocket-launch: **Getting Started**

    ---

    Install the runtime, connect your first robot, and run your first command in under 10 minutes.

    [:octicons-arrow-right-24: Installation](getting-started/installation.md)

-   :material-shield-check: **Safety & P66**

    ---

    Protocol 66 guarantees. Every robot, every time. Enforced in code, not prompts.

    [:octicons-arrow-right-24: Safety](runtime/safety.md)

-   :material-flask: **Harness Research**

    ---

    263,424 harness configurations. Your robot's idle compute helps find the optimal one.

    [:octicons-arrow-right-24: Research](research/overview.md)

-   :material-broadcast: **RCAN Protocol**

    ---

    The open protocol that gives every robot a canonical identity, scoped permissions, and standardized message types.

    [:octicons-arrow-right-24: RCAN](rcan/overview.md)

</div>

## Ecosystem

🟢 **Public** — open source, contributions welcome  
🔒 **Private** — internal / business / ops only

| Component | Repo | Visibility | Description |
|---|---|---|---|
| **Runtime** | [OpenCastor](https://github.com/craigm26/OpenCastor) | 🟢 Public | Core runtime, API, CLI, harness engine |
| **Protocol** | [rcan-spec v2.2 |
| **Python SDK** | [rcan-py](https://github.com/continuonai/rcan-py) | 🟢 Public | Python RCAN implementation |
| **TypeScript SDK** | [rcan-ts](https://github.com/continuonai/rcan-ts) | 🟢 Public | TypeScript RCAN implementation |
| **Research** | [opencastor-autoresearch](https://github.com/craigm26/opencastor-autoresearch) | 🟢 Public | Harness evaluation pipeline |
| **Client App** | [opencastor-client](https://github.com/craigm26/opencastor-client) | 🟢 Public | Flutter fleet management app |
| **Docs** | [opencastor-docs](https://github.com/craigm26/opencastor-docs) | 🟢 Public | This documentation site |
| **Ops / Business** | opencastor-ops | 🔒 Private | Business, legal, compliance, roadmap |

## Current Version

- **OpenCastor: `v2026.3.30.0``
- **RCAN Spec:** `v2.2.0`
- **rcan-py:** `v1.2.1` · **rcan-ts:** `v1.2.1`
- **OHB-1 Champion:** `vla-dual-brain` (OpenCastor Dual-Brain, RCAN v2.2)
