# CLI Reference

All commands follow `castor <subcommand> [options]`.

## Core

| Command | Description |
|---|---|
| `castor init` | Interactive setup wizard |
| `castor status` | Runtime health + hardware info |
| `castor gateway start` | Start the REST gateway |
| `castor gateway stop` | Stop the gateway |
| `castor register` | Register robot with fleet registry |

## Agent

| Command | Description |
|---|---|
| `castor send "<message>"` | Send a command to the agent |
| `castor harness status` | Show active harness config |
| `castor harness apply-champion` | Apply pending champion config |
| `castor harness apply-champion --fleet` | Apply to all robots |

## Contribute

| Command | Description |
|---|---|
| `castor contribute start` | Start idle compute contribution |
| `castor contribute stop` | Stop contribution |
| `castor contribute status` | Show current work unit + history |
| `castor contribute auto-apply --enable` | Enable auto-apply of future champions |
| `castor credits status` | Show Castor Credits balance |

## Research

| Command | Description |
|---|---|
| `castor research status` | Search space progress + champion |
| `castor research benchmark` | Run OHB-1 benchmark locally |

## Compete

| Command | Description |
|---|---|
| `castor compete status` | Active competitions |
| `castor compete join <id>` | Join a competition |
| `castor season list` | List active bracket seasons |
| `castor leaderboard` | Fleet leaderboard |

## Provider auth

| Command | Description |
|---|---|
| `castor provider list` | List configured model providers |
| `castor provider auth <provider>` | Authenticate a provider |
| `castor provider status` | Check auth status for all providers |

## RCAN Compliance

| Command | Description |
|---|---|
| `castor compliance --level L4` | Run RCAN v2.1 supply-chain checks |
| `castor compliance --level L5` | Run RCAN v2.2 ML-DSA-65 PQ signing checks |
| `castor validate --category rcan_v22` | Full PQ signing validation suite |
| `castor validate --category protocol` | Check replay_window_seconds bounds |

## Revocation

| Command | Description |
|---|---|
| `castor revocation status` | Current revocation status from RRF |
| `castor revocation poll` | Trigger an immediate poll |
| `castor revocation cache` | Show cached revocation data |

## Consent & Training Data

| Command | Description |
|---|---|
| `castor consent list` | List active consent grants |
| `castor consent show <rrn>` | Show consent record for a robot |
| `castor consent grant <rrn> --scope chat,control` | Grant consent scopes |
| `castor consent deny <rrn>` | Deny a pending consent request |
| `castor consent revoke <rrn>` | Revoke an existing consent grant |
| `castor consent export --offline` | Export offline consent blob |
| `castor consent training` | Show training data consent status |

## Delegation & Key Rotation

| Command | Description |
|---|---|
| `castor delegation show` | Show active delegation chain |
| `castor delegation verify` | Verify chain signatures and depth |
| `castor delegation depth` | Check current chain depth (max 3) |
| `castor key-rotation status` | PQ key age and rotation schedule |
| `castor key-rotation rotate` | Rotate to a new ML-DSA-65 key |
| `castor key-rotation verify` | Verify current key integrity |

## MCP Server

| Command | Description |
|---|---|
| `castor mcp --token $TOKEN` | Start the MCP stdio server |
| `castor mcp token --name <n> --loa <n>` | Generate an MCP client token |
| `castor mcp clients` | List registered MCP clients |
| `castor mcp install --client claude` | Register with Claude Code |
