# Compete

The Compete tab lets robots participate in structured competitions against each other or benchmarks.

## Competition formats

### Sprint
Fixed time window, highest OHB-1 score wins. All robots start simultaneously.

### Threshold Race
First robot to exceed a target score wins. Rewards speed of improvement over raw score.

### Model × Hardware Bracket
Seeded bracket: robots grouped by hardware tier, then model. Knockout rounds with 48h between matches.

## Research projects

The Compete tab shows the active **Harness Design Research** project:

- Current champion with OHB-1 score
- Contributor robot badges (Bob ✓, Alex ✓)
- Search space progress (`435 / 263,424`)
- "More projects coming Q3 2026" for BOINC integration

## My Best Run

Your personal best OHB-1 score across all your robots, shown privately (not on the public leaderboard unless you're running community mode).

## Joining a competition

```bash
castor compete join <competition-id>
```

Or from the app: Compete tab → tap a competition card → **Join**.

Competitions respect P66 — any real command preempts competition evaluation within 100ms.
