# Changelog

## OpenCastor v2026.3.27.1 — 2026-03-27

- **RCAN v2.2 full ecosystem** — rcan-py v1.2.1, rcan-ts v1.2.1, rcan-spec v2.2.0
- **ML-DSA-65 (FIPS 204) only** — Ed25519 fully removed; all messages post-quantum signed
- **Multi-type entity numbering** — RRN/RCN/RMN/RHN registered via RRF
- **LoA enforcement** — `castor loa status/enable/disable`; Flutter LoA button
- **Hardware component registry** — `castor components detect/list/register`; `ComponentsScreen`
- **castor rrf** CLI — register/components/models/harness/status/wipe
- **LLMFit in castor doctor** — reports active model fit, headroom GB, max context
- **WebSocket real-time telemetry** — `wsTelemetryProvider` at `ws://192.168.68.88:8001/ws/telemetry`
- **Skills live push** — bridge pushes `/api/skills` to Firestore `telemetry/skills` subcollection
- **ProvenanceCard** in Flutter app — shows full RRF chain (🤖→🔌→🧠→⚙️)
- **Bob (RRN-000000000001)** — fully registered, L5 compliant, visible in fleet UI
- **RRF v1 API deprecated** — HTTP 410, all traffic on `/v2/`
- **opencastor-client** — chat-first detail screen, hardware/transport/software/components screens


- Opt-in champion deployment: `POST /api/harness/apply-champion` + `auto-apply` endpoint
- `WorkUnit.run_type` field: `personal` vs `community`
- Pattern engine: `single_agent_supervisor`, `initializer_executor`, `multi_agent`, `reactive`
- Memory backends: `working`, `filesystem`, `firestore` with overflow strategies
- Security layer: OPA guardrail, telemetry exporter, `SecurityContext`
- `GET /api/research/contributors` — per-RRN lineage and credit share
- Competition engines: Sprint, Threshold Race, Model×Hardware Bracket
- `castor leaderboard`, `castor compete`, `castor season`, `castor research` CLI
- Blog updated: real OHB-1 scores, opt-in deployment, Q3 2026 BOINC roadmap

## OpenCastor v2026.3.20.4 — 2026-03-20

- PyPI OIDC publishing (8/8 CI green)
- `castor contribute` credits API endpoints
- Karpathy loop integration

## RCAN v2.2.0 — 2026-03-21

- Contribute scope v1.7 — Castor Credits protocol, community vs personal run types
- 36 message types (aligned across spec/py/ts)
- rcan-py v1.2.1 — 631 tests
- rcan-ts v1.2.1 — 466 tests

## opencastor-client v1.3.0+5 — 2026-03-22

- NSSpeechRecognitionUsageDescription added (TestFlight ITMS-90683 fix)
- flutter_launcher_icons integration
- Opt-in champion deployment UI: pending banner, apply button, auto-apply toggle
- Pipeline explainer on all contribute surfaces
- Fleet Leaderboard: real search progress, real OHB-1 score, PipelineExplainer
- Research Projects: removed fake cards, real harness_research card + "Coming soon Q3 2026"
- Competition cards, Sprint/Threshold/Bracket compete section
- Version update notification banner
- Capabilities crash fixed (`_asList()` safe casting)
