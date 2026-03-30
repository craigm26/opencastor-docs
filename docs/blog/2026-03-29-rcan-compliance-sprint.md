# Closing 17 RCAN v2.2 compliance gaps in one sprint

**March 29, 2026** · Craig Merry

---

When we shipped RCAN v2.2 in late March, the spec was complete but the implementation wasn't. A compliance audit identified 17 gaps across the CLI, gateway, Flutter client, and SDKs — things like missing envelope fields on messages, no delegation chain management, no training consent API, and the MCP server having no spec foundation. We closed all 17 in a single sprint. Here's what changed and why it matters.

## The audit

A systematic pass against the v2.2 spec produced 17 issues filed across 5 repos:

- **OpenCastor** (#776–785): envelope fields, compliance levels, revocation CLI, consent CLI, delegation chain management, JWKS endpoint, health enrichment, Qwen3-4B-Thinking provider
- **rcan-py** (#45): DelegationHop and MediaChunk types in the Python SDK
- **rcan-ts** (#38): same types in TypeScript
- **rcan-spec** (#185, #186): §22.9 MCP delegation semantics + §23 Training Consent API
- **opencastor-client** (#39–44): six Flutter screens missing RCAN v2.2 data

Not bugs — gaps. The system worked, it just didn't surface everything it should.

## What shipped

### Envelope fields (§4, #776)

`RCANMessage` now carries the full v2.2 envelope: `firmware_hash`, `attestation_ref`, `pq_sig`, `pq_alg`, `delegation_chain`, and `media_chunks`. These were in the spec but missing from the Python dataclass. The SDK and gateway both validate them now.

```python
msg = RCANMessage(
    type=MessageType.NAVIGATE,
    rrn="RRN-000000000001",
    firmware_hash="sha256:42139f...",
    attestation_ref="rrf://RRN-000000000001/attestation/latest",
    delegation_chain=[DelegationHop(
        issuer_ruri="rrn://craigm26/robot/bob-001",
        scope="operator",
        timestamp=time.time(),
    )],
)
```

### Compliance levels L4 and L5 (#777, #784)

`castor compliance` now accepts `--level L4` (RCAN v2.1 supply-chain checks: firmware manifest, SBOM, authority handler) and `--level L5` (v2.2 adds ML-DSA-65 PQ signing verification). Bob currently holds **L5** — the highest compliance tier.

```
$ castor compliance --level L5
✅ L5 — RCAN v2.2 ML-DSA-65 PQ signing: PASS
   firmware_hash: sha256:42139f...
   pq_kid: 7c21bf58
   attestation_ref: rrf://RRN-000000000001/attestation/latest
```

### Revocation CLI (#780)

```
$ castor revocation status
RRN-000000000001 — active (last polled 47s ago)

$ castor revocation poll
Polling RRF... active ✓
```

### Training consent API (#778, §23)

`castor consent` manages cross-robot and training data consent grants:

```
$ castor consent list
No active grants.

$ castor consent grant RRN-000000000002 --scope chat,status
✓ Granted: RRN-000000000002 scopes [chat, status]

$ castor consent training
Training data consent: not granted (EU AI Act Art. 10)
```

The spec page at [§23 Training Consent API](https://rcan.dev/docs/training-consent-api) documents the full REST interface including GDPR Art. 17 right-to-erasure (`DELETE /api/training-data/consent/{subject_id}`) and 7-year audit log retention per EU AI Act Art. 12.

### Delegation chain management (#779, #781)

`castor delegation` manages the RCAN v2.2 delegation chain — who has delegated authority to whom, up to a maximum depth of 3:

```
$ castor delegation show
No active delegation chain.

$ castor delegation depth
Current depth: 0 / 3

$ castor key-rotation status
PQ key age: 2 days
kid: 7c21bf58
Next rotation recommended: 178 days
```

The `/.well-known/rcan-keys.json` endpoint now publishes the robot's JWKS-style key set — unauthenticated, for verifiers.

### MCP delegation semantics (§22.9)

The MCP server spec now distinguishes between human-configured MCP clients (no delegation chain required — the token is pre-authorized) and robot-to-robot MCP (R2R requires a `delegation_chain` with ≥1 `DelegationHop`, max depth 3). This matters for multi-robot coordination: when Bob delegates to Alex, Alex's MCP token carries a verifiable chain.

### Flutter RCAN screens (#39–44)

Six screens updated to surface v2.2 data:

- **Identity** — RURI copy button, revocation status (color-coded), PQ key ID, offline mode flag, firmware hash
- **Conformance** — L4/L5 section with delegation chain depth, PQ signing status, attestation ref, RRF provenance link
- **Robot detail** — Consent shortcut pill added
- **Consent** — RRN-scoped tabs: "Request Access" + "Training Data" (with GDPR delete)
- **Mission** — RCAN v2.2 mission ID card + scope badges on robot message bubbles
- **Safety** — Live telemetry: revocation poll lag, offline mode, replay cache size
- **Hardware** — Multimodal capability row

## The MCP server is live

If you have Claude Code installed, you can connect to Bob directly:

```bash
claude mcp add castor -- castor mcp --token $CASTOR_MCP_TOKEN
```

14 tools across 3 LoA tiers. At LoA 0 (read-only): `robot_status`, `robot_telemetry`, `fleet_list`, `rrf_lookup`, `robot_ping`, `compliance_report`. At LoA 1 (operator): `robot_command`, `harness_get`, `research_run`, `contribute_toggle`, `components_list`. At LoA 3 (admin): `harness_set`, `system_upgrade`, `loa_enable`.

Every mutating call is signed with Bob's ML-DSA-65 key and validated against the RCAN v2.2 envelope.

## Versions

| Package | Version | Tests |
|---|---|---|
| OpenCastor | v2026.3.29.1 | 255 |
| rcan-py | v1.2.3 | 681 |
| rcan-ts | v1.2.3 | 553 |
| rcan-spec | v2.2.1 | 101 |
| opencastor-client | v1.4.4 | — |

All repos at 0 open issues.
