# Phase 2 — Grant Disbursement Integration (Northwind Grid)

> **Phase orchestration — what's in/out of phase, dependencies, starting state.** Read this first to orient. Per-capability buildable specs live in `11-intents-2.md` (when present) — that's what you actually build against, one intent at a time.
> Phase duration: **sequence only — no committed duration**.

## Intent

- **For:** Finance/Accounts Payable processing grant payments; recipients awaiting disbursement; Rachel Doyle's stated #1 priority.
- **Outcome:** A validated Salesforce-to-SAP data exchange eliminates duplicate vendor setup and the entity-name mismatch that bounces roughly a third of approved applications, and the "Funded -- Payment Held" status reflects a real, confirmed payment-gateway outcome.
- **Measured by:** Mismatch bounce-back rate reduced toward zero; disbursement cycle time shortened by removing the duplicate setup round-trip; no case silently shows "Funded" after an actual payment failure.
- **Must not:** Must not build the settlement-outcome integration (INT-008) before the integration topology and gateway identity are confirmed in Phase 0 -- this is the single largest open risk in the engagement.

## Pre-decided (do not re-litigate)
- A validation callout against the official business registry runs before any payment request reaches SAP, not after.
- A record-triggered Flow generates the payment instruction and sends it to SAP; SAP's returned payment identifier writes back onto the case.
- Staged/milestone disbursements get their own tracking structure, migrating the ~20 existing spreadsheet-tracked grants before go-live.

## Starting state (from Core Case Lifecycle Hardening)

You should find these already deployed in the sandbox:
- **Core Case Lifecycle Hardening outcome:** Every inbound application (web or email) reliably creates a case; the 2km check runs without manual lookup for the majority of cases; the Full Application's conditional-field logic (GST branching, amount reconciliation, document checklist) is built and validated.

## Plan-mode questions (resolve before switching to Build mode)
- [ ] What validation ruleset is intended for the entity-name check -- exact match, fuzzy tolerance, or an interactive confirmation with the applicant? (INT-006)
- [ ] Does Salesforce derive GL/cost-centre coding automatically, or does Finance/AP append it manually? (INT-007)
- [ ] Does Salesforce integrate directly with the EFT/payment gateway, or only with SAP? What is the gateway's identity? (INT-008 -- this should already be confirmed from Phase 0; re-verify before build)

## Build-mode questions (ask only if the situation arises)
- Confirm the SAP integration's exact API surface (REST/SOAP, auth model) before building INT-007.
- Confirm the registry lookup service's API before building INT-006.

## Epics in scope for this phase

The phase brief is authoritative. Epics below are listed for cross-reference only — when an automation cites `(E04)`, this is what it refers to. For deeper epic narrative, see `90-epics-context.md`.

- **E05: Grant Disbursement** — Initiate disbursement, validate and exchange vendor/payment data with SAP to eliminate duplicate vendor setup, and track settlement outcomes including a genuine 'Funded - Payment Held' state.

## Build targets — orchestration summary

These sections orient the build agent on the shape of the phase. Per-capability buildable detail (Outcome, Build target, Guardrails, Out of scope, Acceptance, Open questions) lives in `11-intents-2.md` per intent. When a section below cites `INT-NNN`, look up the intent there.

### Data model
Bank account, ABN, and tax reference fields carry Shield Platform Encryption. A staged-disbursement data model ties multiple payment stages to a single grant record. See INT-006 through INT-009 for load-bearing detail.

### Automation
Pre-submission registry validation (INT-006) gates the payment-request Flow (INT-007). Settlement-outcome tracking (INT-008) and milestone reminders for staged grants (INT-009) complete the disbursement lifecycle.

### UI & navigation
No new user-facing surface in this phase beyond status visibility on the existing case layout.

### Security & access
Shield Platform Encryption applies to all bank/ABN/tax fields given their sensitivity and the absence of a stated data-residency policy.

### Reports & dashboards
Not load-bearing for this phase.

### Sample data
Recommend test cases covering: a clean entity-name match, a deliberate mismatch, a staged-grant schedule, and a simulated payment failure.

### Data sources

| Source | Feeds | Status |
|---|---|---|
| Official business registry (ABN/ABR) | INT-006 | Not yet confirmed as an integration target |
| SAP vendor master | INT-007, INT-008 | Existing system, integration surface unconfirmed |
| AP-owned EFT/payment gateway | INT-008 | Identity unconfirmed |
| Existing staged-payment spreadsheet | INT-009 | Live today, on one person's laptop |

## Acceptance — user-outcome checks (phase-level)

Phase-level user-outcome claims a stakeholder would walk through to feel "Phase 2 is done." Run them in conversation with the user; mark `- [x]` only when the user agrees. Per-intent acceptance walkthroughs live in `11-intents-2.md`.

- [ ] An applicant with a mismatched entity name is prompted to correct it before their application proceeds to disbursement, not after SAP rejects it.
- [ ] A failed payment shows "Funded -- Payment Held" on the case within the same business day, with the assigned Community Marketing Lead notified.
- [ ] A migrated staged grant generates a milestone reminder ahead of its next due date.

## Acceptance — metadata-shaped checks (phase-level)

Phase-level metadata-shaped checks — queries the build agent runs against the target org without human help. Run via the Metadata skill (describe / tooling / SOQL). Per-intent acceptance is in `11-intents-2.md`.

- [ ] SAP integration deployed and tested end-to-end against a sandbox vendor-setup flow.
- [ ] Registry validation callout deployed and tested against both a match and a deliberate mismatch.
- [ ] All ~20 existing staged grants migrated and verified against the source spreadsheet.

## Out of scope for Phase 2

If you find yourself needing to build any of these, stop and surface it — it belongs to a later phase or is explicitly excluded.

_(none surfaced in gaps.json — confirm with user during plan-mode review)_

## Dependencies and risks

**Dependencies:** Phase 0's resolution of the integration topology and payment gateway identity -- this phase cannot be built with confidence until that's confirmed. Isolated as its own phase given its size (XL) and because it carries the single largest open risk in the engagement.

**Risks:** Still Unknown confidence entering this phase if Phase 0 doesn't fully close the topology question; three-team ownership (Comms/AP/Legal) with no named accountable owner; no grounded KB pattern yet for the ABN/registry validation mechanism.

## Story citations covered in this phase

_(no user-story backlog captured for this phase)_

## Recipe boundary

When this phase is accepted, ask the user: *"Save this run as a recipe so we can repeat for Phase 3?"* The recipe should capture: the data-model decisions made above, the naming patterns confirmed in `03-glossary-and-naming.md`, and any Build-mode question resolutions that emerged.
