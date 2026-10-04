# Phase 1 — Core Case Lifecycle Hardening (Northwind Grid)

> **Phase orchestration — what's in/out of phase, dependencies, starting state.** Read this first to orient. Per-capability buildable specs live in `11-intents-1.md` (when present) — that's what you actually build against, one intent at a time.
> Phase duration: **sequence only — no committed duration**.

## Intent

- **For:** Applicants submitting Expressions of Interest and Full Applications; Community Marketing Leads triaging and screening them.
- **Outcome:** Every inbound application attempt reliably produces a trackable case, the 2km eligibility check runs without a manual external-tool lookup, and the Full Application captures its real conditional-field logic correctly.
- **Measured by:** Zero lost-application incidents from the email intake path; a materially reduced eligibility-dispute rate on the 2km check; the Full Application's GST/amount/co-applicant logic captured without guesswork.
- **Must not:** Must not ship intake or eligibility automation that silently reproduces the current manual process's failure modes (lost emails, undocumented eligibility reasoning).

## Pre-decided (do not re-litigate)
- Email-to-Case creates a system-of-record case from every inbound email immediately; a courtesy redirect to the web form never substitutes for case creation.
- The eligibility outcome stores the specific criteria evaluated (or override basis) alongside the pass/fail status, not just the status itself.
- Full Application's GST-conditional funding fields and amount-changed-since-EOI reconciliation are built as conditional logic, not a flat form.
- The co-applicant attestation is a typed-name-and-checkbox pattern, not a dedicated e-signature product.

## Starting state

You are building into an **existing** Salesforce org, not a clean slate. Before creating anything, inventory the objects, fields, automation, and permission sets already present that relate to this phase, and reconcile them against `03-glossary-and-naming.md`. Extend what fits; make additive changes only; never modify or delete existing config without explicit user approval. See `04-org-rules.md` for the hard rules.

## Plan-mode questions (resolve before switching to Build mode)
- [ ] What are all the email addresses applicants currently use to reach the fund? (INT-001)
- [ ] What should count as a "likely duplicate" across intake channels? (INT-002)
- [ ] Is Geoscape confirmed as the authoritative asset-location data source for the 2km check? (INT-003)
- [ ] What margin near the 2km boundary should trigger manual review instead of an automatic pass/fail? (INT-003)
- [ ] Does the 2-year re-application rule block on "funded" or "submitted"? (INT-004 -- carried from Phase 0 if still unresolved)
- [ ] Should the Full Application support save/resume? (INT-005 -- carried from Phase 0 if still unresolved)

## Build-mode questions (ask only if the situation arises)
- Confirm the exact field-to-case mapping for the public web form before building INT-001.
- Confirm the asset-location data format and location before building INT-003.

## Epics in scope for this phase

The phase brief is authoritative. Epics below are listed for cross-reference only — when an automation cites `(E04)`, this is what it refers to. For deeper epic narrative, see `90-epics-context.md`.

- **E01: Intake & Case Creation** — Capture Expressions of Interest via the public web form or email, create cases, and triage them to a Community Marketing Lead.
- **E02: Eligibility Screening** — Assess EOIs against the 2km-of-asset rule and the 2-year re-application policy (including the special-project exception), and set eligibility status. The precise 2-year rule wording (funded vs. submitted) is a documented source conflict -- see G0201 -- and this epic's scope is deliberately neutral on it pending client confirmation.
- **E03: Full Application Capture** — Invite eligible applicants to submit a Full Application with supporting documentation once their EOI is accepted.

## Build targets — orchestration summary

These sections orient the build agent on the shape of the phase. Per-capability buildable detail (Outcome, Build target, Guardrails, Out of scope, Acceptance, Open questions) lives in `11-intents-1.md` per intent. When a section below cites `INT-NNN`, look up the intent there.

### Data model
Case remains the backbone object for applications. New fields capture intake channel, eligibility criteria evaluated with reasoning, and measured proximity distance. See INT-001, INT-003, INT-004 for the load-bearing detail.

### Automation
Email-to-Case and web-form intake both funnel into Case creation (INT-001). Capacity-based routing replaces static assignment rules (INT-002). The 2km proximity check and 2-year re-application check run automatically with recorded reasoning (INT-003, INT-004).

### UI & navigation
The public Full Application form is built with conditional branching for GST status, amount-changed-since-EOI, and the document checklist (INT-005). No custom Lightning Web Components required for this phase.

### Security & access
The Experience Cloud guest-user profile for the public intake forms is locked to form submission only, with no broader object access.

### Reports & dashboards
Not load-bearing for this phase.

### Sample data
Not yet defined -- recommend a small set of test applications covering each intake channel, a borderline-eligible location, and a GST-registered vs. non-registered applicant.

### Data sources

| Source | Feeds | Status |
|---|---|---|
| Public EOI/Full Application web forms | INT-001, INT-005 | Live today |
| Fund email inboxes | INT-001 | Live today, mechanics undocumented |
| Asset location data (likely Geoscape) | INT-003 | Unconfirmed as source |

## Acceptance — user-outcome checks (phase-level)

Phase-level user-outcome claims a stakeholder would walk through to feel "Phase 1 is done." Run them in conversation with the user; mark `- [x]` only when the user agrees. Per-intent acceptance walkthroughs live in `11-intents-1.md`.

- [ ] An applicant emailing the fund directly sees a case created without acting on any redirect.
- [ ] A Community Marketing Lead sees the measured proximity distance on a borderline application, not just pass/fail.
- [ ] An applicant whose requested amount changed since EOI is prompted to explain the change and the quotes are checked against it.

## Acceptance — metadata-shaped checks (phase-level)

Phase-level metadata-shaped checks — queries the build agent runs against the target org without human help. Run via the Metadata skill (describe / tooling / SOQL). Per-intent acceptance is in `11-intents-1.md`.

- [ ] Email-to-Case routing addresses deployed and tested against at least one real inbox.
- [ ] Capacity-based assignment rule deployed and verified against a multi-case test batch.
- [ ] Proximity-check automation deployed against a test asset-location dataset.

## Out of scope for Phase 1

If you find yourself needing to build any of these, stop and surface it — it belongs to a later phase or is explicitly excluded.

_(none surfaced in gaps.json — confirm with user during plan-mode review)_

## Dependencies and risks

**Dependencies:** Phase 0's resolution of the funded-vs-submitted rule (E02) and the no-partial-save conflict (E03).

**Risks:** De-duplication logic across intake channels is still undefined; Geoscape's identity as the CML's actual mapping tool is unverified.

## Story citations covered in this phase

_(no user-story backlog captured for this phase)_

## Recipe boundary

When this phase is accepted, ask the user: *"Save this run as a recipe so we can repeat for Phase 2?"* The recipe should capture: the data-model decisions made above, the naming patterns confirmed in `03-glossary-and-naming.md`, and any Build-mode question resolutions that emerged.
