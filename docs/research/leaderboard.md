# Leaderboard

The fleet leaderboard is visible in the [OpenCastor client app](https://app.opencastor.com) under **Fleet → Leaderboard**.

## What's shown

- **Search progress bar** — explored configs vs 263,424 total (live from `/api/research/status`)
- **Current champion** — candidate ID, OHB-1 score, hardware profile, contributor robots
- **Per-robot credits** — RRN, eval count, credit share %, champion flag

## API

```bash
GET /api/leaderboard
GET /api/research/contributors
```

`/api/research/contributors`:

```json
{
  "champion": {
    "candidate_id": "lower_cost",
    "score": 0.6541,
    "contributors": ["RRN-000000000001", "RRN-000000000005"]
  },
  "per_robot": [
    {
      "rrn": "RRN-000000000001",
      "eval_count": 374,
      "credit_share_pct": 85.6,
      "is_champion": true
    },
    {
      "rrn": "RRN-000000000005",
      "eval_count": 63,
      "credit_share_pct": 14.4,
      "is_champion": true
    }
  ],
  "explored_pct": 0.17,
  "search_space_size": 263424
}
```

## Privacy

- **Community runs** — public, appear on leaderboard, credit-eligible
- **Personal runs** — private, not shown, no credits

Your RRN is shown on the leaderboard (not your name or email).
