# Engagement Intent — Northwind Grid

> Reference role: the *why* of the build. The phase briefs are how. When weighing a Plan-mode trade-off, weigh against this.
>
> If you need scoping context (current-state challenges, business impacts, confidence summary), see `93-scoping-context.md` (emitted only when scoping data is present).

## Intent

- **Outcome:** By the time the Community Care Fund reaches 1,000 applications a year, every eligibility decision, panel score, and audit follow-up is defensible on demand -- and a recipient gets paid without the SAP paperwork round-trip that costs weeks today.
- **Measured by:** _Discovery & Dependency Resolution:_ All 8 source conflicts have a client-confirmed answer or a conscious deferral logged in gaps.json; Public Sector Grantmaking licensing confirmed or an alternative base pattern selected; E05's integration topology (Salesforce-to-gateway vs. Salesforce-to-SAP-only) and gateway identity confirmed. · _Core Case Lifecycle Hardening:_ Every inbound application (web or email) reliably creates a case; the 2km check runs without manual lookup for the majority of cases; the Full Application's conditional-field logic (GST branching, amount reconciliation, document checklist) is built and validated. · _Grant Disbursement Integration:_ Entity-name and bank-detail validation catches mismatches before they reach SAP; a genuine 'Funded - Payment Held' status reflects real payment-gateway outcomes. · (+1 more — see the `10-phase-*` files)

## Engagement at a glance

Northwind Grid

- **Clouds in scope:** —
- **Phases planned:** 4
- **Target org:** `Northwind Grid CCF Sandbox` (sandbox)
- **Build allowed:** yes

## Vision

By the time the Community Care Fund reaches 1,000 applications a year, every eligibility decision, panel score, and audit follow-up is defensible on demand -- and a recipient gets paid without the SAP paperwork round-trip that costs weeks today.

## Value drivers (how to break ties between approach A and B)

- **Eliminate duplicate SAP vendor setup** — Removes an estimated 3+ weeks from the disbursement cycle and the entity-name mismatch bounce-back loop.
- **Eliminate manual panel-score transcription** — Removes the ~40 hours/year spent moving spreadsheet scores into the CRM before each panel meeting.
- **Defensible audit trail for overrides and scores** — Closes the specific compliance gap the regulator has already flagged in writing, and gives Rachel an answerable record for special-project decisions.
- **Real payment-status visibility** — Replaces a 3-week discovery lag (recipient calls to report a bounced payment) with a status Salesforce already knows.
- **A proper review workspace for external panellists** — Replaces the spreadsheet-and-manual-transcription workaround for the 5 of 11 panellists without Salesforce logins.

## Guiding principles

- **Every funding decision leaves a record** — Eligibility outcomes, special-project exceptions, and panel scores are logged with reasoning, not just an outcome -- directly answers the regulator's 'evidence of monitoring' ask.
- **External reviewers work inside the platform** — No spreadsheet round-trips for people without a Salesforce seat; licensing exists for exactly this pattern.
- **Fix the system, not just the policy** — Salesforce builds the enforcement and audit structure; Rachel and Legal own defining the 'special project' policy itself -- the client's explicit choice.
- **Configure before customize** — Lean on standard Salesforce grantmaking patterns rather than bespoke build, consistent with the platform-health recommendations already on file for this org.
