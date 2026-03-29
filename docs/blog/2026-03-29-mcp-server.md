# OpenCastor is now an MCP server — and what that means for Physical AI compliance

**March 29, 2026** · Craig Merry

---

The Model Context Protocol is having a moment. What started as a way to give AI agents access to file systems and databases is becoming the standard interface layer between AI models and the real world. This week we shipped `castor mcp` — a full MCP server for OpenCastor — and the implications go deeper than "Claude Code can now talk to your robot."

## The problem it solves

Until now, interacting with a robot running OpenCastor programmatically meant one of three things:

1. Use the Flutter app (great for humans, useless for agents)
2. Write custom HTTP calls against the gateway API (works, but nothing is discoverable)
3. Send messages through OpenClaw via WhatsApp, which routes to the bridge (three hops, brittle)

If you wanted Claude Code or Codex or any agent to command a robot, you were writing glue code. `httpx` calls, auth headers, RCAN scope strings — none of it self-describes. Every new agent integration was a new integration surface to maintain.

MCP fixes this. You add one line to your Claude Code config:

```bash
claude mcp add castor -- castor mcp --token $CASTOR_MCP_TOKEN
```

Now any MCP-capable agent can call `robot_command`, `robot_status`, `fleet_list`, `rrf_lookup`, and nine other tools directly. No custom code. No protocol knowledge required.

## Twelve tools, three trust tiers

The server exposes 12 tools grouped by the Level of Assurance (LoA) required to call them:

**LoA 0 — Read-only** (any authenticated client)

- `robot_status` — live brain state, active model, uptime
- `robot_telemetry` — full system snapshot: RAM, CPU, NPU, model runtime
- `fleet_list` — all robots with compliance level and RCAN version
- `rrf_lookup` — provenance chain for any RRN, RCN, RMN, or RHN

**LoA 1 — Operate** (named agent with a standard token)

- `robot_command` — send instructions at any RCAN scope (chat/control/system/safety)
- `harness_get` — read the current agent harness config and flow graph
- `research_run` — trigger an OHB-1 benchmark run
- `contribute_toggle` — enable/disable idle compute donation
- `components_list` — list registered hardware components

**LoA 3 — Admin** (elevated token, explicit grant)

- `harness_set` — deploy a new harness configuration
- `system_upgrade` — trigger an OTA update
- `loa_enable` — enable LoA enforcement with a minimum level

A LoA-0 token cannot call `harness_set`. This is enforced server-side before the request reaches the gateway — it is not an application-layer check that can be bypassed.

## Provider-agnostic by design

The LoA tier is tied to the **token**, not the model or provider. A Gemini agent, a Codex session, a cron job, and Claude Code with identical tokens get identical access. This is intentional.

```yaml
# bob.rcan.yaml
mcp_clients:
  - name: "claude-code-laptop"
    token_hash: "sha256:abc123..."
    loa: 3
  - name: "gemini-monitoring-agent"
    token_hash: "sha256:def456..."
    loa: 0
  - name: "codex-research-runner"
    token_hash: "sha256:ghi789..."
    loa: 1
```

OpenCastor is a runtime layer, not an AI product. The intelligence is pluggable. What we provide is signing, trust enforcement, fleet registry, and hardware abstraction — and now we expose all of it through a standard interface that any AI agent can consume.

## The EU AI Act connection

Here is where it gets interesting.

The EU AI Act's technical documentation requirements (Article 11) mandate that operators of AI systems maintain records covering: system identity, hardware provenance, model provenance, safety controls, and post-market monitoring. For a physical robot running an AI agent, this means documenting the CPU, the NPU, the models loaded, the harness governing behavior, and the safety mechanisms in place.

`rrf_lookup` makes this auditable from any MCP client. A compliance agent — running on any model from any provider — can call:

```
rrf_lookup("RRN-000000000001")  → robot identity, manufacturer, firmware_hash
rrf_lookup("RCN-000000000002")  → Hailo-8 NPU, 26 TOPS, parent robot
rrf_lookup("RMN-000000000002")  → OpenVLA 7b, apache-2.0, local deployment
rrf_lookup("RHN-000000000001")  → dual-brain harness, rcan_version: 2.2
```

The full Art. 11 provenance chain is now a series of four MCP tool calls. Any agent, any model, any provider — the records are there, signed, and retrievable.

The LoA enforcement maps to Article 9 (risk management system). LoA gating means that the level of assurance required to issue a control command is declared, enforced, and logged — not aspirational documentation after the fact.

Article 14 requires human oversight mechanisms. An admin LoA-3 token to `loa_enable` means a human explicitly grants elevated access. The token is generated with `castor mcp token --name NAME --loa 3`, stored as a hash in the robot config, and can be revoked by removing the entry. The audit trail is the gateway's command log, which is also streaming to BigQuery.

## What ships now

```bash
# Install
pip install "opencastor[mcp]"

# Generate a client token
castor mcp token --name "my-agent" --loa 1

# Start the MCP server
export CASTOR_MCP_TOKEN=<generated token>
castor mcp

# Wire into Claude Code
claude mcp add castor -- castor mcp --token $CASTOR_MCP_TOKEN

# Dev mode (LoA 3, no token, local only)
CASTOR_MCP_DEV=1 castor mcp
```

The current transport is stdio, which works with any MCP client today. HTTP/SSE transport is on the roadmap — once that ships, remote agents can subscribe to live telemetry events, not just poll for status snapshots.

## What this means for the ecosystem

The OpenCastor gateway already handled RCAN signing, LoA enforcement, and fleet telemetry. The MCP server doesn't add a new security surface — every mutating tool call routes through `/api/command`, the same signed path the bridge uses. What's new is the interface: a standard, discoverable, provider-agnostic layer that any agent can use without custom integration work.

The value proposition for physical AI deployments is clear: you get a compliant, auditable robot runtime that any AI agent can interact with through a single interface, with trust levels enforced at the protocol layer rather than the application layer.

The MCP spec is moving fast. We're watching the HTTP/SSE transport PR closely. When it ships, `castor mcp --transport http --port 8002` becomes a remote endpoint that multiple agents can share simultaneously — the same robot, different providers, all operating within their declared LoA bounds.

---

**Resources**

- [GitHub Issue #775](https://github.com/craigm26/OpenCastor/issues/775) — original scope and design
- [RCAN v2.2 Specification](https://rcan.dev/spec/) — protocol reference
- [Robot Registry Foundation](https://robotregistryfoundation.org) — provenance registry
- [EU AI Act Art. 11 Technical Documentation](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689) — compliance reference
- [`castor mcp` source](https://github.com/craigm26/OpenCastor/blob/main/castor/mcp_server.py)
