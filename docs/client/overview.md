# Client App

The OpenCastor client app is a Flutter web + iOS app for fleet management, harness design, and research participation.

**Web:** [app.opencastor.com](https://app.opencastor.com)  
**iOS:** TestFlight (v1.3.0)  
**Repo:** [craigm26/opencastor-client](https://github.com/craigm26/opencastor-client)

## Screens

| Screen | Description |
|---|---|
| **Fleet** | All registered robots, live status |
| **Fleet Leaderboard** | Research progress, champion config, contributor credits |
| **Robot** | Per-robot detail: capabilities, contribute, harness |
| **Compete** | Active competitions, brackets, research projects |
| **Harness Designer** | Visual harness config editor with pipeline view |
| **Credits** | Castor Credits balance, Pro waitlist |

## Design system

Kinetic Command — obsidian dark theme:

| Token | Value | Use |
|---|---|---|
| Background | `#0e1416` | Base surfaces |
| Cyan | `#55d7ed` | Primary accent, interactive |
| Amber | `#ffba38` | Warnings, champion highlights |
| Fonts | Space Grotesk + Inter | UI text |
| Code | JetBrains Mono | Inline code, config values |

## Contribute settings (per robot)

The Robot → Contribute tab shows:

- Enable/disable toggle (calls `/api/contribute/start` or `/stop`)
- **Pending champion banner** (amber) — shows when a new champion is available
- **Apply to this robot** button — calls `/api/harness/apply-champion`
- **Auto-apply toggle** — opt-in for automatic future deploys

See [Contribute](../runtime/contribute.md) for the full flow.
