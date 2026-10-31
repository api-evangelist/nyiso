# New York Independent System Operator (NYISO)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

The New York Independent System Operator (NYISO) is the not-for-profit FERC-jurisdictional entity that operates New York State's bulk electricity grid, administers the state's wholesale energy, capacity and ancillary-services markets, and performs long-term power system planning. Formed in 1999 out of the New York Power Pool, it sits squarely in the wholesale middle of the value chain - between generators, transmission owners and interconnectors on one side and the investor-owned utilities and retail suppliers who actually bill New York consumers on the other. NYISO's API posture is the sector's classic two-speed split, and NYISO lands hard on both ends of it. Market and system data is genuinely open - the MIS public archive at mis.nyiso.com/public serves roughly sixty machine-readable report families (day-ahead and real-time LBMP, actual and forecast load, real-time fuel mix, ATC/TTC, outages, constraints, interface flows, bid data, capacity, uplift, emissions) as daily CSV and monthly ZIP with no account, no key and no referrer check, and the FERC-mandated OASIS node publishes an anonymously listable object store of transmission postings. Consumer data is the exact opposite - there is none, and none is expected, because NYISO holds no retail customer relationships. Every real REST API NYISO documents - the Finance APIs (Metering, Settlements, Invoicing) and the Metering API under api.nyiso.com - is market-participant-only, gated behind an MIS user account plus a NAESB-accredited digital certificate, and every one of those endpoints answers 401 anonymously. The United States has no consumer energy data mandate behind Green Button, and in any case Green Button binds distribution utilities, not system operators, so NYISO carries no Green Button or Consumer Data Right obligation at all.

**Areas:** Energy · United States · Electricity · Energy Markets · Grid · Open Data · System Operator · New York · Renewables · Emissions

## New York Independent System Operator (NYISO) on APIs.io

Everything profiled for New York Independent System Operator (NYISO) is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/nyiso/](https://apis.io/providers/nyiso/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [NYISO MIS Public Data Archive](https://apis.io/apis/nyiso/nyiso-mis-public-data/) — NYISO's Market Information System public archive - the operator's flagship open data surface. Roughly sixty machine-readable report families are published…
- [NYISO OASIS Postings](https://apis.io/apis/nyiso/nyiso-oasis-postings/) — NYISO's Open Access Same-Time Information System (OASIS) node, the FERC Order 889/890 transmission-service transparency surface. The operator front-end at…
- [NYISO Finance Metering API](https://apis.io/apis/nyiso/nyiso-finance-metering-api/) — The Metering component of NYISO's Finance APIs - submission and retrieval of hourly revenue-grade meter data for generators, ties and subzones, plus…
- [NYISO Finance Settlements API](https://apis.io/apis/nyiso/nyiso-finance-settlements-api/) — The Settlements component of NYISO's Finance APIs. The published guide documents GET /v1/stationPower, retrieving station power settlement data by billing…
- [NYISO Finance Invoicing API](https://apis.io/apis/nyiso/nyiso-finance-invoicing-api/) — The Invoicing component of NYISO's Finance APIs, retrieving daily reconciliation data for both settlements dollars and energy volumes. The published guide…
- [NYISO Metering API](https://apis.io/apis/nyiso/nyiso-metering-api/) — NYISO's standalone Metering API, documented separately from the Finance APIs guide and served from its own root at api.nyiso.com/metering. It covers…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### JSON Schema (10)

The data models the API reads and writes.

- [NYISO Day-Ahead Ancillary Services Prices record](https://apis.io/schemas/nyiso/nyiso-damasp/)
- [NYISO Day-Ahead Market Generator LBMP record](https://apis.io/schemas/nyiso/nyiso-damlbmp-gen/)
- [NYISO Day-Ahead Market Zonal LBMP record](https://apis.io/schemas/nyiso/nyiso-damlbmp-zone/)
- [NYISO ISO Load Forecast record](https://apis.io/schemas/nyiso/nyiso-isolf/)
- [NYISO Day-Ahead Scheduled Outages record](https://apis.io/schemas/nyiso/nyiso-outSched/)
- [NYISO Real-Time Actual Load record](https://apis.io/schemas/nyiso/nyiso-pal/)
- [NYISO Integrated Real-Time Actual Load record](https://apis.io/schemas/nyiso/nyiso-palIntegrated/)
- [NYISO Real-Time Market Zonal LBMP record](https://apis.io/schemas/nyiso/nyiso-realtime-zone/)
- [NYISO Real-Time Ancillary Services Prices record](https://apis.io/schemas/nyiso/nyiso-rtasp/)
- [NYISO Real-Time Fuel Mix record](https://apis.io/schemas/nyiso/nyiso-rtfuelmix/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Nyiso Authentication](https://apis.io/security/nyiso/nyiso-authentication/)
- [Nyiso Domain Security](https://apis.io/security/nyiso/nyiso-domain-security/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.nyiso.com/)
- [Documentation](https://www.nyiso.com/energy-market-operational-data)
- [Support](https://www.nyiso.com/contact-us)
- [Blog](https://www.nyiso.com/blog)
- [Terms of service](https://www.nyiso.com/legal-notice)

## API Evangelist rating

**Kin Score 28.2** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 12.0 |
| Contract governance | 18.2 |
| Operational transparency | 7.9 |
| Developer ergonomics | 54.2 |
| Access clarity | 17.1 |

**Agent readiness 6.3** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | — |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 17.2** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/nyiso/](https://apis.io/providers/nyiso/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for New York Independent System Operator (NYISO): the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/nyiso/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
