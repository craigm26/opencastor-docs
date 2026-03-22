# Installation

## Requirements

- Python 3.10+
- Any Linux/macOS board with 1 GB+ RAM (Raspberry Pi 4/5, Jetson, x86)
- Optional: Hailo-8L NPU, Coral TPU, CUDA GPU

## Install

=== "pip"

    ```bash
    pip install opencastor
    ```

=== "From source"

    ```bash
    git clone https://github.com/craigm26/OpenCastor.git
    cd OpenCastor
    pip install -e .
    ```

## First boot

```bash
castor init          # interactive setup wizard
castor status        # verify runtime is healthy
castor gateway start # start the local HTTP gateway
```

The gateway runs on `http://localhost:18789` by default.

## Verify

```bash
curl http://localhost:18789/api/status
```

Expected response:

```json
{
  "version": "v2026.3.21.2",
  "rcan_version": "1.9.0",
  "runtime": "ok",
  "p66": "active"
}
```

## Next steps

- [Quickstart →](quickstart.md) — connect a robot and run your first command
- [Configuration →](configuration.md) — tune the runtime for your hardware
- [Hardware Profiles →](hardware-profiles.md) — Pi 5 + Hailo, Jetson Orin, CPU-only setups
