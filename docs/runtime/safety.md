# Safety & Protocol 66

!!! safety "P66 is absolute"
    Protocol 66 cannot be disabled by configuration, champion harness configs, or any API call. It is enforced at the runtime level.

## What is P66?

Protocol 66 (P66) is OpenCastor's safety preemption invariant. It guarantees that any incoming command — from a human operator, the client app, or an emergency signal — immediately preempts any background work the robot is doing.

**P66 guarantee:** Background work (contribute, research evaluation, idle tasks) is cancelled within **100ms** of any incoming command.

## Priority hierarchy

```
Level 5 — ESTOP          ← hardware emergency stop
Level 4 — safety         ← RCAN safety scope
Level 3 — control        ← motor commands, physical actions
Level 2.5 — contribute   ← idle compute donation (preempted by control)
Level 2 — chat           ← conversation
Level 1 — status         ← telemetry, read-only
```

Contribute work runs at level 2.5 — above chat so conversation doesn't interrupt computation, but below control so any real command takes immediate priority.

## Consent thresholds

Physical actions require consent before execution. The threshold is configurable:

| Threshold | Behavior |
|---|---|
| `physical` | All physical actions (motor, gripper, movement) require explicit consent |
| `digital` | Only irreversible digital actions require consent |
| `none` | No consent gate (not recommended for physical robots) |

The champion harness config apply path explicitly **never** modifies `p66_consent_threshold` or any safety invariant.

## RCAN signal

When P66 triggers, the runtime sends `CONTRIBUTE_CANCEL` to the active coordinator:

```python
# RCAN message type: CONTRIBUTE_CANCEL (type 34)
{
  "type": "CONTRIBUTE_CANCEL",
  "rrn": "RRN-000000000001",
  "reason": "p66_preemption",
  "work_unit_id": "wu_abc123"
}
```

## What P66 protects

- Motor commands and physical actuators
- Gripper and manipulation actions
- Any action with real-world consequences

P66 does **not** preempt:

- Telemetry and status reads (always allowed)
- Authentication checks (always allowed)
