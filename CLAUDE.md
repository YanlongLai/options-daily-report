# options-daily-report

> **Part of the [DappGo Stocks family](https://github.com/YanlongLai/dappgo-stocks-meta) (14 sibling repos).**
> If you've never worked on this family before, read `~/git/dappgo-stocks-meta/CLAUDE.md` first — it's the canonical entry point.

## Family quick links
- Repo map: `~/git/dappgo-stocks-meta/docs/REPO_MAP.md` — what each repo does
- Dependency flow: `~/git/dappgo-stocks-meta/docs/DEPENDENCY_FLOW.md` — when changing X, also touch Y
- Architecture: `~/git/dappgo-stocks-meta/docs/ARCHITECTURE.md` — data flow + diagrams
- Conventions: `~/git/dappgo-stocks-meta/docs/CONVENTIONS.md` — TS / Python / commit standards

## This repo's role
**Tier 2** | **Data-only / machine-emitted** | Public archive of generated artifacts for the DappGo Options mobile app.

- **Inputs**: nightly engine push from `~/git/options-core` (Tier 1 Python engine).
- **Outputs**: jsDelivr CDN consumption by `~/git/dappgo-options-app` (Tier 3) and `~/git/dappgo-stocks-mcp` (Tier 4). Dashboard consumers follow `dashboard/latest.json` to the immutable `dashboard/data-<hash12>.json` payload.
- **When to touch**: **NEVER manually**. Reports and `dashboard/*.json` are emitted by the engine. If you find yourself wanting to edit content here, you almost certainly want `~/git/options-core` instead.
- **Retention**: the engine publisher applies the 30-day base `DATA_RETENTION_DAYS` plus optional `REPORT_RETENTION_DAYS`, `AI_RETENTION_DAYS`, and `SNAPSHOT_RETENTION_DAYS` overrides before committing. It protects mutable aliases and current pointers; malformed or missing `latest.json` disables immutable-snapshot cleanup for safety.

## Sibling repos commonly edited together
- `~/git/options-core` — the engine producing the data in this repo.
- `~/git/dappgo-options-app` — the consumer mobile app.

## What this repo is

A read-mostly archive of **generated artifacts**:
- `reports/YYYY-MM-DD.md` — daily options strategy reports (markdown)
- `reports/weekly_summary_YYYY-MM-DD.md` — weekly summaries
- `dashboard/data.json` — mutable aggregate payload alias consumed by legacy clients
- `dashboard/data-<hash12>.json` — immutable aggregate payload selected by `latest.json`
- `dashboard/latest.json` — current payload pointer
- `dashboard/weekly_summary.json` — weekly summary payload
- `dashboard/index.html` — static web viewer (GitHub Pages)

## What this repo is NOT

- **Not** a place for Python source code. The analysis engine lives in `~/git/options-core`.
- **Not** a place for workflows that generate data. Those live in the
  engine repo and push outputs here via a fine-grained PAT.
- **Not** for contributors modifying reports. Reports are auto-generated.

`reports/YYYY-MM-DD.md` and the dashboard snapshot are separate generated
artifacts. A newer report does not make an older dashboard payload current;
repair the engine workflow when their dates diverge. Never hand-edit any of
these generated files.

## What contributors CAN help with

- Fixing typos or formatting in README / LICENSE
- Improving `dashboard/index.html` (the static viewer)
- Reporting data quality issues (bad price, missing field, etc.)
- Translating the README to other languages
- Suggesting features via GitHub Issues

See [`.claude/memory/contributing.md`](./.claude/memory/contributing.md) for
the full contributor guide.

## Before committing

- **Never** add Python source files — those belong in the engine repo
- **Never** modify files in `reports/` or `dashboard/*.json` by hand — they
  get overwritten on the next pipeline run

The publisher also removes dated reports, weekly summaries, AI results, and
old immutable dashboard snapshots past their configured TTL. Do not perform
that cleanup by hand in this data-only repository.
- Changes to `dashboard/index.html` are welcome
- Changes to `README.md` and `LICENSE` should go through a PR

## Language

User's primary language is 繁體中文. Respond in 繁體中文 unless the user
writes in English. Code / docs in English.
