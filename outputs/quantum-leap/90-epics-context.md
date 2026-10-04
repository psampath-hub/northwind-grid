# Epics — Context Only — Northwind Grid

> Reference role: **background**, not load-bearing. The phase briefs are authoritative. This file dereferences epic IDs cited in phase briefs (e.g. `(E01)`) and provides scoping-stage context for trade-off reasoning.
>
> Do not plan against epics. Plan against `10-phase-N.md`.

## E01: Intake & Case Creation
Capture Expressions of Interest via the public web form or email, create cases, and triage them to a Community Marketing Lead.

_Confidence: Confirmed_

## E02: Eligibility Screening
Assess EOIs against the 2km-of-asset rule and the 2-year re-application policy (including the special-project exception), and set eligibility status. The precise 2-year rule wording (funded vs. submitted) is a documented source conflict -- see G0201 -- and this epic's scope is deliberately neutral on it pending client confirmation.

_Confidence: Assumed_
**KB sources:** [assumption: Geoscape confirmed as the CML's current mapping tool for the 2km boundary check, pending independent verification]

## E03: Full Application Capture
Invite eligible applicants to submit a Full Application with supporting documentation once their EOI is accepted.

_Confidence: Confirmed_

## E04: Review & Panel Decisioning
CML/Head-of-Communications assessment for applications under $50K; panel scoring, discussion, and decision for applications over $50K, including a review workspace for the 5 external panellists who currently lack Salesforce logins.

_Confidence: Assumed_
**KB sources:** [assumption: Experience Cloud login-based licensing fits the external panellists' review cadence -- validate panellist login frequency against the specific license model before costing]

## E05: Grant Disbursement
Initiate disbursement, validate and exchange vendor/payment data with SAP to eliminate duplicate vendor setup, and track settlement outcomes including a genuine 'Funded - Payment Held' state.

_Confidence: Assumed_
**KB sources:** [assumption: AP-owned EFT/payment gateway identity and integration surface are unconfirmed -- open question in the Discovery Brief]

## E06: Post-Funding Audit Tracking
Automated tracking of the 2-year post-funding audit-report due date, recipient chasing, and escalation to a CML and then the Head of Communications, replacing the current manual calendar reminder.

_Confidence: Assumed_

---

## Estimates and complexity drivers

These T-shirt sizes are scoping-stage planning estimates. They are **not** build instructions — the build agent should plan against the per-phase briefs, not against epic size. Sizes are included here for context only.

| Epic | T-shirt | Complexity drivers | Risks |
| --- | --- | --- | --- |
| E01: Intake & Case Creation | M | Dual-channel intake reconciliation (web form + email) with no deterministic de-dup rule defined yet; routing needs to hold up under seasonal spike volume. | Email-vs-web intake split mechanics are undocumented (open question); de-dup rule across channels not yet defined. |
| E02: Eligibility Screening | M | Two conflicting eligibility-rule readings in the source documents; case-to-Contact linking (E01) is non-deterministic, risking missed repeat-applicant detection. | Funded-vs-submitted 2-year rule conflict unresolved; Geoscape identity as the CML's current tool unverified; point-radius precision may prove insufficient for corridor-shaped assets. |
| E03: Full Application Capture | M | Co-applicant consent/dedup handling undefined; no-partial-save vs. standard save-for-later conflict unresolved; two-party attestation workflow has no grounded e-signature pattern. | Co-applicant PII capture with no independent consent step; e-signature formality bar unconfirmed with Legal. |
| E04: Review & Panel Decisioning | L | New license type to procure/budget; panel-scoring editability conflict requires a lock mechanism; prep-pack generation is a net-new automation; panel cadence (quarterly vs. twice-yearly) unresolved and affects licensing cost model. | External licensing is a real new cost not yet budgeted; score-editability conflict unresolved; panel cadence conflict affects license consumption math. |
| E05: Grant Disbursement | XL | Integration topology itself is unresolved (Salesforce-to-gateway vs. Salesforce-to-SAP-only); payment gateway identity unknown; no grounded KB pattern for ABN/registry validation; idempotency/double-payment risk on retries; three-team ownership (Comms/AP/Legal) with no named owner. | Architecture cannot be finalized until the integration topology and gateway identity are confirmed -- this is the single largest open risk in the engagement. |
| E06: Post-Funding Audit Tracking | M | Requirements document describes only a single escalation tier against the three-tier model actually wanted; whether structured intake (not just tracking) is in scope is unresolved; CML reassignment on staff turnover undefined. | If a structured intake mechanism or 5-year historical backfill is confirmed in scope, size moves up. |
