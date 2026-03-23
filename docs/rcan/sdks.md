# RCAN SDKs

Reference SDK implementations for the RCAN protocol.

## Python — rcan-py

**Repo:** [continuonai/rcan-py](https://github.com/continuonai/rcan-py) · 🟢 Public  
**Version:** v0.8.0 · 631 tests

```bash
pip install rcan-py
```

```python
from rcan import RCANClient, Scope

client = RCANClient(
    rrn="RRN-000000000001",
    scope=Scope.CONTROL,
    registry="opencastor.com"
)

await client.send_command("pick up the cup")
```

## TypeScript — rcan-ts

**Repo:** [continuonai/rcan-ts](https://github.com/continuonai/rcan-ts) · 🟢 Public  
**Version:** v0.8.0 · 466 tests

```bash
npm install rcan-ts
```

```typescript
import { RCANClient, Scope } from 'rcan-ts';

const client = new RCANClient({
  rrn: 'RRN-000000000001',
  scope: Scope.CONTROL,
  registry: 'opencastor.com',
});

await client.sendCommand('pick up the cup');
```

## Spec compliance

Both SDKs are validated against the [rcan-spec](https://github.com/continuonai/rcan-spec) 🟢 Public. The spec is the single source of truth — all message type IDs, scope levels, and HMAC signing requirements are defined there.

Current spec version: **v1.9.0**

| SDK | Spec version | Tests |
|---|---|---|
| rcan-py v0.8.0 | v1.9 | 631 |
| rcan-ts v0.8.0 | v1.9 | 466 |
