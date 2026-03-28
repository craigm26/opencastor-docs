# Message Types

RCAN v2.2 defines 40 message types across 6 categories. The spec is the single source of truth — all SDK implementations are generated from it.

## Command & Control

| Type | ID | Description |
|---|---|---|
| `COMMAND` | 1 | General command to the agent |
| `COMMAND_ACK` | 2 | Command acknowledged |
| `COMMAND_RESULT` | 3 | Command completed with result |
| `COMMAND_CANCEL` | 4 | Cancel an in-progress command |

## Telemetry

| Type | ID | Description |
|---|---|---|
| `TELEMETRY_HEARTBEAT` | 10 | Periodic health ping |
| `TELEMETRY_STATUS` | 11 | Full status snapshot |
| `TELEMETRY_HARDWARE` | 12 | Hardware profile broadcast |
| `TELEMETRY_HARNESS` | 13 | Active harness config broadcast |

## Safety

| Type | ID | Description |
|---|---|---|
| `ESTOP` | 20 | Emergency stop (HMAC-signed) |
| `ESTOP_ACK` | 21 | ESTOP acknowledged |
| `SAFETY_ALERT` | 22 | Safety condition flagged |
| `P66_PREEMPT` | 23 | P66 preemption triggered |

## Contribute

| Type | ID | Description |
|---|---|---|
| `CONTRIBUTE_START` | 30 | Robot begins work unit |
| `CONTRIBUTE_PROGRESS` | 31 | Work unit progress update |
| `CONTRIBUTE_RESULT` | 32 | Work unit completed |
| `CONTRIBUTE_CANCEL` | 34 | Work unit cancelled (P66 or manual) |
| `CONTRIBUTE_CREDITS` | 35 | Credits balance update |

## Authentication

| Type | ID | Description |
|---|---|---|
| `AUTH_CHALLENGE` | 40 | HMAC challenge |
| `AUTH_RESPONSE` | 41 | HMAC response |
| `AUTH_TOKEN` | 42 | Scoped token grant |
| `AUTH_REVOKE` | 43 | Token revocation |

## Peer

| Type | ID | Description |
|---|---|---|
| `PEER_ANNOUNCE` | 50 | Robot announces presence |
| `PEER_DISCOVER` | 51 | Discover peers on network |
| `PEER_TEST` | 52 | Latency/connectivity test |

Full spec: [rcan-spec §3](https://github.com/continuonai/rcan-spec) 🟢 Public
