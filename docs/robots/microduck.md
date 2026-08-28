# Pollen Microduck

A 25 cm, ~800 g walking biped. Fifteen servos, a camera, a time-of-flight depth
sensor, two IMUs and an articulated beak. It does not walk by inverse
kinematics — motion comes from **RL policies** trained with PPO in MuJoCo,
exported to ONNX, and executed by the on-board `robotd` daemon at **50 Hz**.

OpenCastor drives it as an **intent client** over robotd's JSON-RPC 2.0 NDJSON
socket — the same contract used by `robotctl`, the gamepad daemon and Pollen's
own phone app.

!!! safety "robotd owns the motor bus"
    robotd enforces its own limits, fall detection and battery cutoff.
    OpenCastor sends intents and **never raw motor writes**. Do not bypass the
    driver to write the servo bus while robotd is running.

## Setup

```bash
pip install opencastor
castor duck
```

That finds the duck, verifies it answers, and writes a ready config.

```bash
castor duck --deep               # also sweep the ARP neighbour table
castor duck --host 192.168.1.42  # skip discovery
castor duck --start              # configure, then run it
castor duck find                 # just list candidates
castor duck health               # live loop rate + battery
castor duck test                 # stand up and walk (asks first)
```

### Run it off-board

The duck has 1 GB of RAM and a 50 Hz control loop to protect, so the
recommended topology is OpenCastor on **another machine**, with the driver
opening an `ssh -L 7788:/run/robotd.sock` forward. On-board works — set
`transport: unix` — but expect contention with robotd, mediad and the policy
across four Cortex-A55 cores.

- Find its address with `duckctl ip` over Bluetooth, or your router's DHCP
  table. **mDNS on the stock image is unreliable** — don't count on it.
- `ssh-copy-id` your key.
- Put the login user in the `robot` group so it can open the robotd socket:
  `sudo usermod -aG robot "$USER"`.

### Bring-up

`robot.init` torques the servos on and ramps to standing over about two
seconds. The driver **does not call it automatically** (`auto_init: false`):
the duck deliberately does not move when a process starts.

!!! tip "stop() and relax() are not the same"
    `stop()` holds it standing. `relax()` cuts torque and it **will drop** —
    seat it first.

## Hardware

| | |
|---|---|
| Compute | Rockchip RK3566 — quad Cortex-A55, 1 GB RAM, 32 GB storage, NPU |
| OS | Armbian (Radxa Zero 3 profile) |
| Actuation | 15 Dynamixel-protocol servos on `/dev/ttyS2` @ 1 Mbps (14 policy-driven) |
| Sensing | Camera, ToF depth, two IMUs |
| Power | Removable NP-F550, ~1 h. Empty floor 6.6 V |

## Choreography

Pollen ships walking, standing, two kicks, a ground pick, a sit toggle, a
forward roll and seven sounds. Each is a **one-shot that holds the whole robot
while it runs** — `robot.do` takes exactly one skill, and a refusal names the
move already in possession.

There is no sequencing, no branching on what the duck can see, and no way to
say *"walk to the ball, line up, knock it toward the couch, then celebrate."*
Every one of those verbs exists. The sentence does not.

That sentence is what OpenCastor supplies. Thirteen primitives carry the two
facts a planner cannot work without — how long a move occupies the duck, and
whether it holds the robot exclusively — and thirteen routines compose them:

| Routine | What it does |
|---|---|
| `approach` | Walk forward a distance in metres, at a careful pace |
| `back_off` | Walk backward a distance in metres |
| `turn_by` | Turn in place by an angle in degrees. Positive is left |
| `scan` | Look left, then right, then back to centre |
| `nod` / `shake` | Yes, and no |
| `greet` | Look up at someone, say hello, and nod |
| `fetch` | Go to something, pick it up in the beak, and bring it back |
| `nudge` | Line up on something on the floor and knock it forward with a kick |
| `patrol` | Walk a square, looking around at each corner |
| `celebrate` | A quack, a roll and a whoop. Needs clear space ahead |
| `dance` | A little dance — lean, sway, quack |
| `settle` | Wind down: sit, say goodnight, and go quiet |

```bash
castor duck do fetch
castor duck do "greet me, then patrol the room"
```

`fetch` is one word that becomes ten primitives: look down, walk, stop, pick,
turn, stop, walk, stop, open the beak, quack. Every routine is written as a
plan a user could have typed, so nothing is hidden in code you could not have
asked for yourself.

!!! safety "A plan is a proposal"
    The choreographer computes **no permission**. Every motion goes out through
    the driver and the SafetyLayer, the duck's own limits apply on top and come
    back in `robot.state.limited_by`, and steps abort on a fall or a flat
    battery. robotd remains the only thing that decides what a servo does.

## Config

The packaged profile is `pollen/microduck` (preset #16). The shipped envelope:

```yaml
drivers:
- id: duck
  protocol: microduck
  transport: ssh          # or unix, to run on-board
  socket: /run/robotd.sock
  local_port: 7788
  max_vx: 0.2             # m/s forward
  max_vy: 0.1             # m/s strafe
  max_vyaw: 1.0           # rad/s yaw
  intent_hz: 20
  command_ttl_s: 1.5
  auto_init: false        # the duck does not move on process start

safety:
  estop_on_startup: true
  max_linear_speed_mps: 0.2
  max_angular_speed_radps: 1.0
  local_safety_wins: true
  watchdog:
    timeout_s: 2
```

`local_safety_wins` is the important line: the duck's own limits are not
advisory, and anything OpenCastor asks for outside them comes back reported
rather than obeyed.

## The same duck, in Swift

The brain also exists as a standalone Swift package,
[DuckKit](https://github.com/craigm26/duckkit), so a phone can run the *real*
trained policy with no robot in the room — which makes an AR ghost duck the
trained network walking rather than an animation of walking.

It has **zero dependencies**: a hand-written ONNX reader and an ELU multilayer
perceptron in Foundation and arithmetic, which is what lets the real
`alpha_walking.onnx` run under `swift test` on a Raspberry Pi and produce the
same floats an iPhone will. The joint order, home pose, action scaling and
filter coefficients are the same ported numbers the driver uses, and the
kinematic chain is the upstream MuJoCo model vendored as a fixture — so the
tables cannot drift from upstream without a test going red. The forward pass is
proved against onnxruntime's own output to 1e-4.

A second product, `DuckEvidence`, adds swift-crypto for the things that sign.

```swift
.package(url: "https://github.com/craigm26/duckkit.git", from: "1.0.0")
```

## Provenance

The Microduck is made by
[Pollen Robotics](https://github.com/pollen-robotics/microduck) and ships to
customers around Christmas 2026. Contract facts — the observation layout, the
joint tables and the control tuning — come from that repository, which is
Apache-2.0. OpenCastor is not affiliated with Pollen Robotics.
