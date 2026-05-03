# RCAN Protocol

**RCAN** (Robot Communication & Authentication Network) is the open protocol that gives every robot a canonical identity, scoped permissions, and standardized message types.

- **Protocol spec:** [continuonai/rcan-spec](https://github.com/continuonai/rcan-spec) 🟢 Public
- **Compatibility:** [rcan.dev/compatibility →](https://rcan.dev/compatibility)
- **Python SDK:** [continuonai/rcan-py](https://github.com/continuonai/rcan-py) 🟢 Public
- **TypeScript SDK:** [continuonai/rcan-ts](https://github.com/continuonai/rcan-ts) 🟢 Public

## Core concepts

### Robot URI (RURI)
A canonical identifier for every robot, structured like a URL:

```
rcan://registry/manufacturer/model/version/serial
```

Example:
```
rcan://opencastor.com/raspberrypi/pi5/1.0/RRN-000000000001
```

### Robot Resource Name (RRN)
A unique identifier assigned at registration. Used in all telemetry, Firestore documents, and API calls.

```
RRN-000000000001   ← Bob (Pi5 + Hailo-8L)
RRN-000000000005   ← Alex (Pi5)
```

### Scopes
A hierarchy of access levels:

| Scope | Level | Access |
|---|---|---|
| `discover` | 1 | Find the robot exists |
| `status` | 1.5 | Read telemetry and health |
| `chat` | 2 | Send commands, receive responses |
| `contribute` | 2.5 | Idle compute coordination |
| `control` | 3 | Motor control, physical actions |
| `safety` | 4 | Emergency stop, safety overrides |

### Message Types
36 standardized message types covering commands, telemetry, safety signals, contribute coordination, and authentication. See [Message Types](message-types.md).

## Safety invariants

RCAN encodes safety guarantees at the protocol level:

- **P66** — contribute scope (2.5) always yields to control scope (3)
- **HMAC signing** — all safety-scope messages are HMAC-signed
- **Scope validation** — tokens are checked against scope before any action is taken

These are enforced in code, not configuration.

## Recent protocol additions

See [rcan.dev/compatibility](https://rcan.dev/compatibility) for the full versioned feature matrix. Recent additions include:

- **ML-DSA-65 signing** (FIPS 204) — post-quantum algorithm replacing Ed25519
- **Multi-type entity numbering** — RRN (robots), RCN (components), RMN (models), RHN (harnesses)
- **LoA enforcement** — Level of Assurance gate on all control-scope commands
- **EU AI Act compliance** — firmware attestation (`firmware_hash`), SBOM publication, 10-year audit retention
- **Dual-brain architecture** — VLA reactive brain + LLM planning brain with confidence gate

<!-- BEGIN: ecosystem certification disclaimer (canonical, derived from spec §10) -->
> **Conformance is not certification.**
>
> Conformance to RCAN tracks (L1–L4 protocol, Gateway Authority, HIL Runtime Safety) is *self-asserted via signed bundles* and *independently replayable from those bundles*. Conformance is not certification. Certification requires audit by a qualified third-party body, which is intentionally out-of-scope for the foundation in 2026.
<!-- END: ecosystem certification disclaimer -->
