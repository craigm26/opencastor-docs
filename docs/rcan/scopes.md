# Scopes & Permissions

RCAN uses a hierarchical scope system. Higher scopes include all lower-scope permissions.

## Scope hierarchy

```
5 ── safety    ← ESTOP, override, emergency
4 ── (reserved)
3 ── control   ← motors, grippers, physical actions
2.5 ── contribute ← idle compute (preempted by control)
2 ── chat      ← send commands, receive responses
1.5 ── status  ← telemetry, health, hardware info
1 ── discover  ← robot exists, basic metadata
```

## Token format

RCAN tokens are JWT-like with explicit scope:

```json
{
  "sub": "user@example.com",
  "rrn": "RRN-000000000001",
  "scope": "control",
  "exp": 1711123200,
  "iat": 1711036800
}
```

## Roles

| Role | Scopes granted |
|---|---|
| `guest` | discover, status |
| `user` | + chat |
| `contributor` | + contribute |
| `operator` | + control |
| `safety` | all |

## P66 and scope enforcement

When a `control`-scope message arrives while a `contribute`-scope work unit is running:

1. Runtime checks scope priority: `control` (3) > `contribute` (2.5)
2. P66 watchdog triggers `CONTRIBUTE_CANCEL`
3. Work unit cancelled within 100ms
4. Control command executes normally

This is enforced in `castor/auth/` — not configurable.
