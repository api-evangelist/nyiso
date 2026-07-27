---
name: Submit and retrieve revenue-grade meter data (NYISO Finance Metering API)
description: Submit hourly revenue-grade meter data for generators, ties and subzones to NYISO as a Meter Authority, validate before committing, read the validation report, and retrieve submitted and calculated data. Market participants only.
api: https://api.nyiso.com/finance/metering
auth: HTTP Basic (MIS account) plus NAESB client certificate — two-factor, on every request
operations:
  - POST /v1/powerMetering
  - GET /v1/powerMetering
  - GET /v1/calculatedSubzoneLoad/summary
  - GET /v1/calculatedSubzoneLoad/detail
  - POST /v1/transmissionOwnerLoad
  - GET /v1/transmissionOwnerLoad
  - GET /v1/transmissionOwnerLoad/verification/summary
  - GET /v1/transmissionOwnerLoad/verification/detail
  - GET /v1/generatorPerformance
grounding: >-
  Endpoints, parameters, precision rules and the validation envelope are taken
  verbatim from the NYISO Finance APIs User's Guide v1.2 DRAFT and the NYISO
  Metering API User Guide. NYISO publishes no OpenAPI; every endpoint above
  returned 401 to an anonymous probe on 2026-07-27, which is the expected
  response without credentials.
generated: '2026-07-27'
method: generated
---

# Submit and retrieve revenue-grade meter data

This flow is for Meter Authorities and Metering Services Entities. You cannot run
it without market-participant credentials.

## 0. Before you start

You need all three:

1. A registered NYISO Market Participant relationship.
2. An MIS user account with the metering privilege, granted by your
   organization's Marketplace administrator (Settlement Data Applications User's
   Guide).
3. A digital certificate from a NAESB-Authorized Certification Authority, bound
   to that MIS account (Market Participant User's Guide).

Both factors go on **every** request. Missing either yields `401`; an
authenticated account without the privilege yields `403`.

Test against the market-trial root `https://apitest.nyiso.com/finance/` first —
every example in NYISO's guide is written against it. It is credentialed too.

## 1. Build the submission

`POST https://api.nyiso.com/finance/metering/v1/powerMetering`

Headers exactly as documented:

```
Accept: application/json
Accept-Encoding: gzip, deflate
Authorization: Basic <encoded MIS credential>
Cache-Control: no-cache
Content-Type: application/json
```

Body carries optional `submissionParameters` plus any of `generators[]`,
`ties[]`, `subzones[]`:

- `generators[]`: `genPtid` (integer, required), `dateHour` (ISO-8601 with
  Eastern offset, required), then only the measures that generator is capable of
  — `meterInjectionEnergyMwh` (0.0000 to <10,000.0000),
  `meterWithdrawalEnergyMwh` (>-10,000.0000 to 0.0000),
  `meterDemandReductionMwh`. Sending a measure the resource is not capable of is
  a validation error, not an ignored field.
- `ties[]`: `tiePtid`, `dateHour`, `meterTieFlowMwh`.
- `subzones[]`: `subzonePtid`, `dateHour`, `meterSubzoneLoadMwh`.

Rules that bite:

- **Four decimal places** on every MWh value.
- **ISO-8601 with an explicit Eastern offset** — `2021-12-14T02:00:00-05:00` in
  EST, `-04:00` in EDT. To express the end of a period use `:59:59`.
- Submission windows follow the Lock-Down Schedules posted on nyiso.com.

## 2. Dry-run first

Set the validation-only switch — `submissionParameters.doNotCommit: true` in the
Metering API guide, `doCommit: false` in the Finance APIs guide v1.2 (check the
guide matching the host you are calling). NYISO describes it as "only validates
data, but does not store any records; designed primarily for use in testing".

Set `submissionParameters.userRequestId` (≤30 characters, letters, numbers,
hyphens, underscores) so you can correlate the response with your own job.

## 3. Read the validation report

Submissions are **all-or-nothing**: if any record fails validation the entire
request is rejected, and every error found is returned. The response body carries:

- `submissionParameters` — echo, including your `userRequestId`
- `nyisoRequestId` — NYISO's UUID for the request; log it, it is what Stakeholder
  Services will ask for
- `requestTimestamp`
- `requestSummary` — per collection: `submitted`, `passedValidation`,
  `failedValidation`, `accepted`, `rejected`
- `failedValidation.<collection>[]` — the offending records, each with an
  `errors[]` array of code-prefixed strings, e.g.
  `"M00001: This is an example message"`

NYISO does not publish the validation code registry; treat the code as an opaque
correlation key and read the message.

Statuses: `200` success, `400` unparseable JSON or invalid parameters, `401` bad
credentials, `403` unauthorized for the service, `422` well-formed JSON that
failed content validation, `500` server error.

## 4. Commit, then verify

Re-send with the commit switch on. Then read back:

- `GET /v1/powerMetering?billingMonth=2021-12` — a whole service month, or
- `GET /v1/powerMetering?startTime=2021-12-20T00:00:00-05:00&endTime=2021-12-20T23:59:59-05:00`
  — a range; `endTime` is inclusive. `billingMonth` and the time pair are
  mutually exclusive.
- Narrow with repeated `genPtid` / `tiePtid` / `subzonePtid` parameters, or
  `meterDataType=ALL|GENERATOR|TIE|SUBZONE` (default `ALL`).
- `version` selects an invoice version of the data; `0` (default) returns the
  latest.
- `maUpdateStartTime` / `maUpdateEndTime` filter by when the Meter Authority last
  updated a record — use these to reconcile what changed since your last pull.

Then check NYISO's own calculation with
`GET /v1/calculatedSubzoneLoad/summary` and `/detail`.

## 5. Operating notes

- No idempotency key exists. A re-submission of the same PTID and service hour
  updates the stored record; do not retry blindly on an ambiguous failure without
  reading back first.
- No pagination. Bound response size with the time range and PTID filters.
- Retrievable history is a rolling **three years and ten months** ending with the
  current month.
- No rate limit is documented, and no webhook or event notification exists — you
  poll.
- This API replaced the SDX Upload/Download application, which was switched off
  after 2025-09-17.
