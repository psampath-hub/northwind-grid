## INTENT FOR
The 11-person review panel (6 internal, 5 external); Rachel Doyle; recipients awaiting their 2-year audit-report obligation.

## INTENT OUTCOME
Panel scoring is tamper-evident and defensible, all 11 panellists score through one system instead of a spreadsheet workaround, and the 2-year audit-report commitment is tracked and escalated automatically.

## INTENT MEASURED BY
No undetected post-decision score changes; the ~40 hours/year of manual score transcription eliminated; the audit-report compliance rate becomes system-verified rather than estimated.

## INTENT MUST NOT
Must not finalize the external-reviewer licensing build (INT-010) until Northwind Grid confirms panel cadence and budget; must not build only a single-tier audit escalation when the actual intent is three tiers (INT-013).

## PRE-DECIDED
- Field change history logs every panel-score edit before the lock point; a validation rule blocks any edit after the case reaches its final decision stage.
- Applications over $50,000 go to panel review; applications under that threshold go to the Community Marketing Lead or Head of Communications.
- The Head of Communications is alerted only above a confirmed value threshold, not on every case reassignment.

## PLAN-MODE QUESTIONS
- [ ] Does the panel meet quarterly or twice a year? (INT-010 -- should be confirmed from Phase 0; re-verify before build)
- [ ] What are the program's five scoring criteria? (INT-010)
- [ ] Has Northwind Grid budgeted for the external panellist Experience Cloud licenses? (INT-010)
- [ ] What case-status transition should trigger the panel-score lock? (INT-011)
- [ ] What dollar figure determines the $50,000 panel-routing threshold? (INT-012)
- [ ] Who formally decides an audit report is "complete," and against what standard? (INT-013)
- [ ] For staged grants, does the 2-year audit clock start at first disbursement or final disbursement? (INT-013)

## BUILD-MODE QUESTIONS
- Confirm the Experience Cloud license type and provisioning before building INT-010.

## DATA MODEL
A new Panel Score child object (one record per panellist per application) carries Field History Tracking. A due-date field on Case drives the audit-tracking sequence. See INT-010, INT-011, INT-013 for load-bearing detail.

## AUTOMATION
Score-lock validation (INT-011) and value-based routing/notification (INT-012) govern the review stage. A scheduled Flow drives the three-tier audit chase/escalate sequence (INT-013).

## UI
An Experience Cloud "Grantmaking" reviewer workspace gives the 5 external panellists a scoring interface, replacing the spreadsheet (INT-010).

## SECURITY
Login-based Experience Cloud licensing for the 5 external panellists, scoped to the review workspace only.

## REPORTS
The review pack (application summaries for an upcoming panel cycle) is generated and distributed to all 11 panellists (INT-010).

## SAMPLE DATA
Recommend a test panel cycle with a mix of under- and over-threshold applications, and a deliberately contentious score correction before and after the lock point.

## DATA SOURCES
Not applicable -- no external data sources beyond the Case and Panel Score objects already in the org.

## ACCEPTANCE USER
- [ ] An external panellist logs into the review site and scores an application without anyone transcribing it afterward.
- [ ] A score corrected before the final decision is logged with who and when; the same score cannot be edited a week later.
- [ ] The Head of Communications receives an alert on a $200,000 application but not on a routine $35,000 one.
- [ ] A grant two years overdue on its audit report has already been chased, escalated to a Community Marketing Lead, and escalated again to the Head of Communications -- without a manual calendar reminder.

## ACCEPTANCE METADATA
- [ ] Experience Cloud reviewer site deployed and tested with at least one external test user.
- [ ] Score-lock validation rule deployed and verified against a pre- and post-decision edit attempt.
- [ ] Three-tier escalation Flow deployed and tested against a simulated overdue audit report.
