# Runtime Overview

The OpenCastor runtime is a Python process that runs on your robot and exposes a REST API + RCAN message bus.

## Components

```
castor/
├── api.py           ← REST gateway (FastAPI)
├── auth/            ← RCAN authentication, scopes, HMAC
├── harness/         ← Agent execution engine
│   ├── pattern.py   ← Execution patterns (single, init/exec, multi)
│   ├── memory.py    ← Memory backends (working, filesystem, firestore)
│   └── security.py  ← OPA guardrails, telemetry
├── contribute/      ← Idle compute coordinator
│   └── credits.py   ← Castor Credits accounting
├── compete/         ← Competition engine (Sprint, Bracket, Threshold)
├── compliance.py    ← RCAN spec version enforcement
└── bridge/          ← MQTT ↔ RCAN transport bridge
```

## Startup sequence

1. Load config (`arm.rcan.yaml`)
2. Validate RCAN compliance (`castor/compliance.py`)
3. Register robot with fleet registry (Firestore)
4. Start P66 watchdog
5. Start REST gateway (`/api/*`)
6. Start MQTT bridge (if configured)
7. Start contribute coordinator (if enabled)

## Health check

```bash
GET /api/status
```

```json
{
  "version": "v2026.3.30.0",
  "rcan_version": "2.2.0",
  "runtime": "ok",
  "p66": "active",
  "contribute": "idle",
  "hardware_tier": "pi5-hailo"
}
```
