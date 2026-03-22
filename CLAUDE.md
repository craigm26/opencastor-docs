# CLAUDE.md — opencastor-docs

This is the OpenCastor ecosystem documentation, built with MkDocs Material and deployed to docs.opencastor.com.

## How to add a page

1. Create `docs/<section>/<page>.md`
2. Add it to `mkdocs.yml` under `nav:`
3. Commit to `main` — Cloudflare Pages deploys automatically

## Nav structure

```
Getting Started → installation, quickstart, configuration, hardware-profiles
Runtime         → overview, harness, safety, contribute, credits, cli, api
RCAN            → overview, message-types, scopes, ruri, sdks
Research        → overview, ohb1-benchmark, search-space, contributing, leaderboard
Client          → overview, fleet, harness-designer, compete
Reference       → changelog, versioning, llms-txt
```

## Writing conventions

- Use `!!! safety "..."` admonition for P66 and safety-critical content
- Use `=== "Tab"` syntax for multi-language/platform examples
- Use Mermaid for architecture diagrams (`\`\`\`mermaid`)
- Keep pages focused — one concept per page
- Update `docs/llms.txt` when adding new pages
- Update `docs/reference/changelog.md` for every ecosystem release

## Updating for a new OpenCastor release

1. Update `docs/index.md` version table
2. Add entry to `docs/reference/changelog.md`
3. Update `docs/reference/versioning.md` compatibility matrix
4. Update any pages that reference specific version numbers
5. Update `docs/llms.txt` if new pages were added
6. Commit — deploy is automatic

## Key facts (for AI assistants)

- **Search space:** 263,424 configs (8×7×7×7×2×2×3×4×2)
- **Current champion:** `lower_cost`, OHB-1 score 0.6541 (21/30 tasks)
- **OHB-1 model:** `gemma3:1b` via Ollama
- **Champion deployment:** ALWAYS opt-in — never auto-applied
- **P66:** Cannot be disabled — enforced in code, not config
- **RCAN version:** v1.9.0 (spec is the single source of truth for message type IDs)
- **Bob RRN:** RRN-000000000001 | **Alex RRN:** RRN-000000000005
- **Firestore project:** opencastor
- **Cloudflare Pages project:** opencastor-docs

## Build locally

```bash
pip install -r requirements.txt
mkdocs serve
# open http://localhost:8000
```
