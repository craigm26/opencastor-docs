# Changelog

## OpenCastor v2026.3.21.2 — 2026-03-21

- Opt-in champion deployment: `POST /api/harness/apply-champion` + `auto-apply` endpoint
- `WorkUnit.run_type` field: `personal` vs `community`
- Pattern engine: `single_agent_supervisor`, `initializer_executor`, `multi_agent`, `reactive`
- Memory backends: `working`, `filesystem`, `firestore` with overflow strategies
- Security layer: OPA guardrail, telemetry exporter, `SecurityContext`
- `GET /api/research/contributors` — per-RRN lineage and credit share
- Competition engines: Sprint, Threshold Race, Model×Hardware Bracket
- `castor leaderboard`, `castor compete`, `castor season`, `castor research` CLI
- Blog updated: real OHB-1 scores, opt-in deployment, Q3 2026 BOINC roadmap

## OpenCastor v2026.3.21.1 — 2026-03-21

- RCAN v1.9.0 compliance (`ACCEPTED_RCAN_VERSIONS` extended)
- Gemini 2.0/1.5 → 2.5 model string updates throughout
- `castor contribute` telemetry via RCAN bridge
- Provider auth: `castor/auth/provider_auth.py` + `castor/providers/gated.py`
- Harness per-layer routing: `get_provider_for_layer()`

## OpenCastor v2026.3.20.4 — 2026-03-20

- PyPI OIDC publishing (8/8 CI green)
- `castor contribute` credits API endpoints
- Karpathy loop integration

## RCAN v1.9.0 — 2026-03-21

- Contribute scope v1.7 — Castor Credits protocol, community vs personal run types
- 36 message types (aligned across spec/py/ts)
- rcan-py v0.8.0 — 631 tests
- rcan-ts v0.8.0 — 466 tests

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
