# REST API

The runtime exposes a REST API at `http://localhost:18789` (default). All endpoints require RCAN-scoped authentication.

## Status

```
GET /api/status
```
Returns runtime health, version, hardware tier, and P66 state.

## Agent

```
POST /api/chat
```
Send a command to the agent. Requires `chat` scope.

```json
{ "message": "What can you see?" }
```

## Contribute

```
GET  /api/contribute/status
POST /api/contribute/start
POST /api/contribute/stop
GET  /api/contribute/history
```

```
POST /api/contribute/start
```
```json
{ "run_type": "community" }   // or "personal"
```

## Harness

```
GET  /api/harness/status
POST /api/harness/apply-champion
POST /api/harness/auto-apply
```

```
POST /api/harness/apply-champion
```
Reads pending champion from Firestore (or `champion.yaml` fallback), merges tunables into live config. Never modifies P66 settings. Requires `operator` role.

```json
{ "dry_run": false }
```

```
POST /api/harness/auto-apply
```
Sets `contribute.auto_apply_champion` flag in Firestore for this robot.

```json
{ "enabled": true }
```

## Research

```
GET /api/research/status
GET /api/research/contributors
```

`/api/research/status` returns:
```json
{
  "search_space_size": 263424,
  "explored_count": 435,
  "explored_pct": 0.17,
  "champion": { "candidate_id": "lower_cost", "score": 0.6541 }
}
```

## Compete

```
GET  /api/compete/status
POST /api/compete/join
GET  /api/leaderboard
```

## Peers

```
GET  /api/peers
POST /api/peer-test
```
