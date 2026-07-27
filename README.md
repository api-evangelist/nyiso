# New York Independent System Operator (NYISO) (nyiso)

The New York Independent System Operator (NYISO) is the not-for-profit FERC-jurisdictional entity that operates New York State's bulk electricity grid, administers the state's wholesale energy, capacity and ancillary-services markets, and performs long-term power system planning. Formed in 1999 out of the New York Power Pool, it sits squarely in the wholesale middle of the value chain — between generators, transmission owners and interconnectors on one side and the investor-owned utilities and retail suppliers who actually bill New York consumers on the other. NYISO's API posture is the sector's classic two-speed split, and NYISO lands hard on both ends of it. Market and system data is genuinely open — the MIS public archive at mis.nyiso.com/public serves roughly sixty machine-readable report families (day-ahead and real-time LBMP, actual and forecast load, real-time fuel mix, ATC/TTC, outages, constraints, interface flows, bid data, capacity, uplift, emissions) as daily CSV and monthly ZIP with no account, no key and no referrer check, and the FERC-mandated OASIS node publishes an anonymously listable object store of transmission postings. Consumer data is the exact opposite — there is none, and none is expected, because NYISO holds no retail customer relationships. Every real REST API NYISO documents — the Finance APIs (Metering, Settlements, Invoicing) and the Metering API under api.nyiso.com — is market-participant-only, gated behind an MIS user account plus a NAESB-accredited digital certificate, and every one of those endpoints answers 401 anonymously. The United States has no consumer energy data mandate behind Green Button, and in any case Green Button binds distribution utilities, not system operators, so NYISO carries no Green Button or Consumer Data Right obligation at all.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/nyiso/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/nyiso/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- Electricity
- Energy Markets
- Grid
- Open Data
- System Operator
- New York
- Renewables
- Emissions

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### NYISO MIS Public Data Archive

NYISO's Market Information System public archive — the operator's flagship open data surface. Roughly sixty machine-readable report families are published as predictable daily CSV files and monthly ZIP bundles under a stable path convention, covering day-ahead and real-time zonal and generator LBMP, ancillary services prices, ISO load forecast, real-time and integrated actual load, real-time fuel mix, ATC/TTC and long-term ATC/TTC, transmission and generation outages, limiting constraints, interface limits and flows, PAR schedules and flows, Lake Erie circulation, bid data, capacity reports, zonal and resource uplift, generator and load reference names, active transmission nodes, subzone definitions, and the DOE EIA-930 hourly demand postings. Fully anonymous over plain HTTP and HTTPS — no key, no account, no application, no licence click-through. Verified by direct probe on 2026-07-27 — for example csv/damlbmp/20260725damlbmp_zone.csv, csv/realtime/20260725realtime_zone.csv, csv/pal/20260725pal.csv, csv/rtfuelmix/20260725rtfuelmix.csv and csv/isolf/20260725isolf.csv each returned 200 text/csv, and monthly bundles such as csv/damlbmp/20260701damlbmp_zone_csv.zip returned 200. This is a file-based data surface with a URL convention, not a REST API, and NYISO publishes no OpenAPI, WSDL or JSON contract for it.

- **Human URL:** [https://www.nyiso.com/energy-market-operational-data](https://www.nyiso.com/energy-market-operational-data)
- **Base URL:** `http://mis.nyiso.com/public`

#### Tags

- Open Data
- Energy Markets
- Electricity
- Grid
- Pricing

#### Properties

- [Documentation](https://www.nyiso.com/energy-market-operational-data)
- [Portal](http://mis.nyiso.com/public/)
- [Documentation](https://www.nyiso.com/pricing-data)
- [Documentation](https://www.nyiso.com/load-data)
- [Documentation](https://www.nyiso.com/power-grid-data)
- [Documentation](https://www.nyiso.com/emissions-data)

### NYISO OASIS Postings

NYISO's Open Access Same-Time Information System (OASIS) node, the FERC Order 889/890 transmission-service transparency surface. The operator front-end at oasis.nyiso.com is a single-page application backed by a publicly readable object store at oasis-postings.nyiso.com, which answers an anonymous S3 ListBucket request with 200 application/xml and serves each posting in parallel CSV, HTML and PDF renderings under keys such as ACTIVE_TRANSMISSION_NODE/CSV/activetransmissionnodes.csv and ATC_TTC/CSV/YYYY/MM/DD/atc_ttc-YYYYMMDD.csv with history reaching back to 1999. No credentials of any kind are required. Verified by probe on 2026-07-27. Conformance to the NAESB WEQ-001 OASIS template interface was NOT verified — template-style paths under oasis.nyiso.com returned the SPA's HTML shell rather than a template response — so this is recorded as an open postings surface, not as a confirmed WEQ-001 implementation.

- **Human URL:** [http://oasis.nyiso.com/](http://oasis.nyiso.com/)
- **Base URL:** `https://oasis-postings.nyiso.com`

#### Tags

- Open Data
- Transmission
- Grid
- OASIS
- Electricity

#### Properties

- [Portal](http://oasis.nyiso.com/)
- [Documentation](https://www.nyiso.com/energy-market-operational-data)

### NYISO Finance Metering API

The Metering component of NYISO's Finance APIs — submission and retrieval of hourly revenue-grade meter data for generators, ties and subzones, plus retrieval of calculated subzone load in summary and detail form, transmission owner load submission, retrieval and verification, generator performance data, and minimum oil burn events. JSON request and response bodies, ISO-8601 dates and times with Eastern offsets, up to four decimals of MWh precision, HTTPS 1.1 over TLS 1.2. Documented operations include POST and GET /v1/powerMetering, GET /v1/calculatedSubzoneLoad/summary and /detail, POST and GET /v1/transmissionOwnerLoad, GET /v1/transmissionOwnerLoad/verification/summary and /detail, GET /v1/generatorPerformance, and GET plus POST on /v1/minOilBurnEvent and its /generator and /transmissionOwner variants. This API replaced the retired SDX upload/download application, which NYISO withdrew on 2025-09-17. Access is restricted to NYISO market participants — two-factor, requiring a valid MIS user account and password over HTTP Basic plus the NAESB certificate associated with that account on every request. All endpoints returned 401 Authorization Required to anonymous probes on 2026-07-27. A market-trial environment exists at apitest.nyiso.com and also answered 401.

- **Human URL:** [https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa](https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa)
- **Base URL:** `https://api.nyiso.com/finance/metering`

#### Tags

- Metering
- Settlements
- Electricity
- Market Participants

#### Properties

- [Documentation](https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa)
- [Documentation](https://www.nyiso.com/billings-and-settlements)
- [Documentation](https://www.nyiso.com/documents/20142/3625950/mpug.pdf)
- [Documentation](https://www.nyiso.com/documents/20142/3625950/SDA_UG.pdf)

### NYISO Finance Settlements API

The Settlements component of NYISO's Finance APIs. The published guide documents GET /v1/stationPower, retrieving station power settlement data by billing month with an optional version parameter, as JSON over HTTPS 1.1 with TLS 1.2. Restricted to NYISO market participants under the same two-factor scheme as the rest of the Finance APIs — MIS user account and password via HTTP Basic plus the associated NAESB certificate on every request. The root and the stationPower endpoint both returned 401 Authorization Required to anonymous probes on 2026-07-27.

- **Human URL:** [https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa](https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa)
- **Base URL:** `https://api.nyiso.com/finance/settlements`

#### Tags

- Settlements
- Electricity
- Market Participants

#### Properties

- [Documentation](https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa)
- [Documentation](https://www.nyiso.com/billings-and-settlements)

### NYISO Finance Invoicing API

The Invoicing component of NYISO's Finance APIs, retrieving daily reconciliation data for both settlements dollars and energy volumes. The published guide documents GET /v1/dailyReconciliation/dollar and GET /v1/dailyReconciliation/mwh, parameterised by start date and an optional version, returning JSON with two decimals of dollar precision and four decimals of MWh precision. Restricted to NYISO market participants — MIS user account and password via HTTP Basic plus the associated NAESB certificate on every request. Both the root and /v1/dailyReconciliation/dollar returned 401 Authorization Required to anonymous probes on 2026-07-27.

- **Human URL:** [https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa](https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa)
- **Base URL:** `https://api.nyiso.com/finance/invoicing`

#### Tags

- Invoicing
- Settlements
- Electricity
- Market Participants

#### Properties

- [Documentation](https://www.nyiso.com/documents/20142/45334160/NYISO%20Finance%20APIs%20User's%20Guide%20v1.2%20-%20DRAFT.pdf/4f0de58a-783d-a317-64fe-caed77ba61fa)
- [Documentation](https://www.nyiso.com/billings-and-settlements)

### NYISO Metering API

NYISO's standalone Metering API, documented separately from the Finance APIs guide and served from its own root at api.nyiso.com/metering. It covers submission and retrieval of hourly revenue-grade meter data for generators, ties and subzones via POST and GET /v1/powerMetering, and retrieval of calculated subzone load via GET /v1/calculatedSubzoneLoad/summary and /v1/calculatedSubzoneLoad/detail. JSON bodies, ISO-8601 dates and times, HTTPS 1.1 over TLS 1.2, and a rolling three-year-and-ten-month data availability window ending with the current month. Two-factor authentication is required — MIS user account and password via HTTP Basic plus the NAESB certificate associated with that account on every request. The root and /v1/powerMetering both returned 401 Authorization Required to anonymous probes on 2026-07-27.

- **Human URL:** [https://www.nyiso.com/documents/20142/27889215/NYISO%20Metering%20API%20User%20Guide.pdf/d8ed36d3-1a40-9584-6961-3fdb845ca4fa](https://www.nyiso.com/documents/20142/27889215/NYISO%20Metering%20API%20User%20Guide.pdf/d8ed36d3-1a40-9584-6961-3fdb845ca4fa)
- **Base URL:** `https://api.nyiso.com/metering`

#### Tags

- Metering
- Electricity
- Market Participants

#### Properties

- [Documentation](https://www.nyiso.com/documents/20142/27889215/NYISO%20Metering%20API%20User%20Guide.pdf/d8ed36d3-1a40-9584-6961-3fdb845ca4fa)
- [Documentation](https://www.nyiso.com/documents/20142/3625950/mpug.pdf)
- [Documentation](https://www.nyiso.com/documents/20142/3625950/SDA_UG.pdf)

## Common Properties

- [Website](https://www.nyiso.com/)
- [Portal](http://mis.nyiso.com/public/)
- [Documentation](https://www.nyiso.com/energy-market-operational-data)
- [Documentation](https://www.nyiso.com/manuals-tech-bulletins-user-guides)
- [Login](https://www.nyiso.com/market-access-login)
- [Legal](https://www.nyiso.com/legal-notice)

## Maintainers

- Kin Lane — kin@apievangelist.com
