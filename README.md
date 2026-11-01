# Silverflow

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

Silverflow is an Amsterdam-based cloud-native card processing platform that connects acquirers, payment service providers, payment facilitators and merchants directly to the card networks (Visa, Mastercard, Amex, Discover, Diners, Maestro, Bancontact) through a single REST API. Founded in 2019, the company replaces legacy acquiring processor infrastructure with a contract-first, OpenAPI 3.0-described platform covering merchant and acceptor onboarding, 3-D Secure and BEP authentication, processor and network tokenization, authorization, clearing and settlement, incremental authorizations, reversals, refunds and payouts, terminal-to-cloud POS charges, ATM card management, dispute and chargeback handling, fraud notifications, transaction risk assessment, interchange and scheme-fee estimation, and a full reconciliation and reporting suite. The Silverflow API exposes 137 operations across 101 paths, emits 31 CloudEvents 1.0 webhook notifications through an event-subscription surface, and publishes a sandbox with amount-driven authorization test scenarios.

**Areas:** Payments · Card Processing · Acquiring · Financial Services · Tokenization · 3D Secure · Disputes · Chargebacks · Interchange · Reconciliation · Point-of-Sale · Netherlands

## Silverflow on APIs.io

Everything profiled for Silverflow is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/silverflow/](https://apis.io/providers/silverflow/)** — or go straight to one of the APIs or properties below.

### APIs (50)

- [Silverflow 3DS Authentication API](https://apis.io/apis/silverflow/silverflow-3ds-authentication-api/) — 3DS Authentication Endpoints
- [Silverflow Agents API](https://apis.io/apis/silverflow/silverflow-agents-api/)
- [Silverflow AMMF Submission Events API](https://apis.io/apis/silverflow/silverflow-ammf-submission-events-api/)
- [Silverflow API Keys API](https://apis.io/apis/silverflow/silverflow-api-keys-api/)
- [Silverflow BEP Authentication API](https://apis.io/apis/silverflow/silverflow-bep-authentication-api/) — The BEP Authentication API from Silverflow — 3 operation(s) for bep authentication.
- [Silverflow Bins API](https://apis.io/apis/silverflow/silverflow-bins-api/)
- [Silverflow Card Info API](https://apis.io/apis/silverflow/silverflow-card-info-api/) — Provides information from the card networks about Card Ranges.
- [Silverflow Card Management API](https://apis.io/apis/silverflow/silverflow-card-management-api/) — ATM card management actions: PIN change, PIN unblock, balance inquiry and PIN change reversal.
- [Silverflow Card Network Reports API](https://apis.io/apis/silverflow/silverflow-card-network-reports-api/) — The Card Network Reports API from Silverflow — 2 operation(s) for card network reports.
- [Silverflow Charge Actions API](https://apis.io/apis/silverflow/silverflow-charge-actions-api/) — Perform different actions on existing charges.
- [Silverflow Charges Events API](https://apis.io/apis/silverflow/silverflow-charges-events-api/)
- [Silverflow Charges Reports API](https://apis.io/apis/silverflow/silverflow-charges-reports-api/) — The Charges Reports API from Silverflow — 1 operation(s) for charges reports.
- [Silverflow Clearing Events API](https://apis.io/apis/silverflow/silverflow-clearing-events-api/)
- [Silverflow Create Charges API](https://apis.io/apis/silverflow/silverflow-create-charges-api/) — Create different types of charges.
- [Silverflow Create POS Charges API](https://apis.io/apis/silverflow/silverflow-create-pos-charges-api/) — Create a POS charge.
- [Silverflow Create Recurring API](https://apis.io/apis/silverflow/silverflow-create-recurring-api/) — Create different types of recurring charges.
- [Silverflow Currency Conversion Rates API](https://apis.io/apis/silverflow/silverflow-currency-conversion-rates-api/) — The Currency Conversion Rates API from Silverflow — 1 operation(s) for currency conversion rates.
- [Silverflow Dispute Reports API](https://apis.io/apis/silverflow/silverflow-dispute-reports-api/) — The Dispute Reports API from Silverflow — 1 operation(s) for dispute reports.
- [Silverflow Disputes API](https://apis.io/apis/silverflow/silverflow-disputes-api/) — The Disputes API from Silverflow — 9 operation(s) for disputes.
- [Silverflow Disputes Events API](https://apis.io/apis/silverflow/silverflow-disputes-events-api/)
- [Silverflow Distribution Events API](https://apis.io/apis/silverflow/silverflow-distribution-events-api/)
- [Silverflow Distributions API](https://apis.io/apis/silverflow/silverflow-distributions-api/) — The Distributions API from Silverflow — 2 operation(s) for distributions.
- [Silverflow Documents API](https://apis.io/apis/silverflow/silverflow-documents-api/) — A Document is a file with associated metadata that can be uploaded and downloaded. Documents have to be attached to a business entity, for example `Dispute…
- [Silverflow Enrollments API](https://apis.io/apis/silverflow/silverflow-enrollments-api/)
- [Silverflow Event Subscriptions API](https://apis.io/apis/silverflow/silverflow-event-subscriptions-api/)
- [Silverflow Fees API](https://apis.io/apis/silverflow/silverflow-fees-api/) — The Fees API from Silverflow — 2 operation(s) for fees.
- [Silverflow File Subscriptions API](https://apis.io/apis/silverflow/silverflow-file-subscriptions-api/) — Subscribe to raw files from the card networks.
- [Silverflow Fraud Notification Events API](https://apis.io/apis/silverflow/silverflow-fraud-notification-events-api/)
- [Silverflow Fraud Notifications API](https://apis.io/apis/silverflow/silverflow-fraud-notifications-api/) — Fraud Notifications are created by the schemes when a fraudulent transaction is reported to them. The Fraud Notification endpoints, allow to obtain more…
- [Silverflow Fraud Notifications Reports API](https://apis.io/apis/silverflow/silverflow-fraud-notifications-reports-api/) — The Fraud Notifications Reports API from Silverflow — 1 operation(s) for fraud notifications reports.
- [Silverflow Merchant Acceptors API](https://apis.io/apis/silverflow/silverflow-merchant-acceptors-api/)
- [Silverflow Merchants API](https://apis.io/apis/silverflow/silverflow-merchants-api/)
- [Silverflow Network Funds Transfers API](https://apis.io/apis/silverflow/silverflow-network-funds-transfers-api/) — The Network Funds Transfers API from Silverflow — 2 operation(s) for network funds transfers.
- [Silverflow Network Tokenization API](https://apis.io/apis/silverflow/silverflow-network-tokenization-api/) — Network Tokenization
- [Silverflow Network Tokens Events API](https://apis.io/apis/silverflow/silverflow-network-tokens-events-api/)
- [Silverflow POS Charge Actions API](https://apis.io/apis/silverflow/silverflow-pos-charge-actions-api/) — Perform different actions on a previously created charge.
- [Silverflow Processor Tokenization API](https://apis.io/apis/silverflow/silverflow-processor-tokenization-api/) — Processor Tokenization
- [Silverflow Processor Tokens Events API](https://apis.io/apis/silverflow/silverflow-processor-tokens-events-api/)
- [Silverflow Reconciliation Details API](https://apis.io/apis/silverflow/silverflow-reconciliation-details-api/) — The Reconciliation Details API from Silverflow — 3 operation(s) for reconciliation details.
- [Silverflow Reconciliation Events API](https://apis.io/apis/silverflow/silverflow-reconciliation-events-api/)
- [Silverflow Reconciliation Reports API](https://apis.io/apis/silverflow/silverflow-reconciliation-reports-api/) — The Reconciliation Reports API from Silverflow — 2 operation(s) for reconciliation reports.
- [Silverflow Report Events API](https://apis.io/apis/silverflow/silverflow-report-events-api/) — The Report Events API from Silverflow — 0 operation(s) for report events.
- [Silverflow Report Scheduling API](https://apis.io/apis/silverflow/silverflow-report-scheduling-api/) — The Report Scheduling API from Silverflow — 2 operation(s) for report scheduling.
- [Silverflow Retrieve Charges API](https://apis.io/apis/silverflow/silverflow-retrieve-charges-api/) — Retrieve a single or multiple charges.
- [Silverflow Retrieve POS Charges API](https://apis.io/apis/silverflow/silverflow-retrieve-pos-charges-api/) — Retrieve charges for given merchant and with additonal filters.
- [Silverflow Retrieve Reports API](https://apis.io/apis/silverflow/silverflow-retrieve-reports-api/) — The Retrieve Reports API from Silverflow — 3 operation(s) for retrieve reports.
- [Silverflow Scheme Fee Reports API](https://apis.io/apis/silverflow/silverflow-scheme-fee-reports-api/) — The Scheme Fee Reports API from Silverflow — 1 operation(s) for scheme fee reports.
- [Silverflow Screenings API](https://apis.io/apis/silverflow/silverflow-screenings-api/)
- [Silverflow Settlement Reports API](https://apis.io/apis/silverflow/silverflow-settlement-reports-api/) — The Settlement Reports API from Silverflow — 1 operation(s) for settlement reports.
- [Silverflow Transaction Risk Assessment API](https://apis.io/apis/silverflow/silverflow-transaction-risk-assessment-api/) — Transaction Risk Assessment

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Silverflow Events Webhooks](https://apis.io/asyncapis/silverflow/silverflow-events-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Silverflow MCP server (candidate)](https://apis.io/servers/silverflow/silverflow-mcp-server-candidate/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Silverflow Openapi Overlay](https://apis.io/overlays/silverflow/silverflow-openapi-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Silverflow Authentication](https://apis.io/security/silverflow/silverflow-authentication/)
- [Silverflow Domain Security](https://apis.io/security/silverflow/silverflow-domain-security/)
- [Silverflow Vulnerability Disclosure](https://apis.io/security/silverflow/silverflow-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Silverflow Plans Pricing](https://apis.io/plans/silverflow/silverflow-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Silverflow Rate Limits](https://apis.io/rate-limits/silverflow/silverflow-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.silverflow.com/)
- [Documentation](https://docs.silverflow.com/)
- [Getting started](https://docs.silverflow.com/guides/getting-started)
- [API reference](https://docs.silverflow.com/apidocs/latest/index.html)
- [Support](https://www.silverflow.com/contact)
- [Blog](https://www.silverflow.com/blog)
- [Privacy policy](https://www.silverflow.com/privacy)
- [Status](https://status.silverflow.com/)

## API Evangelist rating

**Kin Score 51.9** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 62.9 |
| Contract governance | 4.5 |
| Operational transparency | 57.9 |
| Developer ergonomics | 66.1 |
| Access clarity | 17.1 |

**Agent readiness 43.0** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | verified |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | verified |
| Event Surface Described | derived (partial credit) |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | partial |
| Operation Distinctiveness | distinct |

**Regulatory alignment 33.0** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/silverflow/](https://apis.io/providers/silverflow/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Silverflow: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/silverflow/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
