# Hardware Profiles

OpenCastor detects available hardware at startup and selects the appropriate execution tier.

## Supported tiers

| Tier ID | Hardware | TOPS | Recommended config |
|---|---|---|---|
| `pi5-hailo` | Raspberry Pi 5 + Hailo-8L | 26 | High `thinking_budget`, NPU inference |
| `pi5` | Raspberry Pi 5 (no NPU) | — | Moderate `context_budget` |
| `pi4` | Raspberry Pi 4 (4–8 GB) | — | Low `cost_gate_usd`, conservative limits |
| `jetson-orin` | NVIDIA Jetson Orin | 275 | High `context_budget`, local model |
| `coral` | Pi + Google Coral TPU | 4 | Lightweight inference, `nexa_enabled` |
| `server` | x86 / high-RAM | — | Full `context_budget`, parallel branches |
| `generic` | Any other ARM/x86 | — | Conservative defaults |

## Auto-detection

```bash
castor status --hardware
```

Output:
```
Hardware tier:  pi5-hailo
NPU:            Hailo-8L (26 TOPS) ✓
RAM:            8 GB
Disk:           64 GB (69% used)
```

## Research contribution by tier

The harness research pipeline matches work units to your hardware tier so results are comparable:

- **pi5-hailo** → evaluates NPU routing, `thinking_budget` impact on Hailo inference
- **pi4** → evaluates `cost_gate_usd` floors, minimal-cloud configs
- **server** → evaluates parallel branch patterns, high `context_budget` configs

See [Contributing Compute](../research/contributing.md) for details.
