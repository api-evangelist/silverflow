---
name: silverflow-onboard-a-merchant
description: Provision a merchant on Silverflow — the legal entity, the BIN-bound acceptor and
  its draft/activate versioning, network enrollment, and screening — using optimistic
  concurrency rather than idempotency keys.
api: Silverflow API
api_base: https://eu-west-1.api.silverflow.com/v1
generated: '2026-08-27'
method: generated
source: openapi/silverflow-openapi.yml + https://docs.silverflow.com/guides/merchant-onboarding +
  https://docs.silverflow.com/guides/entities-and-hierarchies +
  https://docs.silverflow.com/guides/merchant-enrollment
operations:
- getBins
- getBin
- getBinConfigurations
- createMerchant
- getMerchant
- getMerchants
- updateMerchant
- createAcceptor
- getAcceptor
- getAcceptors
- getMerchantAcceptors
- updateAcceptor
- getDraftAcceptorVersion
- activateAcceptor
- getAcceptorVersion
- createEnrollment
- getAllEnrollments
- postScreening
- listScreeningResults
---

# Onboard a merchant on Silverflow

## The distinction that governs everything

A **Merchant** (`mct-`) is a legal entity. A **MerchantAcceptor** (`mac-`) is the *technical*
configuration that binds that merchant to a **BIN** for network connectivity. Silverflow's own
llms.txt leads with this: "Merchants represent legal entities; acceptors are technical
configurations for network connectivity (BIN-based)."

Charges are created against an **acceptor**, not a merchant. One merchant can carry many
acceptors — different BINs, different MCCs, different regions.

## 1. Know your BINs

- `getBins` — `GET /bins`, `getBin` — `GET /bins/{binKey}`,
  `getBinConfigurations` — `GET /bins/{binKey}/configurations`.

BINs are read-only through the API; Silverflow provisions them for your agent. Pick the BIN
before you build the acceptor — you cannot bind an acceptor to a BIN that is not configured for
the MCC or region you need, and you will get
`/silverflow/problems/merchant-acceptor/incompatible-mcc` or
`/silverflow/problems/bin/unexpected-network` if you try.

## 2. Create the merchant

`createMerchant` — `POST /merchants`. Returns an `mct-` key and a `version`.

## 3. Create the acceptor

`createAcceptor` — `POST /merchants/{merchantKey}/acceptors`. Returns a `mac-` key.

## 4. Understand acceptor versioning — this is the unusual part

Acceptors are **versioned with an editable draft**. You do not edit a live acceptor in place:

- `updateAcceptor` — `PATCH /acceptors/{acceptorKey}` — writes to the **draft** version.
- `getDraftAcceptorVersion` — `GET /acceptors/{acceptorKey}/versions/draft` — read what you are
  about to ship.
- `activateAcceptor` — `PATCH /acceptors/{acceptorKey}/versions/draft` — promotes the draft to
  active.
- `getAcceptorVersion` — `GET /acceptors/{acceptorKey}/versions/{acceptorVersion}` — read any
  historical version.

This is why a bad acceptor change is **superseded rather than rolled back**: there is no undo
operation, but there is always a prior version and a draft you can correct before activating.
Review the draft before you activate; that is the safety step.

## 5. Enroll and screen

- `createEnrollment` — `POST /enrollments` — registers the acceptor with a card-network
  programme. `getAllEnrollments` — `GET /enrollments` — accepts an optional `merchantKey` query
  filter (added in v1.413.0) and returns an empty page, not a 404, when nothing matches.
- `postScreening` — `POST /screenings` — runs compliance screening;
  `listScreeningResults` — `GET /screenings/{screeningsKey}/results` — collects the outcome.

## Concurrency: use If-Match, not Idempotency-Key

None of these provisioning operations accepts `Idempotency-Key` — that header is reserved for
the 19 money-moving charge operations. The mechanism here is **optimistic concurrency**:

- Every `PATCH` should carry an `If-Match` header.
- Its value is the `ETag` from the `GET`, or the object's own `version` attribute.
- If another process changed the object first, you get `412 Precondition Failed`
  (`/silverflow/problems/precondition-failed`). Re-read, re-apply, retry — do not blindly
  overwrite. This is exactly the lost-update problem Silverflow's docs call out.

## Reversibility

`deleteMerchant` and `deleteAcceptorVersion` are **archive** semantics, not destruction. There
is no published un-archive operation, so archive deliberately.

## Then take a payment

See `silverflow-take-a-card-payment`.
