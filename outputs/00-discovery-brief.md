# Northwind Grid — Discovery Brief

**Engagement:** Community Care Fund (CCF) — Salesforce enhancement
**Prepared:** 1 October 2026
**Sources:** Discovery interview with Head of Communications (12 June 2026), Community Care Fund High Level Requirements workbook, CCF Expression of Interest and Full Application forms, Northwind Grid Salesforce Org Health Assessment (9 June 2026), Northwind Grid brand kit

---

## Executive Summary

Northwind Grid, a state-owned Australian electricity network operator, runs the Community Care Fund — a grant program for communities near its infrastructure — on Salesforce. The program has outgrown its process: volume has grown 8x since 2019 (50 → ~400 applications/year) and the board has approved a further doubling to ~1,000/year by 2028. The current Salesforce implementation lacks the governance, integration, and audit-trail depth needed to support that scale, creating real regulatory exposure (an unresolved audit-tracking gap, no defensible record for discretionary "special project" exceptions, and no tamper-evident panel-scoring trail). This engagement scopes the CCF rebuild to close those gaps. A separate, broader Salesforce Org Health Assessment of Northwind Grid's platform (governance, security, performance) is treated as current-state context the design should respect, not as scope to remediate directly.

## Company and Industry Context

Northwind Grid is a state-owned enterprise managing Australia's national electricity grid. As a regulated utility running critical infrastructure through communities, it operates the Community Care Fund as a "good neighbour" program — one-off grants to community organisations within 2km of its transmission/substation assets. The program launched in 2019 as a small, informal initiative (~50 applications/year via shared inbox and spreadsheet) and has scaled sharply: ~400 applications/year today, with individual grants ranging from ~$2,000 (local sporting club) to $2.3M (major capital works). The board has approved doubling the fund over the next three years, targeting ~1,000 applications/year by 2028.

Northwind Grid's broader Salesforce platform (migrated from Microsoft Dynamics, delivered with implementation partner DigiTech) spans Platform, Marketing Cloud, and Data 360, with Mulesoft/real-time analytics under consideration. A June 2026 Salesforce Professional Services Org Health Assessment rated the org's security posture "Needs Attention" (63% Health Check non-compliance) and flagged 45 findings across governance, performance, data model, and integration architecture — most notably the complete absence of a Center of Excellence or Design Authority to govern the platform as it scales. That assessment is current-state context for this engagement, not its scope.

## Current vs. Target Salesforce Landscape

**Current state.** The CCF process already runs substantially on Salesforce (per the High Level Requirements workbook, treated as the as-built spec with known gaps): applicants submit an Expression of Interest via a public web form or email; cases are created and manually triaged to one of 4 Community Marketing Leads (CMLs); eligibility is checked against a 2km-of-asset rule (manually, against a separate — likely Geoscape — mapping tool, with no boundary layer in the CRM); eligible applicants submit a Full Application; applications under $50K are decided by the CML/Head of Communications, while applications over $50K go to an 11-person panel (6 internal, 5 external) that meets twice a year (March and September, per the application form — the interview described this as quarterly, a discrepancy worth confirming with the client); approved grants are disbursed via a manual, duplicated SAP vendor-setup process; and recipients are expected to submit post-funding audit reports within two years, chased via a manual calendar reminder.

**Target state — Rachel Doyle's (Head of Communications) three stated priorities:**

1. **Integrated vendor setup** — Salesforce and SAP exchanging validated entity-name and bank-detail data, eliminating the duplicate manual setup that causes ~1/3 of applications to bounce on entity-name mismatches and adds 3+ weeks to disbursement.
2. **Real audit tracking** — a system-driven mechanism that knows when a grant was paid, tracks when the audit report is due, chases the recipient automatically, and escalates to a CML and then to Rachel if overdue (replacing a manual calendar reminder that is already producing an estimated ~20% late/missing rate).
3. **Panel scoring integrity** — an immutable record of who scored what and when, with no undetected post-decision edits — both a compliance requirement (the current field just overwrites, destroying prior values) and a fairness requirement.

Additional implied needs surfaced in discovery (not yet explicitly scoped): value-based alert filtering so only high-value cases require Rachel's direct review; a CRM-native boundary/geofencing layer for the 2km rule (likely via the existing Geoscape integration); a defensible structure for "special project" exceptions (a policy decision for Rachel/Legal to make — Salesforce's role is to enforce and log whatever they land on, not to design the policy); and a genuine "Funded – Payment Held" status reflecting real payment-gateway outcomes.

## Project Scope and Objectives

**In scope:** the Community Care Fund application lifecycle end-to-end — intake (EOI + Full Application), eligibility screening, CML and panel review, disbursement (CRM-to-SAP integration), and post-funding audit tracking — targeting the three priority fixes above plus the supporting capabilities in the High Level Requirements workbook (Capabilities 1–5, REQ-1 through REQ-34).

**Out of scope (for now):** the broader Org Health Assessment's 45 platform-wide findings (governance framework, Center of Excellence, security hardening, performance remediation) are current-state context informing design choices — e.g., the CCF design should not add to the sharing-rule sprawl already flagged (SV-03), and any new SAP/Geoscape credential work should follow the secrets-handling fix already recommended (IA-04) — but are not themselves deliverables of this engagement unless the client explicitly pulls them in.

**Explicitly deferred (client-owned policy decision):** formally defining what qualifies as a "special project" exception. Today this is undocumented, held in Rachel's personal, unshared document, and was applied inconsistently by an acting director during her leave. This is a business/policy decision for Rachel and Legal to resolve — Salesforce's role is to provide the structure (criteria fields, approval workflow, logged rationale) once that policy exists, not to define the policy itself.

## Users and Roles 

/notes: some grounding issue on this

- **Rachel Doyle** — Head of Communications; CCF business owner; sole decision-maker on "special project" exceptions; reviews reassigned cases.
- **4 Community Marketing Leads (CMLs)** — 3 Melbourne, 1 Perth; handle eligibility checks and application chasing; two were on medical leave during last year's volume spike.
- **Rachel's PA** — biweekly pipeline review; maintains the staged-payment tracking spreadsheet; runs the monthly audit-overdue report.
- **Senior Advisor** — ~15% of time on MP constituent-escalation briefing notes.
- **11-person approval panel** — 6 internal (Rachel, General Counsel, Operations, ESG, Finance, MD's Chief of Staff) + 5 external (community sector reps, an academic, an Indigenous liaison). The 5 external panellists lack Salesforce logins and currently score via spreadsheet, manually transcribed back into the CRM (~8–10 hours per panel, ~40 hours/year of data entry).
- **Legal / General Counsel** — signs funding agreements; reviews ASIC/ABN documentation for grants over $100K.
- **Finance / Accounts Payable** — owns SAP vendor-master setup and the EFT/payments platform (name not yet identified).

## Data and Compliance Considerations

Regulator scrutiny of Northwind Grid's community-funding decisions is increasing, including explicit questions about the public two-year audit-report follow-up commitment. A recent compliance review asked for "evidence of monitoring"; the organisation could not fully account for 5 years of audit-report status and sent the regulator a letter acknowledging the gaps. Rachel anticipates a possible Senate estimates question about the basis for funding decisions and currently cannot produce a defensible record for "special project" eligibility overrides. The panel-scoring process has no audit trail today — scores are editable post-decision with no change history — a live compliance exposure. Legal requires ASIC/ABN check documentation for grants over $100,000 as a governance control before disbursement. No data-residency or PII-handling requirements for applicant/recipient bank details were discussed in the interview and should be confirmed.

## Integrations

- **Salesforce ↔ SAP (vendor master)** — no integration today; the #1 priority fix. Causes duplicate manual setup (3-week to 3-week-plus delays against a 5-day SLA) and entity-name mismatch rejections on an estimated ~1/3 of applications.
- **Salesforce ↔ EFT/payments gateway** — owned by Accounts Payable; platform not yet identified. Payment success/failure status does not flow back to Salesforce, so the CRM shows "Funded" even after a bounced payment — a hard status gap Rachel explicitly wants closed with a real "Funded – Payment Held" state.
- **Public website EOI form → Salesforce case creation** — partially automated for an estimated ~60% of applications submitted via the web form; the remainder arrive by email with inconsistent manual case creation. Exact automation mechanics for the web-form path are unconfirmed.
- **Geoscape (geocoding/address data)** — confirmed as an existing org-wide integration (per the Org Health Assessment) and, per the client's confirmation during this discovery session, likely the same tool CMLs use separately today for the 2km boundary eligibility check — currently disconnected from the CRM, requiring a manual eyeball comparison responsible for ~40% of eligibility disputes.

## Research Findings and Market Context

*Confidence note: live web search was unavailable in this session; the points below combine general industry knowledge (unverified against a primary source — flagged accordingly) with Salesforce central-KB grounding (cited).*

- Australian network utilities commonly run community-benefit-sharing or grant programs tied to infrastructure proximity, often shaped by regulatory and social-licence expectations. Typical governance patterns include two-stage intake (EOI → full application), mixed internal/external assessment panels, tiered approval thresholds, documented conflict-of-interest declarations, and audit trails for funding decisions given the use of public money. **[Unverified — general pattern, confirm against Northwind Grid's actual regulatory framework.]**
- Salesforce's standard pattern for this type of program is **Nonprofit Cloud for Grantmaking / Public Sector Grantmaking** — covering the full lifecycle from application intake through award, disbursement, and progress reporting `[KA-9728]`, with a **Form Framework** supporting multi-stage applications and a consolidated reviewer workspace `[KA-9719]`.
- For the 5 external panellists without Salesforce licenses: the standard PS pattern is an **Experience Cloud "Grantmaking" site**, which explicitly supports external reviewers scoring applications through a portal rather than a spreadaheet workaround `[KA-9511]`, using **login-based Experience Cloud licensing** (metered by daily unique logins — well suited to panellists who review only twice a year) rather than full internal seats `[KA-10444]`, `[KA-0417]`.
- No KB atom in this pass directly addressed SAP-to-Salesforce disbursement integration patterns — recommend a targeted architecture-decision query on this before design.
- General expectation for Australian government/utility grant disbursements is an ABN validity check before payment and an auditable per-decision trail — consistent with what Rachel described as a current gap. **[Unverified against a primary regulatory source — confirm specific obligations with the client's compliance team.]**

## Open Questions

1. **Panel cadence discrepancy** — the interview describes quarterly panel meetings; the EOI application form states twice-yearly (March/September). The client has indicated the form is likely the accurate source, but this should be explicitly confirmed before it drives any cadence-dependent design (e.g., SLA timers, alert cadence).
2. **Website → case-creation mechanics** — the exact automation behind the ~60% of EOIs submitted via the public web form is undocumented; needs confirmation of what's automated today versus assumed.
3. **EFT/payment gateway identity** — the AP-owned payment platform referenced in the interview was never named. Needed to scope the Salesforce-side integration for the "Funded – Payment Held" status fix.
4. **Data migration scope** — no volumes or historical-data migration requirements were discussed for the CCF rebuild; needs scoping once the technical approach is set.
5. **Salesforce project budget** — the board has committed to doubling the Community Care Fund program itself; no Salesforce project budget figure, rate, or investment range has been discussed or validated. Pricing work is gated on the client supplying and validating a rate.
6. **"Special project" exception policy** — deliberately deferred as a client-owned policy decision (see Scope above); Rachel and Legal need to define criteria and ownership before Salesforce builds the enforcement/audit structure around it.
7. **PII/data residency requirements** — no specific requirements for handling applicant/recipient bank details or personal information were raised; should be confirmed given the sensitivity of payment data.
8. **Scope boundary with the Org Health Assessment** — confirmed with the client that this engagement is CCF-rebuild-only, with the Health Assessment's 45 findings treated as current-state context rather than in-scope remediation. Worth re-confirming as design decisions get made (e.g., if the CCF rebuild touches the same sharing-rule or secrets-handling issues the Health Assessment already flagged).

---

*Prepared using Scopezilla. T-shirt sizing, roadmap, and commercial figures are produced by later stages of this process and are not yet available in this brief.*
