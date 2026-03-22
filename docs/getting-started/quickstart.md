# Quickstart

This guide gets a robot online and controlled via the client app in under 10 minutes.

## 1. Start the gateway

```bash
castor gateway start
```

The gateway exposes a local REST API and handles RCAN message routing.

## 2. Register your robot

```bash
castor register \
  --name "Bob" \
  --hardware pi5-hailo \
  --model "Raspberry Pi 5 + Hailo-8L"
```

This creates an RRN (Robot Resource Name) and registers the robot in the fleet registry.

## 3. Connect the client app

1. Download the [OpenCastor app](https://app.opencastor.com) (web) or TestFlight (iOS)
2. Sign in with Google
3. Your robot appears in the Fleet view within 30 seconds

## 4. Send your first command

=== "CLI"

    ```bash
    castor send "What can you see?"
    ```

=== "REST"

    ```bash
    curl -X POST http://localhost:18789/api/chat \
      -H "Content-Type: application/json" \
      -d '{"message": "What can you see?"}'
    ```

=== "App"

    Tap your robot in Fleet view → Chat tab → type a message.

## 5. Enable contribute (optional)

Donate idle compute to the harness research pipeline and earn Castor Credits:

```bash
castor contribute start
```

See [Contribute](../runtime/contribute.md) for details.

## What's running

```
castor gateway     → REST API + RCAN router (port 18789)
castor contribute  → idle compute coordinator (opt-in)
```

All processes respect P66: any incoming command preempts background work within 100ms.
