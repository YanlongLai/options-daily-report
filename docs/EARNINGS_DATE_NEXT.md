# Optional next earnings date

Dashboard schema **1.4.0** adds the optional per-ticker field
`ticker_stats[<symbol>].earnings_date_next` to `dashboard/data.json` and its
content-addressed `dashboard/data-<hash12>.json` snapshot. Envelope schema
`schema/v1.json` is **1.1.0**; `report.json` remains a lean pointer to that
dashboard. The envelope schema also documents this field for legacy embedded
`data.ticker_stats` payloads. The dashboard schema lives in
`schemas/data.schema.json`.

```json
{
  "ticker_stats": {
    "TSLA": {"trades": 12, "win_rate": 75.0, "earnings_date_next": "2026-11-19"},
    "AMZN": {"trades": 8, "win_rate": 62.5, "earnings_date_next": null}
  }
}
```

The value is a calendar date in `YYYY-MM-DD` format or JSON `null` when the
provider has no date or fails. The engine uses
`yf_helpers.fetch_next_earnings`, ported from
`us-stocks-core.data_sources.fetch_next_earnings`, through the existing shared
yfinance session. It accepts yfinance's dictionary and DataFrame calendar
formats. The report reuses its ticker's calendar cache and passes the result
to the dashboard, including explicit nulls. Standalone dashboard runs use the
same provider path. No client-maintained date table is involved.

The field is optional for backward compatibility: consumers must accept an
omitted field in older payloads and a null value in new payloads as unknown.
The app computes days until earnings; no `earnings_in_days` field is published.
Existing ticker rows, aggregate statistics, configured universe, report date
and publication schedule are unchanged. This is an additive minor schema
change under `schemas/VERSIONING.md`; older clients need no matching release.
