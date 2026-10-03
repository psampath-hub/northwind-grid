# Northwind Grid — Community Care Fund: Delivery Plan

Benchmark-based program duration: 13–28 weeks (top-down from engagement shape: 6 epics, 4 Medium/1 Large/1 Extra-Large, Experience Cloud standard-portal pattern, +15% for regulated/audit-trail requirements, +10% for custom public-facing forms, +10% for one unresolved integration unknown). Benchmark-based, not a commitment.

*This duration range is a benchmark-derived estimate based on general implementation patterns and is not a committed delivery timeline. Actual duration depends on team composition, client responsiveness, data quality, and scope changes discovered during delivery.*

The high end of the range is driven by two open items:
- **Grant Disbursement's integration approach and payment gateway identity** are both unconfirmed. Resolving this tightens the range the most.
- **Experience Cloud licensing for the 5 external panellists** needs a procurement decision — this sits outside build time but inside the wall-clock range below.

## Named Dependencies

Two dependencies gate this plan and are carried as named Phase 0 items rather than assumptions buried in a phase description:

1. **Public Sector Grantmaking module license.** The architecture in this plan rests on the Public Sector Grantmaking module (Platform + Experience Cloud) — Northwind Grid's org does not have this provisioned today. Confirming it is a Phase 0 exit criterion; if it's unavailable, the base pattern needs to pivot before Phase 1 starts.
2. **Grant Disbursement integration topology and gateway identity.** Whether Salesforce integrates directly with the payment gateway or only with SAP, and what that gateway actually is, is unresolved. Phase 2 (Grant Disbursement) cannot be sized or built with confidence until this is confirmed — it's the single largest driver of this plan's duration range.

## Phase Sequence

**Phase 0 — Discovery & Dependency Resolution** *(sequence position 1 of 4)*
Resolves the 8 source conflicts surfaced in requirements and the two named dependencies above, before any build phase starts.

**Phase 1 — Core Case Lifecycle Hardening** *(sequence position 2 of 4)* — E01, E02, E03
Closes the reputational-risk gap in the email intake path, automates the 2km eligibility check, and hardens Full Application capture.
*Depends on: Phase 0's resolution of the re-application rule wording and the no-partial-save conflict.*

**Phase 2 — Grant Disbursement Integration** *(sequence position 3 of 4)* — E05
Delivers Northwind Grid's #1 stated priority: eliminating duplicate SAP vendor setup.
*Depends on: Phase 0's resolution of the integration topology and gateway identity. Held as its own phase given its size and risk.*

**Phase 3 — Panel Integrity, External Review & Audit Tracking** *(sequence position 4 of 4)* — E04, E06
Delivers tamper-evident panel scoring, a reviewer workspace for the 5 external panellists, and automated audit-report tracking.
*Depends on: Experience Cloud license procurement for the external panellists, kicked off during Phase 0 so it doesn't block this phase.*

## Consolidated Risk Table

| Risk | Phase | Mitigation |
|---|---|---|
| Public Sector Grantmaking license unavailable or delayed | 0 | Confirm with Northwind Grid's account team before Phase 1 kicks off |
| Integration topology/gateway unresolved | 0, 2 | Direct confirmation needed from Rachel and Accounts Payable |
| External reviewer licensing not budgeted | 3 | Start procurement conversation in Phase 0 given its lead time |
| Panel cadence conflict (quarterly vs. twice-yearly) | 3 | Affects licensing cost model; confirm before costing |
| Three-team ownership gap (Comms/AP/Legal) on disbursement | 2 | No named accountable owner today; flag for the client to resolve |
| Cross-channel de-duplication undefined | 1 | Design decision needed before intake automation is finalized |

## Team

The disciplines and named roster to deliver this — with defensible counts, per lane — come from `estimate`.

---

*Prepared using Scopezilla. Pricing requires a validated rate via `commercials`.*
