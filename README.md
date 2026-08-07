# Options Daily Report — Data

**Read in:** [English](README.md) · [繁體中文](README.zh-TW.md)

[![Schema Check](https://github.com/YanlongLai/options-daily-report/actions/workflows/schema-check.yml/badge.svg)](https://github.com/YanlongLai/options-daily-report/actions/workflows/schema-check.yml)
[![Guard — no Python](https://github.com/YanlongLai/options-daily-report/actions/workflows/guard-no-python.yml/badge.svg)](https://github.com/YanlongLai/options-daily-report/actions/workflows/guard-no-python.yml)
[![Licence: CC BY-NC 4.0](https://img.shields.io/badge/Licence-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)

Published output data for the **Options** mobile app.

This repository contains only the **generated artifacts** of our options analysis
pipeline. The analysis engine itself is proprietary and not open-source.

## Contents

```
reports/
  YYYY-MM-DD.md                 — Daily strategy reports (markdown)
  weekly_summary_YYYY-MM-DD.md  — Weekly review + next-week outlook

dashboard/
  data.json                     — Latest aggregate dashboard payload
  data-<hash12>.json            — Immutable content-addressed dashboard payload
  latest.json                   — Pointer to the current content-addressed payload
  weekly_summary.json           — Latest weekly summary payload
  index.html                    — Static dashboard viewer (GitHub Pages)
```

## Artifact relationship

`reports/YYYY-MM-DD.md` is the daily report artifact. `dashboard/data.json`
is the mutable compatibility alias for the aggregate metrics, while
`dashboard/latest.json` points to the matching immutable
`dashboard/data-<hash12>.json` payload. The engine publishes the payload first
and flips the pointer second. These generated files must not be edited by hand;
if report and dashboard dates differ, fix the `options-core` generation or
publish workflow.

## Retention

The engine publisher removes dated reports, weekly summaries, AI results, and
old immutable dashboard snapshots according to the TTL settings. The
30-day base is `DATA_RETENTION_DAYS`; `REPORT_RETENTION_DAYS`,
`AI_RETENTION_DAYS`, and `SNAPSHOT_RETENTION_DAYS` can override categories
independently (allowed range 7–3650). Mutable aliases and the snapshot
referenced by `dashboard/latest.json` are always preserved. If `latest.json`
is malformed or missing, snapshot cleanup is skipped for safety. Do not
manually delete generated artifacts in this data-only repository.

## Update schedule

| File | Cadence | Time (UTC) |
|------|---------|------------|
| `reports/YYYY-MM-DD.md` | Weekdays | 13:20 |
| `reports/weekly_summary_*.md` | Sundays | 18:00 |
| `dashboard/*.json` | After each daily/weekly run | 13:25 / 18:05 |

## Licence

### Data & reports — **CC BY-NC 4.0**

You may view, share, and reference the published reports for **personal,
non-commercial** use with attribution to `options.YanlongLai.dev`.
Commercial redistribution, resale, or use for training AI/ML models
requires a separate written licence.

### Analysis source code — **Proprietary (All Rights Reserved)**

The source code that generates these reports is maintained in a private
repository and is not licensed for public use. The methodology (Black-Scholes
modelling, CP scoring, OI clustering, timing signals, AI commentary pipeline)
is proprietary.

## Disclaimer

Reports are **educational** and for **informational purposes only**. They do
not constitute investment advice, a solicitation to buy or sell securities,
or a recommendation to employ any specific strategy. Options trading carries
substantial risk of loss. Consult a licensed financial advisor before making
any investment decision.

## Get the app

Options mobile app (iOS) — currently in private beta. Coming to the
App Store soon.

## Family

Sibling product: **DappGo TW Stocks** ([tw-stocks-core](https://github.com/YanlongLai/tw-stocks-core) · [tw-stocks-daily-report](https://github.com/YanlongLai/tw-stocks-daily-report) · [dappgo-tw-stocks-app](https://github.com/YanlongLai/dappgo-tw-stocks-app)) — same three-repo architecture (engine → public data → mobile app), different domain.

---

© 2026 Yan Long Lai. All rights reserved.
