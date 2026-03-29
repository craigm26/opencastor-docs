# MCP Server (`castor mcp`)

OpenCastor exposes a [Model Context Protocol](https://modelcontextprotocol.io) (MCP) server that wraps the robot runtime as 14 discoverable tools. Any MCP-capable AI agent — Claude Code, Codex, Gemini CLI, cron jobs — can command and observe robots without custom glue code.

## Quick start

```bash
# Install
pip install "opencastor[mcp]"

# Generate a client token
castor mcp token --name "my-agent" --loa 1

# Start the server
export CASTOR_MCP_TOKEN=<token>
castor mcp

# Register with Claude Code automatically
castor mcp install --client claude --token $CASTOR_MCP_TOKEN

# Dev mode (LoA 3, no token, local only)
CASTOR_MCP_DEV=1 castor mcp
```

## Client configuration

Tokens are declared in `bob.rcan.yaml` under `mcp_clients:`. The token is stored as a SHA-256 hash — never the raw value. Auth is provider-agnostic: any model (Claude, Gemini, Codex, GPT-4) with the same token gets the same access.

```yaml
mcp_clients:
  - name: "claude-code-laptop"
    token_hash: "sha256:abc123..."
    loa: 3                           # admin
  - name: "gemini-monitor"
    token_hash: "sha256:def456..."
    loa: 0                           # read-only
  - name: "codex-research"
    token_hash: "sha256:ghi789..."
    loa: 1                           # operate
```

## Tool catalogue

### LoA 0 — Read (any authenticated client)

| Tool | Description |
|---|---|
| `robot_ping` | Gateway reachability check + latency |
| `robot_status` | Brain state, active model, uptime |
| `robot_telemetry` | Full system snapshot (RAM, CPU, NPU, model runtime) |
| `fleet_list` | All robots with compliance level and RCAN version |
| `rrf_lookup` | Provenance chain for any RRN, RCN, RMN, or RHN |
| `compliance_report` | EU AI Act Art. 11 compliance status |

### LoA 1 — Operate (named agent token)

| Tool | Description |
|---|---|
| `robot_command` | Send instruction at any RCAN scope (chat/control/system/safety) |
| `harness_get` | Read current harness config + flow graph |
| `research_run` | Trigger OHB-1 benchmark run |
| `contribute_toggle` | Enable/disable idle compute donation |
| `components_list` | List registered hardware components (RCNs) |

### LoA 3 — Admin (elevated token)

| Tool | Description |
|---|---|
| `harness_set` | Deploy a new harness configuration |
| `system_upgrade` | Trigger OTA upgrade |
| `loa_enable` | Enable LoA enforcement with a minimum level |

## LoA → RCAN mapping

```
LoA 0  →  read-only, M2M_PEER role
LoA 1  →  operate, M2M_PEER + chat/control scope
LoA 3  →  admin, M2M_TRUSTED + system scope
```

ESTOP always bypasses LoA checks regardless of token level.

## EU AI Act compliance

The `rrf_lookup` and `compliance_report` tools make Art. 11 technical documentation machine-readable from any AI agent:

```python
# Five tool calls → full Art. 11 provenance chain
compliance_report("RRN-000000000001")   # safety controls, monitoring status
rrf_lookup("RRN-000000000001")          # robot identity + firmware_hash
rrf_lookup("RCN-000000000002")          # Hailo-8 NPU provenance
rrf_lookup("RMN-000000000002")          # model provenance + license
rrf_lookup("RHN-000000000001")          # harness version + components
```

## CLI reference

```bash
castor mcp                              # start server (stdio)
castor mcp --token TOKEN                # start with explicit token
castor mcp token --name NAME --loa N   # generate new client token
castor mcp clients                      # list registered clients
castor mcp install --client claude      # register with Claude Code
CASTOR_MCP_DEV=1 castor mcp            # dev mode (LoA 3, no token)
```

## Transport roadmap

| Phase | Transport | Status |
|---|---|---|
| 1 | stdio | ✅ Available now |
| 2 | HTTP/SSE | 🔜 Roadmap — enables remote agents + live telemetry streams |

See [GitHub #775](https://github.com/craigm26/OpenCastor/issues/775) for HTTP/SSE tracking.
