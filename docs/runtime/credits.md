# Castor Credits

Castor Credits are earned by contributing idle compute to the fleet harness research pipeline. They're a transparent measure of how much your robot has contributed.

## How credits are calculated

```
credit_share_pct = (your_eval_count / total_eval_count) * 100
```

If your robot evaluated 374 of 437 total candidates, your credit share is **85.6%**.

Champion bonus: robots that evaluated a config that became the fleet champion receive a credit flag (`is_champion: true`) in Firestore.

## Check your credits

```bash
castor credits status
```

```
RRN: RRN-000000000001 (Bob)
Evaluations: 374
Credit share: 85.6%
Champion contribution: ✓ (lower_cost, score 0.6541)
```

## API

```bash
GET /api/research/contributors
```

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
    }
  ],
  "explored_pct": 0.17,
  "search_space_size": 263424
}
```

## Future: credit redemption

Credits are currently informational. Pro tier redemption (compute credits, priority support) is planned. See the business plan (internal — `opencastor-ops` 🔒 Private) for details.
