# Configuration

OpenCastor is configured via `config/arm.rcan.yaml` (or the path set by `CASTOR_CONFIG`).

## Minimal config

```yaml
robot:
  name: "Bob"
  hardware_tier: pi5-hailo   # pi5-hailo | pi5 | pi4 | jetson-orin | server | generic

agent:
  model: anthropic/claude-sonnet-4-6
  harness:
    max_iterations: 8
    thinking_budget: 2048
    context_budget: 16384
    cost_gate_usd: 0.05
    retry_on_error: true
    drift_detection: true
    p66_consent_threshold: physical   # physical | digital | none

  contribute:
    enabled: false
    idle_after_minutes: 15
    projects:
      - harness_research
```

## Key tunables

| Key | Default | Description |
|---|---|---|
| `max_iterations` | `8` | Max agent loop iterations per task |
| `thinking_budget` | `2048` | Token budget for model thinking/CoT |
| `context_budget` | `16384` | Max context window tokens |
| `cost_gate_usd` | `0.05` | Abort task if estimated cost exceeds this |
| `retry_on_error` | `true` | Auto-retry on transient errors |
| `drift_detection` | `true` | Detect and abort drifting tasks |
| `p66_consent_threshold` | `physical` | Minimum consent level for physical actions |

## Environment variables

| Variable | Description |
|---|---|
| `CASTOR_CONFIG` | Path to config file |
| `CASTOR_RRN` | Robot Resource Name override |
| `ANTHROPIC_API_KEY` | API key for Claude models |
| `OLLAMA_URL` | Ollama endpoint (default: `http://localhost:11434`) |
| `OHB_MODEL` | Model for OHB-1 benchmark (default: `gemma3:1b`) |

## Champion configs

When the research pipeline finds a better harness config, it's stored as a **pending update** in Firestore — never applied automatically. You choose when to apply it:

```bash
castor harness apply-champion        # apply to this robot
castor harness apply-champion --fleet # apply to all robots
```

Or use the [client app](../client/harness-designer.md) to review and apply with one tap.
