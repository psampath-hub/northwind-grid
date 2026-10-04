## INTENT FOR
Applicants submitting Expressions of Interest and Full Applications; Community Marketing Leads triaging and screening them.

## INTENT OUTCOME
Every inbound application attempt reliably produces a trackable case, the 2km eligibility check runs without a manual external-tool lookup, and the Full Application captures its real conditional-field logic correctly.

## INTENT MEASURED BY
Zero lost-application incidents from the email intake path; a materially reduced eligibility-dispute rate on the 2km check; the Full Application's GST/amount/co-applicant logic captured without guesswork.

## INTENT MUST NOT
Must not ship intake or eligibility automation that silently reproduces the current manual process's failure modes (lost emails, undocumented eligibility reasoning).

## PRE-DECIDED
- Email-to-Case creates a system-of-record case from every inbound email immediately; a courtesy redirect to the web form never substitutes for case creation.
- The eligibility outcome stores the specific criteria evaluated (or override basis) alongside the pass/fail status, not just the status itself.
- Full Application's GST-conditional funding fields and amount-changed-since-EOI reconciliation are built as conditional logic, not a flat form.
- The co-applicant attestation is a typed-name-and-checkbox pattern, not a dedicated e-signature product.

## PLAN-MODE QUESTIONS
- [ ] What are all the email addresses applicants currently use to reach the fund? (INT-001)
- [ ] What should count as a "likely duplicate" across intake channels? (INT-002)
- [ ] Is Geoscape confirmed as the authoritative asset-location data source for the 2km check? (INT-003)
- [ ] What margin near the 2km boundary should trigger manual review instead of an automatic pass/fail? (INT-003)
- [ ] Does the 2-year re-application rule block on "funded" or "submitted"? (INT-004 -- carried from Phase 0 if still unresolved)
- [ ] Should the Full Application support save/resume? (INT-005 -- carried from Phase 0 if still unresolved)

## BUILD-MODE QUESTIONS
- Confirm the exact field-to-case mapping for the public web form before building INT-001.
- Confirm the asset-location data format and location before building INT-003.

## DATA MODEL
Case remains the backbone object for applications. New fields capture intake channel, eligibility criteria evaluated with reasoning, and measured proximity distance. See INT-001, INT-003, INT-004 for the load-bearing detail.

## AUTOMATION
Email-to-Case and web-form intake both funnel into Case creation (INT-001). Capacity-based routing replaces static assignment rules (INT-002). The 2km proximity check and 2-year re-application check run automatically with recorded reasoning (INT-003, INT-004).

## UI
The public Full Application form is built with conditional branching for GST status, amount-changed-since-EOI, and the document checklist (INT-005). No custom Lightning Web Components required for this phase.

## SECURITY
The Experience Cloud guest-user profile for the public intake forms is locked to form submission only, with no broader object access.

## REPORTS
Not load-bearing for this phase.

## SAMPLE DATA
Not yet defined -- recommend a small set of test applications covering each intake channel, a borderline-eligible location, and a GST-registered vs. non-registered applicant.

## DATA SOURCES
| Source | Feeds | Status |
|---|---|---|
| Public EOI/Full Application web forms | INT-001, INT-005 | Live today |
| Fund email inboxes | INT-001 | Live today, mechanics undocumented |
| Asset location data (likely Geoscape) | INT-003 | Unconfirmed as source |

## ACCEPTANCE USER
- [ ] An applicant emailing the fund directly sees a case created without acting on any redirect.
- [ ] A Community Marketing Lead sees the measured proximity distance on a borderline application, not just pass/fail.
- [ ] An applicant whose requested amount changed since EOI is prompted to explain the change and the quotes are checked against it.

## ACCEPTANCE METADATA
- [ ] Email-to-Case routing addresses deployed and tested against at least one real inbox.
- [ ] Capacity-based assignment rule deployed and verified against a multi-case test batch.
- [ ] Proximity-check automation deployed against a test asset-location dataset.
