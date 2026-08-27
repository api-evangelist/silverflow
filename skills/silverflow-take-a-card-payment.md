---
name: silverflow-take-a-card-payment
description: Authorize, clear and settle a card payment through Silverflow, including the
  pre-flight checks, the idempotency contract, and how to tell which undo operation applies.
api: Silverflow API
api_base: https://eu-west-1.api.silverflow.com/v1
sandbox_base: https://eu-west-1.api-sbx.silverflow.com/v1
generated: '2026-08-27'
method: generated
source: openapi/silverflow-openapi.yml + https://docs.silverflow.com/guides/charge-types +
  https://docs.silverflow.com/guides/charge-actions + https://docs.silverflow.com/guides/idempotency
operations:
- postCardInfo
- estimateInterchangeFee
- performRiskAssessment
- create3dsAuthentication
- createCharge
- getCharge
- triggerManualClearing
- increment
- reverse
- cancel
- refund
- getActions
---

# Take a card payment with Silverflow

Silverflow is a card processor sitting directly on the networks. A payment is a **charge**, and
every change after authorization is an **action appended to that charge** — never an edit of it.

## Before you charge

These are cheap, non-committal calls. Use them; they are the only rehearsal this API offers.

1. `postCardInfo` — `POST /cardInfo`. Resolve the PAN's network, funding type and issuer
   details. Do this before deciding routing or surcharging.
2. `estimateInterchangeFee` — `POST /fees/interchange/estimate`, and `getSchemeFeeEstimation` —
   `POST /fees/scheme/estimate`. Price the transaction before you run it.
3. `performRiskAssessment` — `POST /riskAssessments`. Score the transaction.
4. `create3dsAuthentication` — `POST /3ds`. Run 3-D Secure where SCA applies. If you skip it
   and the issuer wants it, you get a soft decline (Visa/Discover `1A`, Mastercard `65`) and
   have to come back here anyway.

There is **no dry-run flag** on the charge itself. The only place to rehearse the authorization
is the sandbox at `https://eu-west-1.api-sbx.silverflow.com/v1`.

## Authorize

`createCharge` — `POST /charges`. Pick the right creation operation for the charge type:

| Situation | operationId |
|---|---|
| Standard cardholder-present or e-commerce charge | `createCharge` |
| Merchant-initiated / recurring, first time | `createChargeMit` |
| Merchant-initiated, derived from an existing initial charge | `createChargeMitFromInitialCharge` |
| Funding transaction | `createChargeFunding` |
| Payout to a card | `createChargePayout` |
| Terminal-to-cloud POS | `createPosCharge` or `createChargePos` |
| ATM | `createChargeAtm` |
| Work-in-progress on an initial charge | `createChargeWip` |

**Always send `Idempotency-Key`.** All nine creation operations accept it. Three rules that will
bite you if you skip them:

- The retry must repeat the **exact same URL and the exact same body bytes, including JSON key
  order**. Silverflow compares literally, not by canonical hash. Serialise once, keep the
  string, resend the string.
- A mismatch on the same key is `409` with type `/silverflow/problems/idempotency/request-mismatch`.
- A retry while the first request is still in flight is `409` with type
  `/silverflow/problems/idempotency/request-is-still-being-processed`. That one is **retriable** —
  back off and try again.

Keys are retained 24 hours. And note the honest caveat in Silverflow's own docs: if their
idempotency store is unavailable they process the request **without** idempotency rather than
fail it. Treat the header as strong protection, not an exactly-once guarantee.

## Read the outcome

The HTTP call succeeding is not the payment succeeding. A `2xx` with a declined authorization is
the normal shape of a decline.

- Check `status.authorization`.
- Read `authorizationResponse.responseCode` (ISO 8583 field 39) and
  `authorizationResponse.responseCodeDescription`, together with `authorizationResponse.network`.
  The same meaning has different codes per network — "do not honor" is `05` on
  Visa/Mastercard/Discover and `100` on Diners. See `errors/silverflow-decline-codes.yml`.
- `getCharge` — `GET /charges/{chargeKey}` — re-reads the whole charge.
- `getActions` — `GET /charges/{chargeKey}/actions` — lists everything that has happened to it.

## Clear

Two modes, and which one you chose at creation decides your entire undo path.

- `clearingMode: auto` — Silverflow clears for you. If you set `clearAfter`, clearing happens at
  that timestamp.
- `clearingMode: manual` — you call `triggerManualClearing` (`POST /charges/{chargeKey}/clear`)
  when you are ready.

## Undo — pick by clearing state, not by intent

This is the part to get right before you act.

| Charge state | Operation | Notes |
|---|---|---|
| `clearingMode: manual`, not cleared | `reverse` (`POST /charges/{chargeKey}/reverse`) | Full or partial. Send the new total in `replacementAmount`. Emits an ISO 8583 4XX message. |
| `clearingMode: auto`, not yet sent for clearing | `cancel` (`POST /charges/{chargeKey}/cancel`) | Cancels the scheduled clearing **and** fully reverses the authorization. If `clearAfter` was set, you have until that timestamp. |
| Already cleared | `refund` (`POST /charges/{chargeKey}/refund`) | Money has moved. Silverflow states no maximum refund age — the networks' rules govern. |
| Need to raise the amount | `increment` (`POST /charges/{chargeKey}/increment`) | `clearingMode: manual` and uncleared only. Send the **new total** in `replacementAmount`, not the delta. To go *down*, use a partial `reverse`. |

Calling the wrong one fails loudly rather than silently: `cancel` on an already-submitted charge
returns `409` `/silverflow/problems/charge/clearing-already-submitted`, and clearing-mode
mismatches return `/silverflow/problems/charge/unexpected-clearing-mode`.

All six undo operations accept `Idempotency-Key`. Send it.

## Retry policy

Retriable: `429`, `502`, `503`, `504`. On `429`, honour `Retry-After` (seconds). Everything else
in `4xx`/`5xx` will fail the same way again — fix the request instead.

Errors are RFC 7807 shaped (`type`, `title`, `status`, `detail`, `instance`) but served as
`application/json`, **not** `application/problem+json`. Match on the `type` URI under
`/silverflow/problems/`, never on the media type. Full catalogue in
`errors/silverflow-problem-types.yml`.
