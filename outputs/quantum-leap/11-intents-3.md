# Intent Statements — Phase 3 (Northwind Grid)

> Reference role: the **load-bearing build target** for Phase 3. Each intent below is one capability — one firing trigger or user action, one outcome, one walkthrough. Build one at a time. The phase brief (`10-phase-3.md`) is orchestration; this file is what to build.
>
> **For architects:** walk these with the customer to assign priority and answer open questions. Edit `data/intents.json` (canonical) or this file directly — the next quantum-leap run re-renders from JSON.

## INT-010 — Give the external review panel a workspace to score applications

epic `E04` · priority _(unassigned)_ · confidence _Assumed_ · surface `experience-cloud`

### 1. Outcome

All 11 panel members -- including the 5 external reviewers who have no Salesforce access today -- score applications in one system, eliminating the spreadsheet-and-manual-transcription workaround that costs roughly 40 hours a year.

### 2. Build target

- The 5 external panellists log into an Experience Cloud site to review and score their assigned applications
- The system distributes the review pack to all 11 panellists ahead of each review cycle
- Each panellist scores an application against the program's defined criteria once those criteria are confirmed

### 3. Guardrails

- Must not require the external panellists to use a full internal Salesforce seat.

### 4. Out of scope

- Must not procure the Experience Cloud licenses -- that is a budget decision for Northwind Grid, not a build task.
- Must not finalize the review-pack generation format until panel cadence (quarterly vs. twice-yearly) is confirmed, since it changes batch size per cycle.

### 5. Acceptance

An external panellist who has never used Salesforce before logs into the review site, opens their assigned batch of applications, and scores one against the program's criteria -- without anyone on the fund team manually transcribing their score afterward.

### 7. Grounding

- **Requirements traced:** REQ-20, REQ-21, REQ-22

### Open questions

- [ ] **Q-011** — Does the panel meet quarterly or twice a year (March/September)? This affects review-pack batch size and the external licensing cost model. (Resolver: Rachel Doyle)
- [ ] **Q-012** — What are the program's five scoring criteria? Neither source document names them. (Resolver: Rachel Doyle + panel chair)
- [ ] **Q-013** — Has Northwind Grid budgeted for the Experience Cloud login-based licenses this requires? (Resolver: Rachel Doyle + Finance)

---

## INT-011 — Lock a panel score once the final decision is made, with a full change history

epic `E04` · priority _(unassigned)_ · confidence _Confirmed_ · surface `automation`

### 1. Outcome

A panel score cannot be changed after the point the panel's decision is final, and every change made before that point is permanently logged with who made it and when -- giving the fund a defensible record it cannot produce today.

### 2. Build target

- Every change to a panel score is logged with who made the change and when, before the lock point
- Once a case reaches its final decision stage, its panel scores become permanently locked against further edits

### 3. Guardrails

- Must not allow any score edit after the case reaches its final decision stage, regardless of role.
- Must preserve the full change history even for scores that were corrected before the lock point.

### 4. Out of scope

_(no explicit non-goals captured)_

### 5. Acceptance

A panellist corrects a scoring typo the day after a panel meeting but before the final decision is recorded -- the correction is logged with their name and timestamp. A week later, after the decision is final, no one -- including an administrator -- can edit that score.

### 7. Grounding

- **Requirements traced:** REQ-21

### Open questions

- [ ] **Q-014** — What case-status transition marks a decision as 'final' and should trigger the score lock -- the panel's accept/reject recording, or a separate confirmation step? (Resolver: Rachel Doyle + Legal)

---

## INT-012 — Route applications to the right review track by value and notify outcomes

epic `E04` · priority _(unassigned)_ · confidence _Assumed_ · surface `automation`

### 1. Outcome

An application over $50,000 goes to panel review; one under $50,000 is decided by the Community Marketing Lead or Head of Communications -- and the Head of Communications is alerted only on the applications that actually need her, not every case reassignment.

### 2. Build target

- Applications are routed to panel review or direct CML/Head-of-Communications review based on the requested amount at the confirmed decision point
- The Head of Communications is alerted only on applications above a confirmed value threshold, not on every reassignment
- Approved and rejected applicants are notified of the outcome by email

### 3. Guardrails

- Must not alert the Head of Communications on every case reassignment -- the current binary alert (about 40/week) is the problem this intent exists to fix.

### 4. Out of scope

- Must not resolve what dollar figure the $50,000 threshold is measured against (requested amount, adjusted recommendation, or per-stage value) -- confirm with the business first.

### 5. Acceptance

A $35,000 application and a $200,000 application are both decided in the same week. The Head of Communications receives an alert about the $200,000 one; the $35,000 decision proceeds through the Community Marketing Lead without generating one of her 40 weekly case-reassignment emails.

### 6. Dependencies

- **Internal (build first):** INT-010, INT-011

### 7. Grounding

- **Requirements traced:** REQ-16, REQ-19, REQ-23, REQ-24, REQ-25

### Open questions

- [ ] **Q-015** — What dollar figure determines the $50,000 panel-routing threshold, and what criteria should trigger a Head-of-Communications alert on an under-threshold application? (Resolver: Rachel Doyle)

---

## INT-013 — Track the 2-year post-funding audit report and escalate automatically

epic `E06` · priority _(unassigned)_ · confidence _Assumed_ · surface `automation`

### 1. Outcome

The fund's public 2-year audit-report commitment is tracked automatically -- chasing the recipient, then escalating to a Community Marketing Lead, then to the Head of Communications -- replacing a manual calendar reminder that already produces an estimated 20% late-or-missing rate and cannot answer the regulator's 'evidence of monitoring' request today.

- **Baseline:** ~20% of audit reports estimated late or missing (unverified estimate)
- **Target:** a defensible, system-tracked compliance rate, verified rather than estimated
- **Window:** first full 2-year cycle post go-live

### 2. Build target

- Each funded grant tracks its 2-year audit-report due date, starting from a confirmed trigger point for staged grants
- The system automatically chases the recipient, then escalates to the assigned Community Marketing Lead, then to the Head of Communications if still unresolved
- A case closes once an audit report is marked complete against a confirmed standard

### 3. Guardrails

- Must not implement only the single escalation tier described in the requirements document -- the three-tier chase/escalate/escalate sequence is the actual intent, pending final confirmation.

### 4. Out of scope

- Must not build a structured intake mechanism for how the audit report itself arrives -- this intent assumes a report lands on the case by whatever means and tracks/chases/escalates from there, unless Northwind Grid confirms otherwise.
- Must not migrate the 5 years of historical audit-report data in this phase unless explicitly confirmed in scope.

### 5. Acceptance

A grant funded two years ago has not had its audit report marked complete. The system has already chased the recipient automatically, escalated to the assigned Community Marketing Lead, and -- since neither resolved it -- escalated to the Head of Communications, all without a manual calendar reminder.

### 6. Dependencies

- **Internal (build first):** INT-009

### 7. Grounding

- **Requirements traced:** REQ-32, REQ-33, REQ-34

### Open questions

- [ ] **Q-016** — Who formally decides an audit report is 'complete', and against what standard, especially where expectations differ by grant size? (Resolver: Rachel Doyle)
- [ ] **Q-017** — For staged/milestone grants, does the 2-year audit clock start at first disbursement (signing) or the final disbursement (completion)? (Resolver: Rachel Doyle + Finance)

