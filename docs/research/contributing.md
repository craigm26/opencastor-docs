# Contributing Compute

Any OpenCastor robot can contribute idle compute to the harness research pipeline.

## Prerequisites

- OpenCastor runtime installed and running
- Ollama installed with `gemma3:1b` (for real eval) — or simulated eval works without it
- Robot registered with fleet registry (has an RRN)

## Enable

```bash
castor contribute start
```

Or in config:

```yaml
agent:
  contribute:
    enabled: true
    idle_after_minutes: 15
    projects:
      - harness_research
```

## What your robot does

1. Waits until idle for `idle_after_minutes`
2. Fetches next candidate from `harness_eval_queue` in Firestore
3. Runs OHB-1 benchmark (30 tasks, ~5–15 min depending on hardware)
4. Submits score to `harness_eval_results` with your RRN
5. Returns to idle standby

## Run types

Choose `community` (public, credit-eligible) or `personal` (private, no credits):

```bash
castor contribute start --run-type community   # default
castor contribute start --run-type personal
```

## Credit attribution

Your RRN is stored with every result you submit:

```json
{
  "candidate_id": "config_abc123",
  "evaluated_by": "RRN-000000000001",
  "score": 0.6123,
  "evaluated_at": "2026-03-21T..."
}
```

If a config you evaluated becomes the fleet champion, you receive a champion credit flag.

Credit share formula:
```
credit_share_pct = (your_eval_count / total_eval_count) * 100
```

## Applying champion configs

Champions are **never auto-applied**. When a new champion is found, you receive a notification in the app. You choose when to apply it — to one robot or the fleet.

```bash
castor harness apply-champion           # apply to this robot
castor harness apply-champion --fleet   # apply to all robots
```

Enable auto-apply for future champions (opt-in):

```bash
castor contribute auto-apply --enable
```
