# CLAUDE.md — opencastor-docs

The OpenCastor ecosystem documentation: MkDocs Material, published to docs.opencastor.com. Build locally with `pip install -r requirements.txt && mkdocs serve`.

## Adding a page

1. Create `docs/<section>/<page>.md`
2. Register it in `mkdocs.yml` under `nav:` — a page that isn't in `nav` silently won't publish
3. **Update `docs/llms.txt`** whenever you add pages

The `nav:` block in `mkdocs.yml` is the authoritative section map; don't keep a copy here.

## Writing conventions

- `!!! safety "..."` admonition for P66 and safety-critical content — not a plain note
- `=== "Tab"` syntax for multi-language / multi-platform examples
- Mermaid for architecture diagrams
- One concept per page
- Every ecosystem release gets an entry in `docs/reference/changelog.md`

## Updating for a new OpenCastor release

1. `docs/index.md` — version table
2. `docs/reference/changelog.md` — new entry
3. `docs/reference/versioning.md` — compatibility matrix
4. Any pages referencing specific version numbers
5. `docs/llms.txt` if pages were added

## Invariants worth stating

- **Champion harness deployment is ALWAYS opt-in — never auto-applied.**
- **P66 cannot be disabled** — it is enforced in code, not configuration. Don't document a way to turn it off, because there isn't one.
- Harness search space: 263,424 configs (8×7×7×7×2×2×3×4×2). The current champion and its OHB-1 score live in `opencastor-ops/harness-research/champion.yaml` — read it rather than quoting a score here, which goes stale every research run.
- RCAN message type IDs and spec version: [rcan.dev/compatibility](https://rcan.dev/compatibility) is the single source of truth.
- Bob is `RRN-000000000001`; Alex is `RRN-000000000005`. Firestore project `opencastor`; Cloudflare Pages project `opencastor-docs`.

## Deploying

**Push to `main` and it publishes.** `.github/workflows/deploy.yml` builds and
ships to Cloudflare Pages in about 40 seconds, and it is green. Do NOT run
`wrangler pages deploy` by hand here.

That makes this repo a deliberate exception to the house rule that Pages
projects are direct-upload and deployed by hand — the rule is scoped to the
repos whose org has Actions hard-blocked on a billing failure, and this org is
not one of them. `opencastor-runtime` (the marketing site, `deploy-pages.yml`)
is the same exception. Verify a push landed with `gh run list` rather than
assuming, then curl the live URL with a `?v=$(date +%s)` cache-bust.

Two build traps: `mkdocs build --strict` fails on a brand-new file with "has no
git logs" (the git-revision-date plugin), so commit before building; and the
forbidden-phrase lint pulls its dictionary from the private `opencastor-ops`
repo, so it cannot be checked locally.
