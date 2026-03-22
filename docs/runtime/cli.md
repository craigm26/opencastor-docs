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
