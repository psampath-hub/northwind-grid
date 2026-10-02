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

*Confidence note: live web search was unavailable in this session; the points below combine general industry knowledge (unverified against a primary source — flagged accordingly) with grounded Salesforce platform guidance.*

- Australian network utilities commonly run community-benefit-sharing or grant programs tied to infrastructure proximity, often shaped by regulatory and social-licence expectations. Typical governance patterns include two-stage intake (EOI → full application), mixed internal/external assessment panels, tiered approval thresholds, documented conflict-of-interest declarations, and audit trails for funding decisions given the use of public money. **[Unverified — general pattern, confirm against Northwind Grid's actual regulatory framework.]**
- Salesforce's standard pattern for this type of program is **Nonprofit Cloud for Grantmaking / Public Sector Grantmaking** — covering the full lifecycle from application intake through award, disbursement, and progress reporting, with a form framework supporting multi-stage applications and a consolidated reviewer workspace.
- For the 5 external panellists without Salesforce licenses: the standard pattern is an **Experience Cloud "Grantmaking" site**, which explicitly supports external reviewers scoring applications through a portal rather than a spreadsheet workaround, using login-based external licensing (metered by daily unique logins — well suited to panellists who review only twice a year) rather than full internal seats.
- No standard pattern found so far directly addresses SAP-to-Salesforce disbursement integration specifically — recommend a targeted architecture-decision review on this before design.
- General expectation for Australian government/utility grant disbursements is an ABN validity check before payment and an auditable per-decision trail — consistent with what was described as a current gap. **[Unverified against a primary regulatory source — confirm specific obligations with the client's compliance team.]**

## Open Questions

Requirements analysis across the six delivery areas (intake, eligibility, full application, panel review, disbursement, and audit tracking) surfaced a substantial list of open items, including several direct conflicts between source documents. These are grouped below by type, with the conflicts resolved first — those are the ones most likely to change how the system is built if answered differently.

### Conflicts between source documents (resolve first)

1. **Re-application rule wording.** The requirements workbook contains two different versions of the 2-year re-application rule: one says an applicant can't reapply if a *prior application was funded* in the last 2 years; an earlier version of the same document says *submitted*, not funded. These produce different outcomes — under one reading, an applicant rejected 18 months ago could reapply today; under the other, they couldn't. Which is correct?
2. **Email-intake behavior.** The requirements describe an email-based application as receiving an automatic reply that redirects the sender to the website form, with no case created from the email itself. Discovery interviews describe lost applications (including two that reached journalists) happening by exactly this path — the sender doesn't follow up on the redirect, and nothing is ever recorded. Should every inbound email create a trackable record immediately, with the redirect as a secondary nudge rather than the only response?
3. **Attachments at first-stage submission.** The requirements document says attachments may be needed at the Expression of Interest stage; the actual Expression of Interest form states no attachments are required at that stage. Is the requirement outdated, or is a form change intended?
4. **Partial-save on the Full Application.** The Full Application form explicitly tells applicants it cannot be saved partway through — yet it is a long, document-heavy form (budget breakdown, two quotes, co-applicant details, supporting documents). Is "no partial save" an intentional policy, or should the rebuild support saving progress and resuming later?
5. **Panel meeting cadence.** One source describes the review panel meeting quarterly; the published Expression of Interest form states the panel meets twice a year (March and September). This affects how review-cycle reminders, batch sizes, and external reviewer access are designed. Which is correct?
6. **Panel score editability.** The requirements state that panel scores stay editable right up until a final accept/reject decision is made. Separately, an immutable, tamper-evident scoring record — no undetected changes after the fact — has been named as a top priority. These two statements are in direct tension. Should the window for editing a score be shortened (e.g., locked once the panel discussion concludes), or is a later correction (e.g., fixing a typo) a genuine business need that should simply be logged rather than prevented?
7. **Where disbursement failures surface.** The requirements imply Salesforce integrates directly with the bank/payment gateway to receive settlement outcomes. Discovery interviews describe a different picture — payment failures happen entirely inside Finance's systems and never reach Salesforce, which is why a case can show "Funded" for weeks after a payment has actually failed. Which integration path is accurate: does Salesforce talk to the payment gateway directly, or only to the ERP (which would then need to relay the outcome back)?
8. **Audit follow-up escalation.** The requirements document describes a single escalation step (an alert to the regional lead) once an audit report becomes overdue. Discovery interviews and the stated goal both describe three steps: chase the recipient, escalate to the regional lead, then escalate to the Head of Communications if it's still outstanding. Building to the documented single-step version would not deliver what's actually wanted — can the three-step version be confirmed as the intended design?

### Missing requirements

9. Is there a monitoring mechanism needed to catch the specific failure mode behind the two journalist-escalation incidents — an application arrives but no case is ever created, so it simply disappears?
10. Should the system record *why* an eligibility decision was reached — which criterion passed or failed, or whether it was a standard pass versus a special-project exception — rather than just the pass/fail outcome itself? This is the exact mechanism behind the inability to defend past decisions to a regulator.
11. Once the "special project" exception policy itself is defined (a decision for Northwind Grid and Legal, not something this engagement will decide), what specific criteria fields and approval information will the enforcement and audit structure need to capture against each application?
12. Who should own keeping the underlying reference data current if the 2km boundary check and the special-project exception move into system-enforced rules — the geographic boundary data, and separately any criteria behind the special-project policy once it exists?
13. Should applicants get clearer, self-service information about the 2km proximity rule — for example, an estimated distance at the point of submission — rather than finding out only after manual review? Roughly 40% of eligibility disputes trace to this exact line.
14. No requirement addresses matching or merging duplicate applications submitted through different channels (web form vs. email) for the same project. Is this needed?
15. Is dedicated usability and accessibility work in scope for the public application form, given it serves a wide range of community groups, schools, and charities under real public scrutiny — or is that owned by Northwind Grid's own team?
16. Who owns ongoing configuration of intake automation (routing rules, duplicate matching) after go-live, given there's currently no established governance body for the Salesforce platform?
17. The Expression of Interest form publishes fixed round deadlines (31 January, 31 July) for submitting the Full Application, but no automated reminder exists against those deadlines today. Should the rebuild add one, and what should happen to an application that misses the deadline?
18. Should the system block submission if the two required quotes don't add up to the requested funding amount, or simply flag the mismatch for the reviewer?
19. If a requested amount changes between the first-stage and full application — crossing the $50,000 review threshold in either direction — should that change which review track the application follows?
20. The Full Application requires a co-applicant's personal details, with a declaration that the primary applicant has confirmed the co-applicant is aware they're included — but that's attested only by the primary applicant, with no independent confirmation step. Is a direct notification or consent step to the co-applicant wanted?
21. Should a returning co-applicant be matched to past records (the way the primary applicant already is), or treated as a fresh record every time?
22. The required reference number and email on the Full Application link it back to the original submission — what should happen if they don't match? Blocked submission, a failed link, or manual review?
23. A volunteer-support offer and a promotional-activity question both appear on the Full Application with no defined destination for the answers — are these informational only, or do they need to feed into another process (e.g., coordinating a volunteering day, or informing panel scoring)?
24. The application is evaluated against several named strategic criteria (community diversity, lower socio-economic areas, environmental benefit, low-carbon support) — should these become explicit, scorable fields on the application, or stay inferred from free-text narrative by the panel?
25. The required document checklist mixes required items (two quotes, or a justification for one) with optional ones — should the system actively enforce the required ones before allowing submission?
26. Is a dedicated design pass warranted for the Full Application form, given its real complexity (conditional fields, a mandatory co-applicant section, a no-partial-save constraint) — or is standard form configuration assumed sufficient?
27. Capturing a non-submitting co-applicant's personal details, and any change to the no-partial-save policy, are data-handling and policy decisions as much as build decisions — does Northwind Grid have (or need) a named owner who signs off on this kind of change before it ships?
28. What dollar figure determines the $50,000 panel-review threshold — the amount requested at full application, an adjusted recommended amount, or (for staged grants) the full agreement value versus a single stage?
29. The current review-alert process is already too noisy by Rachel's own account (roughly 40 emails a week, all-or-nothing). Within the under-$50,000 track specifically, which applications genuinely need her direct review versus the regional lead's alone, and on what basis?
30. What are the five scoring criteria panel members actually score against? Neither source document names them.
31. Should the system generate and distribute the panel's review pack (currently a manual, ~40-hours-a-year task) to all panel members including the external reviewers?
32. No service-level target is defined for how quickly the regional lead or Head of Communications should assess an under-$50,000 application — should one be set, with an escalation if missed?
33. Does Northwind Grid have an existing conflict-of-interest policy for the 11-person panel (6 internal, 5 external) that should be enforced in the system, or does one need to be defined?
34. The external reviewer workspace is a brand-new experience for 5 people who have never used Salesforce and will use it only a few times a year — has any onboarding or usability work been scoped for them?
35. The payment gateway Finance's team uses for disbursement was never named in any source material — this blocks scoping the integration that would let Salesforce see settlement outcomes. Can this be identified?
36. Once a payment is held due to a failure, who should be alerted and how, and should there be an escalation if the regional lead doesn't act — mirroring the audit-tracking escalation design?
37. The approach for staged/milestone grant payments doesn't address migrating today's ~20 live grants (currently tracked in a spreadsheet) into the new system, or reminders ahead of upcoming milestones. Should both be in scope?
38. Once a payment is marked "held" due to a failure, what happens next — does correcting the details automatically retry the payment, is a fresh request required, and does a large grant's retry need the same Legal/panel re-approval it originally required?
39. The requirement that Finance's cost-centre coding travel with every payment request doesn't say whether Salesforce derives that coding automatically or whether Finance adds it manually after receiving the request. Which is intended?
40. Payment details are captured once and reused for every stage of a multi-year grant — should they be re-confirmed if a later-stage payment happens long after the last one, in case bank details have changed?
41. Legal's review of large-grant documentation (required over $100,000, with a 5-day target that is routinely missed) isn't reflected in any requirement. Should this step be visible and trackable in Salesforce, or does it stay an entirely manual, off-system step?
42. The disbursement process spans three separate teams (Communications, Finance, Legal) with no named end-to-end owner. Who should be accountable for the overall disbursement timeline and for fixing it when something breaks?
43. There's no defined mechanism today for an audit report to reliably reach Salesforce as a trackable record — it currently arrives by email to a shared inbox or directly to a staff member, with inconsistent storage. Is building a structured intake path in scope, or is the rebuild only the tracking and escalation layer on top of however a report happens to arrive?
44. Should the roughly 5 years of existing audit-report history (the subject of a 3-day manual reconstruction during a recent compliance review) be migrated into the new tracking mechanism, so the system isn't starting blind on grants already in flight?
45. What evidentiary type of audit report is expected for a given grant (a simple receipt versus a full outcomes report) and where should that expectation be recorded so an automated reminder can tell the recipient what's actually required of them?
46. If the regional lead responsible for an audit follow-up changes roles or leaves before the 2-year mark, what happens to that tracking responsibility? Should it be tied to a role or shared queue rather than a named individual?
47. Who formally decides that an audit report is "complete," and what specific threshold should trigger escalation from the regional lead to the Head of Communications? Both are currently informal.

### Ambiguities to clarify

48. Which of the five eligibility criteria should the system determine automatically, and which stay a manual judgment call that the system simply records?
49. If a Full Application's stated distance is "verified during shortlisting" after already passing the initial 2km check, can an application be rejected at that later stage for failing the same rule — and if so, how should that reversal be recorded?
50. Should the under-$50,000 review track use the same shortlist step as the panel track before a final decision, or go straight to an accept/reject call?
51. Is each panel member's score visible to other panellists during or before the discussion, or only revealed when the panel meets? Visibility either way has implications for scoring integrity.
52. Rejected applicants are notified only after the full batch finishes processing, while approved applicants are notified individually as decisions are made. Is the delay for rejections intentional?
53. How settlement outcomes (success, failure, pending) should reach Salesforce — a live push from the payment gateway, scheduled checking, or manual entry — depends on which gateway is used (see question 35) and can't be resolved until that's known.
54. Should Finance's cost-centre coding be mapped automatically inside Salesforce, or applied manually by Finance after the fact?
55. Legal's documentation review for grants over $100,000 isn't reflected anywhere in the requirements — should it become a tracked step, or remain off-system?
56. Who decides an audit report is "complete," and against what standard, given that what's expected differs by grant size?

### Risks worth flagging

57. The current manual 2km proximity check already drives roughly 40% of eligibility disputes at today's volume. Without a system-native fix, that dispute volume — and the staff time to resolve it — should be expected to grow at least in line with the board-approved growth to 1,000 applications a year.
58. If the web-form intake path gets most of the automation attention, the email intake path — the actual source of the two lost-application incidents that reached journalists — may remain manual and under-addressed. Worth confirming both paths get equal attention.
59. Whatever routing mechanism is chosen for incoming applications needs to hold up under seasonal spikes (400 applications in six weeks last year), not just average volume — this is what strained staff capacity last time.
60. Capturing a co-applicant's personal details based only on the primary applicant's unverified word that the co-applicant is aware of it is a privacy exposure worth Northwind Grid's attention, particularly for a regulator-scrutinized utility.
61. At the board-approved target of 1,000 applications a year, the volume of applications reaching panel review scales proportionally — each panel meeting could face 2–3 times today's caseload. The review workspace should be designed and tested with that higher volume in mind, not just today's.
62. Neither source document defines how a split or contentious panel vote should be resolved, or whether there's an appeal path separate from the existing escalation to local representatives. Worth defining given the fairness concerns already raised about scoring.
63. Payment details are reused across every stage of a multi-year grant with no re-confirmation step — a bank-detail change partway through a grant could go undetected until a payment fails.
64. No requirement addresses data residency or handling standards for the bank account numbers, ABNs, and tax details this rebuild will capture and transmit — the most sensitive data in the entire program, and worth confirming with Northwind Grid's security and compliance function before detailed design.
65. If a regional lead responsible for a specific grant's audit follow-up leaves or changes roles before the 2-year mark, the tracking responsibility could be silently lost unless it's tied to a role rather than a named person — the exact failure mode the current manual process already has.

### Capability questions

66. Should the 2km proximity check become a true geographic boundary check reflecting the actual shape of a substation or transmission corridor, a simpler point-to-point distance check, or stay an external manual lookup with only the outcome recorded in Salesforce? This decision affects both accuracy and build complexity.
67. The exact mechanics behind the public web-form intake path aren't documented today. Two different standard approaches would fit what's described, with different consequences for how attachments, multi-stage forms, and the review workspace work later in the process — this needs a deliberate choice, not a default.
68. With four regional leads and no formal routing logic today (it's informally workload-balanced), should assignment of new applications be a defined rule, or a more dynamic workload-based distribution — especially given it's already strained staff capacity during spikes?
69. The requirement that an applicant's full application sign off with both their own and a co-applicant's signature doesn't match any standard pattern already confirmed for this type of program — what level of formality is expected here (a true e-signature product, or a simpler attestation)?
70. Should Northwind Grid adopt a dedicated reviewer workspace for the 5 external panel members who currently lack system access and score via spreadsheet — which would need new licenses to be budgeted — or is there a reason to keep them off the platform?
71. Confirming that a validated check (not just a data pass-through) will catch entity-name mismatches before they reach Finance is central to the #1 stated priority for this engagement — what level of validation is intended (an exact match, a tolerant/fuzzy match, or a live lookup against an official registry)?
72. There's no existing time-based tracking mechanism in Salesforce today for a multi-year due date with staged escalation. A couple of standard approaches would fit — this is a design decision to make deliberately rather than defaulting to a one-off custom build.

### Assumptions carried forward (confirm, or we proceed on this basis)

73. We're assuming Geoscape — already connected to Northwind Grid's platform for other purposes — is the same tool regional leads currently use for the 2km proximity check, based on your confirmation during our strategy discussion. This hasn't been independently verified against what's actually used day-to-day; worth a quick check before this becomes an integration decision.
74. We're assuming both the web form and email remain permanent intake channels going forward. If the real intent is to phase out email as a direct application channel over time, that changes where effort should go.
75. The research into appropriate licensing for external reviewers assumed the twice-yearly panel cadence from the application form, not the quarterly cadence described in discovery (see conflict #5). If the cadence is confirmed as quarterly, the licensing recommendation should be revisited.
76. We're assuming the vendor-data validation Northwind Grid wants will have a concrete, agreed rule set once defined (see question 71) — this is currently assumed rather than confirmed, and is easy to leave unexamined given how confidently it's described as a priority.
77. We're assuming that reports attaching reliably to the right case/grant record will improve incidentally as other parts of the system are rebuilt. This hasn't been separately confirmed, and if intake stays out of scope (question 43), this assumption may not hold at go-live.

### Other open items (not drawn from requirements analysis)

78. **Data migration scope.** No volumes or historical-data migration requirements beyond the audit-history question above (44) have been discussed for the broader CCF rebuild; needs scoping once the technical approach is set.
79. **Salesforce project budget.** The board has committed to doubling the Community Care Fund program itself; no Salesforce project budget figure, rate, or investment range has been discussed or validated. Pricing work is gated on Northwind Grid supplying and validating a rate.
80. **Scope boundary with the Org Health Assessment.** Confirmed that this engagement is Community Care Fund rebuild only, with the separate platform-wide Health Assessment findings treated as current-state context rather than in-scope remediation. Worth re-confirming as design decisions get made — for example, if this rebuild touches the same data-sharing or credential-handling issues the Health Assessment already flagged.

---

*Prepared using Scopezilla. T-shirt sizing, roadmap, and commercial figures are produced by later stages of this process and are not yet available in this brief.*
