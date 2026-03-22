# Search Space

The harness research pipeline explores a space of **263,424** possible configurations.

## Dimensions

```
8 × 7 × 7 × 7 × 2 × 2 × 3 × 4 × 2 = 263,424
```

| Axis | Values | Count |
|---|---|---|
| `max_iterations` | 3, 4, 5, 6, 7, 8, 9, 10 | 8 |
| `thinking_budget` | 256, 512, 1024, 2048, 3072, 4096, 8192 | 7 |
| `context_budget` | 2048, 4096, 8192, 16384, 24576, 32768, 65536 | 7 |
| `cost_gate_usd` | 0.001, 0.005, 0.01, 0.02, 0.05, 0.10, 0.25 | 7 |
| `retry_on_error` | true, false | 2 |
| `drift_detection` | true, false | 2 |
| `p66_consent_threshold` | physical, digital, none | 3 |
| `pattern` | single_agent_supervisor, initializer_executor, multi_agent, reactive | 4 |
| `memory_backend` | working, filesystem | 2 |

## Progress

| Metric | Value |
|---|---|
| Total configs | 263,424 |
| Explored | 435 (0.17%) |
| Champion score | 0.6541 |
| Est. full exploration | ~1,800 robot-days |

## Named candidates

The pipeline includes 56 named "interesting" candidates with human-readable IDs:

- `lower_cost` — minimal cost gate, conservative budgets (current champion)
- `max_thinking` — maximum thinking budget, high context
- `fast_low_cost` — low iterations, minimal cost
- `balanced_retry` — moderate everything, retry enabled
- `no_drift` — drift detection disabled for comparison
- ... and 51 more

## State tracking

The search space state is tracked in:
```
opencastor-ops/harness-research/search_space_state.json
```

```json
{
  "explored_count": 435,
  "last_updated": "2026-03-21T...",
  "champion_id": "lower_cost",
  "champion_score": 0.6541
}
```

## API

```bash
GET /api/research/status
```

```json
{
  "search_space_size": 263424,
  "explored_count": 435,
  "explored_pct": 0.17,
  "champion": {
    "candidate_id": "lower_cost",
    "score": 0.6541
  }
}
```
