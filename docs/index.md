# OpenCastor Documentation

**OpenCastor** is a productized open-core RCAN runtime — Layer 4 of
the OpenCastor stack. It bundles the
[`robot-md-gateway`](https://github.com/RobotRegistryFoundation/robot-md-gateway)
safety kernel (Layer 3) with drivers, reactive safety, telemetry,
fleet management, and a cloud bridge. The safety kernel is open and
identical in every tier; OpenCastor's commercial value is in
operational ergonomics, fleet scale, and compliance prep.

## Where OpenCastor sits in the stack

| Layer | Project | Role |
|---|---|---|
| 1 | [robot-md](https://github.com/RobotRegistryFoundation/robot-md) | Declaration — ROBOT.md manifest |
| 2 | (any MCP host) | Agent runtime — Claude Code, Codex, Gemini |
| 3 | [robot-md-gateway](https://github.com/RobotRegistryFoundation/robot-md-gateway) | Mandatory exclusive enforcement path |
| 4 | **OpenCastor** ← *this* | Productized robot-facing runtime |
| 5 | [rcan-spec](https://github.com/continuonai/rcan-spec) | Wire protocol |
| 6 | [RRF](https://robotregistryfoundation.org) | Identity + evidence registry |

[See the live compatibility matrix →](https://rcan.dev/compatibility)

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
| **Protocol** | [rcan-spec](https://github.com/continuonai/rcan-spec) | 🟢 Public | [RCAN protocol](https://rcan.dev/spec/) wire spec |
| **Python SDK** | [rcan-py](https://github.com/continuonai/rcan-py) | 🟢 Public | Python RCAN implementation |
| **TypeScript SDK** | [rcan-ts](https://github.com/continuonai/rcan-ts) | 🟢 Public | TypeScript RCAN implementation |
| **Research** | [opencastor-autoresearch](https://github.com/craigm26/opencastor-autoresearch) | 🟢 Public | Harness evaluation pipeline |
| **Client App** | [opencastor-client](https://github.com/craigm26/opencastor-client) | 🟢 Public | Flutter fleet management app |
| **Docs** | [opencastor-docs](https://github.com/craigm26/opencastor-docs) | 🟢 Public | This documentation site |
| **Ops / Business** | opencastor-ops | 🔒 Private | Business, legal, compliance, roadmap |

## Versions

OpenCastor version numbers and compatible [RCAN protocol](https://rcan.dev/compatibility) releases are tracked in the live compatibility matrix.

- **OpenCastor:** see [changelog](reference/changelog.md)
- **RCAN protocol compatibility:** [rcan.dev/compatibility →](https://rcan.dev/compatibility)
- **OHB-1 Champion:** `vla-dual-brain` (OpenCastor Dual-Brain)
