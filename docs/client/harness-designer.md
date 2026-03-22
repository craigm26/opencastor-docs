# Harness Designer

The Harness Designer lets you visually edit your robot's agent harness configuration.

## Opening the designer

Robot detail → **Harness** tab → **Edit** (pencil icon)

## Pipeline view

The designer shows a visual flow of the harness pipeline:

```
[Input] → [Pattern] → [LLM] → [Cost Gate] → [Consent] → [Action]
```

Each block is tappable. Drag to reorder layers.

## Design panels

Three panels at the bottom of the pipeline:

### Pattern panel
Choose the execution pattern:
- **Single Agent Supervisor** — default, one agent + supervisor
- **Initializer / Executor** — two-phase for complex tasks
- **Multi-Agent** — parallel branches (server-class only)
- **Reactive** — event-driven, low latency

### Memory panel
Choose the memory backend:
- **Working** — in-memory, fastest
- **Filesystem** — persists to disk across sessions
- **Firestore** — fleet-visible, cloud-synced

Configure overflow strategy: Summarize / Drop oldest / Error.

### Security panel
Optional OPA guardrail and telemetry export.

## Applying champion configs

When a champion config is pending, an amber banner appears at the top of the Harness screen:

> 🏆 New champion available — `lower_cost` · 0.6541

Tap **Apply** to merge the champion tunables into your current config. Tap **Review** to see a diff before applying. Tap **Dismiss** to defer.
