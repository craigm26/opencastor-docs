# Contribute

`castor contribute` turns your robot's idle compute into harness research — finding the optimal AI agent configuration for robotics tasks across the fleet.

## Enable

=== "CLI"

    ```bash
    castor contribute start
    ```

=== "Config"

    ```yaml
    agent:
      contribute:
        enabled: true
        idle_after_minutes: 15
        projects:
          - harness_research
    ```

=== "App"

    Fleet view → your robot → Contribute tab → toggle on.

## How it works

1. Robot idles for `idle_after_minutes` (default: 15)
2. Runtime fetches a candidate harness config from the research queue
3. Runs the [OHB-1 benchmark](../research/ohb1-benchmark.md) — 30 real robotics tasks via local LLM (`gemma3:1b`)
4. Submits score to Firestore with your RRN as evaluator
5. Nightly pipeline finds the champion config across all contributors
6. Champion is stored as a **pending update** — you choose when to apply

**P66 guarantee:** Any incoming command preempts work within 100ms.

## Champion deployment (opt-in)

Champion configs are **never applied automatically**. When a new champion is found, the app shows an amber banner:

> 🏆 New champion config available — `lower_cost` · score 0.6541

You choose:
- **Apply to this robot** — merges tunables into live config
- **Apply to fleet** — applies to all robots
- **Dismiss** — defer indefinitely

To enable auto-apply for future champions (per robot):

```bash
castor contribute auto-apply --enable
```

## Run types

| Type | Description | Leaderboard |
|---|---|---|
| `community` | Public, credit-eligible, feeds fleet champion | ✅ |
| `personal` | Private, no credits, explore freely | ❌ |

## Castor Credits

Community runs earn **Castor Credits** — a measure of contribution proportional to the number of evaluated configs and whether any became champion.

```bash
castor credits status
```

See [Castor Credits](credits.md) for the credit formula.

## Active projects

| Project | Status | Description |
|---|---|---|
| `harness_research` | 🟢 Live | Find the optimal harness config — 263,424 search space |
| `climate_modeling` | 🔵 Q3 2026 | BOINC integration for climateprediction.net |
| `protein_folding` | 🔵 Q3 2026 | Rosetta@home via BOINC |
