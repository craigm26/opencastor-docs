# Robot URI (RURI)

Every OpenCastor robot has a canonical RURI — a structured identifier modeled after URLs for physical machines.

## Format

```
rcan://<registry>/<manufacturer>/<model>/<version>/<serial>
```

## Example

```
rcan://opencastor.com/raspberrypi/pi5-hailo/1.0/RRN-000000000001
```

| Component | Value | Description |
|---|---|---|
| `registry` | `opencastor.com` | Fleet registry domain |
| `manufacturer` | `raspberrypi` | Hardware manufacturer |
| `model` | `pi5-hailo` | Model + hardware tier |
| `version` | `1.0` | Hardware revision |
| `serial` | `RRN-000000000001` | Unique Robot Resource Name |

## RRN

The **Robot Resource Name** is the stable unique identifier assigned at registration. It's used in:

- All Firestore documents
- RCAN message headers
- Telemetry streams
- Castor Credits accounting
- Research contributor lineage

Format: `RRN-{12-digit zero-padded integer}`

```
RRN-000000000001   ← first registered robot
RRN-000000000005   ← fifth registered robot
```

## Registration

```bash
castor register \
  --name "Bob" \
  --hardware pi5-hailo \
  --model "Raspberry Pi 5 + Hailo-8L"
```

The RRN is assigned by the registry and stored in the robot's config. It never changes.
