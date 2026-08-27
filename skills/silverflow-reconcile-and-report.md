---
name: silverflow-reconcile-and-report
description: Pull settlement, reconciliation, scheme-fee and interchange data out of Silverflow
  — the asynchronous report lifecycle, scheduling, distribution, and the direct reconciliation
  reads that skip reports entirely.
api: Silverflow API
api_base: https://eu-west-1.api.silverflow.com/v1
generated: '2026-08-27'
method: generated
source: openapi/silverflow-openapi.yml + https://docs.silverflow.com/guides/reporting +
  https://docs.silverflow.com/guides/reconciliation-details-report +
  https://docs.silverflow.com/guides/scheduling-reports +
  https://docs.silverflow.com/guides/distributing-reports
operations:
- createChargesReport
- createSettlementDetailsReport
- reconciliationDetails
- createNetworkFundsTransfersReport
- schemeFeeDetails
- createDisputeHistoryEventsReport
- createFraudNotificationsReport
- createAmmfReport
- createQuarterlySchemeReport
- listReport
- getReport
- getReportFile
- createReportSchedule
- listReportSchedules
- getReportSchedule
- updateReportSchedule
- archiveReportSchedule
- createDistribution
- listDistributions
- getDistribution
- updateDistribution
- archiveDistribution
- getReconciliationDetails
- getReconciliationDetailsFundsTransferDate
- listReconciliationDetailsByNetworkFundsTransferKey
- getNetworkFundsTransfers
- getNetworkFundsTransferByNetworkFundsTransferKey
- ListFileSubscriptions
- CreateFileSubscription
---

# Reconcile and report with Silverflow

Two different surfaces answer "where is my money" — pick the right one and you avoid generating
a report you did not need.

## Direct reads (synchronous, no report needed)

Use these when you want reconciliation data *now* for a known date or transfer:

- `getReconciliationDetails` — `GET /reports/reconciliationDetails`
- `getReconciliationDetailsFundsTransferDate` —
  `GET /reports/reconciliationDetails/fundsTransferDate/{fundsTransferDate}`
- `listReconciliationDetailsByNetworkFundsTransferKey` —
  `GET /reports/reconciliationDetails/networkFundsTransferKey/{networkFundsTransferKey}`
- `getNetworkFundsTransfers` —
  `GET /reports/networkFundsTransfers/fundsTransferDate/{fundsTransferDate}`
- `getNetworkFundsTransferByNetworkFundsTransferKey` —
  `GET /reports/networkFundsTransfers/{key}`

A `NetworkFundsTransfer` (`nft-`) is the settlement event; reconciliation details hang off it.
That relationship is the backbone of the whole surface — start from the transfer, walk down to
the details.

## Reports (asynchronous, three steps)

Report generation is a **`POST` that creates a job**, not a call that returns data. Every time:

1. **Request** — one of:
   - `createChargesReport` — `POST /reports/charges`
   - `createSettlementDetailsReport` — `POST /reports/settlementDetails`
   - `reconciliationDetails` — `POST /reports/reconciliationDetailRecords`
   - `createNetworkFundsTransfersReport` — `POST /reports/networkFundsTransfers`
   - `schemeFeeDetails` — `POST /reports/schemeFeeDetails`
   - `createDisputeHistoryEventsReport` — `POST /reports/disputeHistoryEvents`
   - `createFraudNotificationsReport` — `POST /reports/fraudNotificationRecords`
   - `createAmmfReport` — `POST /reports/ammfReport`
   - `createQuarterlySchemeReport` — `POST /reports/quarterlySchemeReport`
2. **Wait** — do not poll blindly. Subscribe to `events.reports.updated`
   (`reportNotificationEvent`) and react to it. If you must poll, use `getReport` —
   `GET /reports/{reportKey}` — with backoff.
3. **Collect** — `getReportFile` — `GET /reports/{reportKey}/file`. This response carries
   `Content-Disposition` and `Content-Encoding` headers, two of only three response headers
   declared anywhere in the spec.

`listReport` — `GET /reports` — lists what exists.

Watch the date-range constraints: `/silverflow/problems/report/invalid-date-range` and
`/silverflow/problems/invalid-to-or-from-date` are both real error types on this surface.
`/silverflow/problems/report/unsupported-mime-type` means you asked for a format that report
does not produce.

## Automate it

- **Schedules** — `createReportSchedule` (`POST /reportSchedules`), plus list/get/update and
  `archiveReportSchedule`. A schedule runs a report definition repeatedly.
- **Distributions** — `createDistribution` (`POST /distributions`), plus list/get/update and
  `archiveDistribution`. A distribution is *where* a generated report is delivered. A schedule
  points at a distribution; `events.distributions.sent` fires when delivery happens.
- **File subscriptions** — `CreateFileSubscription` (`POST /fileSubscription`) and
  `ListFileSubscriptions` — bulk/batch data delivery, separate from the report surface.

Schedules and distributions both use `PATCH` with `If-Match` optimistic concurrency, and
`archive` rather than delete. There is no published un-archive.

## Cost analysis

`estimateInterchangeFee` (`POST /fees/interchange/estimate`) and `getSchemeFeeEstimation`
(`POST /fees/scheme/estimate`) price transactions *before* they run;
`schemeFeeDetails` and `createSettlementDetailsReport` tell you what actually happened. Running
both and diffing them is how interchange optimisation gets measured.

## Pagination

Every list operation here uses the same cursor: `limit` (1–100, default 10), `offsetToken`,
`sortOrder` (`asc`/`desc`). The page envelope key varies per collection — there is no generic
`data` wrapper, so read the collection key from the response rather than assuming one. No
`offsetToken` in the response means there are no more pages.
