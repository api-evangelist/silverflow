---
name: silverflow-handle-a-dispute
description: Work a Silverflow chargeback end to end — find it, read its history, attach
  evidence and defend it, or accept it knowing that accepting is a one-way door.
api: Silverflow API
api_base: https://eu-west-1.api.silverflow.com/v1
generated: '2026-08-27'
method: generated
source: openapi/silverflow-openapi.yml + https://docs.silverflow.com/guides/dispute-lifecycle +
  https://docs.silverflow.com/guides/dispute-handling
operations:
- getDisputes
- getDispute
- getDisputeEventHistory
- getEventHistory
- getDisputeDocumentsMetadata
- addDisputeDocumentsMetadata
- uploadDocumentFile
- downloadDocumentFile
- archiveDocument
- defendDispute
- submitEvidence
- rejectEvidence
- acceptDispute
- createDisputeHistoryEventsReport
---

# Handle a Silverflow dispute

## Read before you act

- `getDisputes` — `GET /disputes`. Cursor-paginated: `limit` (1–100, default 10),
  `offsetToken`, `sortOrder`, plus `from`/`to` date filters.
- `getDispute` — `GET /disputes/{disputeKey}`. One dispute, current state.
- `getDisputeEventHistory` — `GET /disputes/{disputeKey}/eventHistory`. The full sequence of
  what has happened. Read this before choosing a branch; it tells you whether you are inside a
  response window and what the network has already been told.
- `getEventHistory` — `GET /disputes/eventHistory/{eventHistoryKey}` for one entry.

## The one-way door

**`acceptDispute` — `POST /disputes/{disputeKey}/accept` — is irreversible.** It concedes the
chargeback and moves the dispute to the terminal `closedAccepted` state. There is no un-accept
operation anywhere in the API. Never call it speculatively, never call it to "clear a queue",
and require explicit human confirmation before calling it at all.

The recoverable branch is `defendDispute`. If you are unsure which applies, defend.

## Defend

1. `addDisputeDocumentsMetadata` — `POST /disputes/{disputeKey}/documents`. Register the
   evidence document first; you get back a `dok-` key.
2. `uploadDocumentFile` — `PUT /documents/{documentKey}/file`. Upload the bytes. Metadata and
   body are two separate calls, in that order.
3. `getDisputeDocumentsMetadata` — `GET /disputes/{disputeKey}/documents` to confirm what is
   attached.
4. `submitEvidence` — `POST /disputes/{disputeKey}/submitEvidence`.
5. `defendDispute` — `POST /disputes/{disputeKey}/defend`.

`rejectEvidence` — `POST /disputes/{disputeKey}/rejectEvidence` — is the counter-path.
`downloadDocumentFile` and `archiveDocument` manage attachments afterwards.

Note that these dispute operations do **not** accept `Idempotency-Key` — only the 19
charge-surface operations do. Use `If-Match` optimistic concurrency where the resource exposes a
`version`, and otherwise re-read state with `getDispute` before retrying, because a blind retry
here is a real second submission.

## Watch it happen

Subscribe with `createEventSubscription` and you receive CloudEvents 1.0 notifications for
thirteen dispute states:

`events.disputes.received`, `.acceptanceInitiated`, `.acceptanceFailed`, `.defenseInitiated`,
`.defenseFailed`, `.evidenceSubmitted`, `.evidenceRejected`, `.awaitingResponse`,
`.closedAccepted`, `.closedWon`, `.closedLost`, `.reversed`, `.withdrawn`.

Three things about these notifications:

- They carry **identifiers only, never data**. Call `getDispute` with the subject key to read
  state.
- Delivery is **at-least-once and unordered**. Deduplicate on the CloudEvents `id`; reconstruct
  order from `time`.
- There is **no published signature verification** — no HMAC header, no signing secret. Treat a
  notification as an untrusted hint that something changed, and confirm with an authenticated
  `GET` before acting on it. The thin-payload design forces that call anyway.

`events.disputes.awaitingResponse` is the one to wire an alert to: it is the signal that a
response window is open and running.

## Report

`createDisputeHistoryEventsReport` — `POST /reports/disputeHistoryEvents` — generates a dispute
history report; collect it with `getReport` and `getReportFile`.
