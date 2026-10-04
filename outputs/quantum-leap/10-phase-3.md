# Phase 3 — Panel Integrity, External Review & Audit Tracking (Northwind Grid)

> **Phase orchestration — what's in/out of phase, dependencies, starting state.** Read this first to orient. Per-capability buildable specs live in `11-intents-3.md` (when present) — that's what you actually build against, one intent at a time.
> Phase duration: **sequence only — no committed duration**.

## Intent

- **For:** The 11-person review panel (6 internal, 5 external); Rachel Doyle; recipients awaiting their 2-year audit-report obligation.
- **Outcome:** Panel scoring is tamper-evident and defensible, all 11 panellists score through one system instead of a spreadsheet workaround, and the 2-year audit-report commitment is tracked and escalated automatically.
- **Measured by:** No undetected post-decision score changes; the ~40 hours/year of manual score transcription eliminated; the audit-report compliance rate becomes system-verified rather than estimated.
- **Must not:** Must not finalize the external-reviewer licensing build (INT-010) until Northwind Grid confirms panel cadence and budget; must not build only a single-tier audit escalation when the actual intent is three tiers (INT-013).

## Pre-decided (do not re-litigate)
- Field change history logs every panel-score edit before the lock point; a validation rule blocks any edit after the case reaches its final decision stage.
- Applications over $50,000 go to panel review; applications under that threshold go to the Community Marketing Lead or Head of Communications.
- The Head of Communications is alerted only above a confirmed value threshold, not on every case reassignment.

## Starting state (from Grant Disbursement Integration)

You should find these already deployed in the sandbox:
- **Grant Disbursement Integration outcome:** Entity-name and bank-detail validation catches mismatches before they reach SAP; a genuine 'Funded - Payment Held' status reflects real payment-gateway outcomes.

## Plan-mode questions (resolve before switching to Build mode)
- [ ] Does the panel meet quarterly or twice a year? (INT-010 -- should be confirmed from Phase 0; re-verify before build)
- [ ] What are the program's five scoring criteria? (INT-010)
- [ ] Has Northwind Grid budgeted for the external panellist Experience Cloud licenses? (INT-010)
- [ ] What case-status transition should trigger the panel-score lock? (INT-011)
- [ ] What dollar figure determines the $50,000 panel-routing threshold? (INT-012)
- [ ] Who formally decides an audit report is "complete," and against what standard? (INT-013)
- [ ] For staged grants, does the 2-year audit clock start at first disbursement or final disbursement? (INT-013)

## Build-mode questions (ask only if the situation arises)
- Confirm the Experience Cloud license type and provisioning before building INT-010.

## Epics in scope for this phase

The phase brief is authoritative. Epics below are listed for cross-reference only — when an automation cites `(E04)`, this is what it refers to. For deeper epic narrative, see `90-epics-context.md`.

- **E04: Review & Panel Decisioning** — CML/Head-of-Communications assessment for applications under $50K; panel scoring, discussion, and decision for applications over $50K, including a review workspace for the 5 external panellists who currently lack Salesforce logins.
- **E06: Post-Funding Audit Tracking** — Automated tracking of the 2-year post-funding audit-report due date, recipient chasing, and escalation to a CML and then the Head of Communications, replacing the current manual calendar reminder.

## Build targets — orchestration summary

These sections orient the build agent on the shape of the phase. Per-capability buildable detail (Outcome, Build target, Guardrails, Out of scope, Acceptance, Open questions) lives in `11-intents-3.md` per intent. When a section below cites `INT-NNN`, look up the intent there.

### Data model
A new Panel Score child object (one record per panellist per application) carries Field History Tracking. A due-date field on Case drives the audit-tracking sequence. See INT-010, INT-011, INT-013 for load-bearing detail.

### Automation
Score-lock validation (INT-011) and value-based routing/notification (INT-012) govern the review stage. A scheduled Flow drives the three-tier audit chase/escalate sequence (INT-013).

### UI & navigation
An Experience Cloud "Grantmaking" reviewer workspace gives the 5 external panellists a scoring interface, replacing the spreadsheet (INT-010).

### Security & access
Login-based Experience Cloud licensing for the 5 external panellists, scoped to the review workspace only.

### Reports & dashboards
The review pack (application summaries for an upcoming panel cycle) is generated and distributed to all 11 panellists (INT-010).

### Sample data
Recommend a test panel cycle with a mix of under- and over-threshold applications, and a deliberately contentious score correction before and after the lock point.

### Data sources

Not applicable -- no external data sources beyond the Case and Panel Score objects already in the org.

## Acceptance — user-outcome checks (phase-level)

Phase-level user-outcome claims a stakeholder would walk through to feel "Phase 3 is done." Run them in conversation with the user; mark `- [x]` only when the user agrees. Per-intent acceptance walkthroughs live in `11-intents-3.md`.

- [ ] An external panellist logs into the review site and scores an application without anyone transcribing it afterward.
- [ ] A score corrected before the final decision is logged with who and when; the same score cannot be edited a week later.
- [ ] The Head of Communications receives an alert on a $200,000 application but not on a routine $35,000 one.
- [ ] A grant two years overdue on its audit report has already been chased, escalated to a Community Marketing Lead, and escalated again to the Head of Communications -- without a manual calendar reminder.

## Acceptance — metadata-shaped checks (phase-level)

Phase-level metadata-shaped checks — queries the build agent runs against the target org without human help. Run via the Metadata skill (describe / tooling / SOQL). Per-intent acceptance is in `11-intents-3.md`.

- [ ] Experience Cloud reviewer site deployed and tested with at least one external test user.
- [ ] Score-lock validation rule deployed and verified against a pre- and post-decision edit attempt.
- [ ] Three-tier escalation Flow deployed and tested against a simulated overdue audit report.

## Out of scope for Phase 3

If you find yourself needing to build any of these, stop and surface it — it belongs to a later phase or is explicitly excluded.

_(none surfaced in gaps.json — confirm with user during plan-mode review)_

## Dependencies and risks

**Dependencies:** Experience Cloud login-based license procurement for the 5 external panellists -- has its own lead time outside PS build time, so kicking off procurement during Phase 0 avoids it blocking this phase.

**Risks:** Panel cadence (quarterly vs. twice-yearly) affects the licensing cost model; whether structured audit-report intake is in scope (vs. tracking/escalation only) is still open.

## Story citations covered in this phase

_(no user-story backlog captured for this phase)_

## Recipe boundary

When this phase is accepted, ask the user: *"Save this run as a recipe so we can repeat for Phase —?"* The recipe should capture: the data-model decisions made above, the naming patterns confirmed in `03-glossary-and-naming.md`, and any Build-mode question resolutions that emerged.
