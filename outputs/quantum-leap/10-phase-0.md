# Phase 0 — Discovery & Dependency Resolution (Northwind Grid)

> **Phase orchestration — what's in/out of phase, dependencies, starting state.** Read this first to orient. Per-capability buildable specs live in `11-intents-0.md` (when present) — that's what you actually build against, one intent at a time.
> Phase duration: **sequence only — no committed duration**.

## Intent

- **For:** Rachel Doyle (Head of Communications) and the delivery team jointly.
- **Outcome:** Every one of the 8 source conflicts in the current requirements has a client-confirmed answer (or a conscious deferral), and the Grant Disbursement integration topology and payment gateway identity are confirmed, before any build phase starts.
- **Measured by:** Zero unresolved source conflicts carried into Phase 1; the integration topology and gateway identity for Phase 2 are named, not assumed.
- **Must not:** Must not start Phase 1 build work on an epic whose source-document conflict is still unresolved.

## Pre-decided (do not re-litigate)
- This engagement is scoped to the Community Care Fund rebuild only; the organization's separate Org Health Assessment findings are current-state context, not in-scope remediation.
- Defining what qualifies as a "special project" exception is a client-owned policy decision (Rachel + Legal), not a build task -- the build only provides enforcement/audit structure once that policy exists.

## Starting state

You are building into an **existing** Salesforce org, not a clean slate. Before creating anything, inventory the objects, fields, automation, and permission sets already present that relate to this phase, and reconcile them against `03-glossary-and-naming.md`. Extend what fits; make additive changes only; never modify or delete existing config without explicit user approval. See `04-org-rules.md` for the hard rules.

## Plan-mode questions (resolve before switching to Build mode)
- [ ] Confirm the 2-year re-application rule wording: does it block on a prior application being *funded*, or merely *submitted*? (blocks INT-004)
- [ ] Confirm whether every inbound email to the fund should create a case immediately, or whether the documented auto-reply-redirect behavior is intentional. (blocks INT-001)
- [ ] Confirm whether the Full Application should support partial save/resume, given its length, despite the current form's "no partial save" wording. (blocks INT-005)
- [ ] Confirm the panel's actual meeting cadence -- quarterly, or twice a year as the application form states. (blocks INT-010)
- [ ] Confirm the panel score editability/lock trigger point. (blocks INT-011)
- [ ] Confirm the Grant Disbursement integration topology: does Salesforce integrate directly with the EFT/payment gateway, or only with SAP? Confirm the gateway's identity. (blocks INT-008, the single largest open item in this engagement)
- [ ] Confirm whether Northwind Grid's Public Sector Grantmaking Salesforce license is available -- the architecture for Phases 1-3 assumes it, and the org does not have it provisioned today.
- [ ] Confirm the audit-report escalation model: a single tier (per the requirements document) or the three-tier chase/escalate/escalate sequence (per discovery). (blocks INT-013)

## Build-mode questions (ask only if the situation arises)
- None -- this phase is discovery only, no build work.

## Epics in scope for this phase

The phase brief is authoritative. Epics below are listed for cross-reference only — when an automation cites `(E04)`, this is what it refers to. For deeper epic narrative, see `90-epics-context.md`.

_(no epics tied to this phase)_

## Build targets — orchestration summary

These sections orient the build agent on the shape of the phase. Per-capability buildable detail (Outcome, Build target, Guardrails, Out of scope, Acceptance, Open questions) lives in `11-intents-0.md` per intent. When a section below cites `INT-NNN`, look up the intent there.

### Data model
No build work in this phase.

### Automation
No build work in this phase.

### UI & navigation
No build work in this phase.

### Security & access
No build work in this phase.

### Reports & dashboards
No build work in this phase.

### Sample data
Not applicable -- no build work in this phase.

### Data sources

Not applicable.

## Acceptance — user-outcome checks (phase-level)

Phase-level user-outcome claims a stakeholder would walk through to feel "Phase 0 is done." Run them in conversation with the user; mark `- [x]` only when the user agrees. Per-intent acceptance walkthroughs live in `11-intents-0.md`.

- [ ] Rachel Doyle has confirmed an answer (or a conscious deferral, logged as a gap) for each of the 8 source conflicts named above.
- [ ] The Grant Disbursement integration topology and payment gateway identity are confirmed in writing.
- [ ] The Public Sector Grantmaking license question is resolved.

## Acceptance — metadata-shaped checks (phase-level)

Phase-level metadata-shaped checks — queries the build agent runs against the target org without human help. Run via the Metadata skill (describe / tooling / SOQL). Per-intent acceptance is in `11-intents-0.md`.

- [ ] No metadata is deployed in this phase.

## Out of scope for Phase 0

If you find yourself needing to build any of these, stop and surface it — it belongs to a later phase or is explicitly excluded.

_(none surfaced in gaps.json — confirm with user during plan-mode review)_

## Dependencies and risks

**Dependencies:** None -- this phase gates Phases 1-3.

**Risks:** If source conflicts aren't resolved before Phase 1 starts, build teams inherit ambiguity and risk building against the wrong reading; if Public Sector Grantmaking licensing isn't confirmed, the base architecture pattern may need to pivot mid-build.

## Story citations covered in this phase

_(no user-story backlog captured for this phase)_

## Recipe boundary

When this phase is accepted, ask the user: *"Save this run as a recipe so we can repeat for Phase 1?"* The recipe should capture: the data-model decisions made above, the naming patterns confirmed in `03-glossary-and-naming.md`, and any Build-mode question resolutions that emerged.
