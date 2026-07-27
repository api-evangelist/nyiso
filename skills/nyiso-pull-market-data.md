---
name: Pull NYISO market and system data
description: Fetch day-ahead and real-time prices, load, load forecast, ancillary services prices, fuel mix and outages from the NYISO MIS public archive, with no credentials, and parse them against derived schemas.
api: http://mis.nyiso.com/public
auth: none
operations:
  - GET /public/csv/damlbmp/<YYYYMMDD>damlbmp_zone.csv
  - GET /public/csv/damlbmp/<YYYYMMDD>damlbmp_gen.csv
  - GET /public/csv/realtime/<YYYYMMDD>realtime_zone.csv
  - GET /public/csv/pal/<YYYYMMDD>pal.csv
  - GET /public/csv/palIntegrated/<YYYYMMDD>palIntegrated.csv
  - GET /public/csv/rtfuelmix/<YYYYMMDD>rtfuelmix.csv
  - GET /public/csv/isolf/<YYYYMMDD>isolf.csv
  - GET /public/csv/rtasp/<YYYYMMDD>rtasp.csv
  - GET /public/csv/damasp/<YYYYMMDD>damasp.csv
  - GET /public/csv/outSched/<YYYYMMDD>outSched.csv
  - GET /public/csv/<report-dir>/<YYYYMM01><report-name>_csv.zip
grounding: >-
  Every path above was fetched anonymously and returned 200 text/csv on
  2026-07-27. NYISO publishes no OpenAPI for this surface; the paths come from
  http://mis.nyiso.com/public/menu.htm and its report index pages, and the record
  schemas were derived from the live header rows.
generated: '2026-07-27'
method: generated
---

# Pull NYISO market and system data

NYISO's Market Information System public archive is a dated file archive, not a
REST API. There is no key, no account, no application and no rate limit. You
address a report by building its URL.

## 1. Pick the report

Start from the catalogue in `data-model/nyiso-data-model.yml`
(`public_data_catalog.reports`) or the live index at
`http://mis.nyiso.com/public/menu.htm`. Each report has a P-code (P-2A, P-63,
P-7 …), an index page `http://mis.nyiso.com/public/<P-code>list.htm`, and a
directory under `/public/csv/`.

## 2. Build the URL

- One day: `http://mis.nyiso.com/public/csv/<report-dir>/<YYYYMMDD><report-name>.csv`
- One month, zipped: `http://mis.nyiso.com/public/csv/<report-dir>/<YYYYMM01><report-name>_csv.zip`
  — the monthly bundle is keyed to the **first day of the month**. A daily-named
  ZIP returns 404; that is the convention, not an outage.
- Some reports also publish a rolling snapshot, e.g.
  `csv/LimitingConstraints/currentLimitingConstraints.csv`.

Backfills should always use the monthly ZIP rather than looping over daily files.

## 3. Fetch

```bash
curl -sSf "http://mis.nyiso.com/public/csv/rtfuelmix/20260726rtfuelmix.csv" -o rtfuelmix.csv
```

HTTPS works too. A missing file returns 404 with no error body — there is no
error envelope on this surface (see `errors/nyiso-problem-types.yml`).

## 4. Parse

Schemas for ten of the highest-traffic reports are in `json-schema/`, derived
from live header rows. The recurring shape is:

- `Time Stamp` — **`MM/DD/YYYY HH:MM[:SS]` in Eastern clock time, not ISO-8601.**
  Convert before storing.
- `Time Zone` — `EST` or `EDT`, present on some reports (`pal`, `rtfuelmix`,
  `rtasp`) and absent on others (`damlbmp`, `isolf`). When it is absent, the
  daylight-saving transition day is ambiguous and must be reconciled against the
  calendar.
- `Name` + `PTID` — the zone or generator label and its NYISO integer point
  identifier. Join to
  `csv/generator/generator.csv`, `csv/load/load.csv` or
  `csv/activetransmissionnodes/activetransmissionnodes.csv` to resolve.
- Price columns are `$/MWHr`; load is MW (`pal`) or MWh (`palIntegrated`).

`isolf` is the odd one out — it is wide, one column per zone plus a `NYISO`
total, rather than one row per zone.

## 5. Freshness

Real-time reports (`realtime`, `pal`, `rtfuelmix`, `rtasp`) publish at
five-minute granularity and the current day's file grows through the day. Refetch
the same URL; do not assume a completed file. Day-ahead reports (`damlbmp`,
`damasp`, `isolf`, `outSched`) are published for the following operating day, so
the file dated tomorrow generally exists today.

## 6. Do not

- Do not scrape `mis.nyiso.com/public/dss/` — the Decision Support System report
  builder returns 403 to anonymous callers.
- Do not expect an API contract, versioning or a deprecation notice on this
  surface. NYISO changes report content through Operational Announcements
  (report P-25) and its user guides.
