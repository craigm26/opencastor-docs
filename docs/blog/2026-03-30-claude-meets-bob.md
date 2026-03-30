# Claude meets Bob: an AI agent controlling a physical robot via MCP

**March 30, 2026** · Craig Merry

---

Something clicked this week that I want to share: I opened Claude Code, typed a question, and a robot in the next room responded. No HTTP client. No curl commands. No custom glue code. Just a conversation.

This is what the OpenCastor MCP integration looks like in practice.

## What it looks like

With `castor mcp` configured in `~/.claude.json`, Claude Code gets 14 new tools. They show up automatically — no plugin to install, no extra setup. You can see them with `/mcp`:

```
castor__robot_status       — live robot status (LoA 0, no auth needed)
castor__robot_telemetry    — sensor + system data
castor__robot_navigate     — send navigation commands (LoA 1)
castor__robot_estop        — emergency stop (LoA 0, always available)
castor__robot_skills       — list available RCAN skills
castor__send_command       — raw RCAN command dispatch
castor__get_config         — read robot config
castor__set_config         — update robot config (LoA 2)
castor__robot_attest       — run a compliance attestation
...and 5 more
```

When Claude decides to call `castor__robot_status`, it goes: Claude → MCP stdio transport → `castor mcp` binary on the Pi → gateway API → Bob's bridge → Firestore result → back to Claude. Round trip in under a second.

Here's a real exchange I had while debugging a session:

> **Me:** What's Bob's current CPU temp and active model?
>
> **Claude:** I'll check that for you.
> *[calls castor__robot_telemetry]*
> Bob is running at 48.5°C with claude-opus-4-6 via Anthropic. RAM usage is at 62% (3.1 / 5.0 GB), disk is 69% full. No thermal concerns at current load.

No switching context. No opening the Flutter app. No SSH. Claude just... knew.

## The access control layer

This isn't a remote-control-anything setup. Every tool call is gated by **Level of Assurance (LoA)**:

| LoA | What it unlocks | Who gets it |
|-----|----------------|-------------|
| 0 | Status, telemetry, ESTOP | Any client with a valid token |
| 1 | Navigation, command dispatch | Clients granted LoA 1+ |
| 2 | Config changes, model switching | Trusted clients only |
| 3 | Firmware, key rotation | Owner only |

Claude Code's token is LoA 3 on my setup — full trust — because it's running as me on my machine. A hypothetical third-party agent integration would get LoA 1 at most.

The ESTOP tool (`castor__robot_estop`) is always at LoA 0 and always wins. Protocol 66 compliance isn't optional even for Claude.

## Why MCP fits Physical AI better than REST

REST APIs are great for deterministic workflows. But AI agents don't work that way — they explore, they retry, they compose. An agent that can *discover* what a robot can do (via MCP tool descriptions) is fundamentally different from one that requires you to write a wrapper for every endpoint.

With MCP, the robot surface is self-describing. Claude doesn't need to know the OpenCastor API docs to work with Bob. It reads the tool schema, infers what's possible, and composes calls. When I asked it to "check if Bob is healthy and optimize the model for current RAM headroom," it:

1. Called `castor__robot_telemetry` to get current RAM and active model
2. Called `castor__get_config` to read the model runtime config
3. Called `castor__llmfit_estimate` to estimate whether a more efficient config was possible
4. Proposed a config change and asked me to confirm before calling `castor__set_config`

That's four MCP tool calls composing into a multi-step physical AI workflow — zero custom code on my end.

## RCAN compliance flows through MCP

Every MCP tool call that mutates robot state generates a signed RCAN envelope. `castor__robot_navigate` doesn't just send a velocity command — it wraps it in a v2.2 envelope with:

- The MCP client's LoA (from token)
- The issuing agent's identity (Claude Code via claude client)
- A monotonic sequence number (replay protection)
- An ML-DSA-65 signature from Bob's signing key

The audit trail is automatic. Every action Claude takes through the MCP server shows up in `castor audit --tail` with full provenance, timestamped and signed.

## How to set it up

If you're running OpenCastor v2026.3.29.1 or later:

```bash
# Generate a token for Claude Code
castor mcp token --name claude --loa 3

# Register it with Claude Code
castor mcp install --client claude
```

That's it. The next time you open Claude Code in a project directory on the same machine as your robot, the tools are available.

For remote setups (robot on Pi, Claude Code on a Mac), the MCP server runs on the robot and you connect via SSH tunnel or the Cloudflare tunnel if you have one configured. We'll have a guide for that in the docs shortly.

## What's next

The current 14 tools cover the core robot loop: observe, plan, act, verify. Coming next:

- **Streaming telemetry** — MCP tool that opens a live sensor stream instead of polling
- **Vision tools** — attach OAK-D depth/RGB frames to the MCP response
- **Fleet tools** — when you have multiple robots, one MCP call can fan out across the fleet
- **Delegation** — a robot can issue MCP tools on behalf of itself to an orchestrating agent

The thing that surprised me most: this isn't a demo. I've been using it daily. When something breaks on Bob, my first move is asking Claude — and Claude can just *check*, without me doing anything.

That's the shift. Physical AI gets a lot more capable when the AI doing the thinking has direct, safe, auditable access to the physical system it's thinking about.

---

*OpenCastor v2026.3.29.1 is live on [GitHub](https://github.com/craigm26/opencastor) and [PyPI](https://pypi.org/project/opencastor/). MCP server docs at [opencastor.com/docs/mcp](https://opencastor.com/docs/mcp).*
