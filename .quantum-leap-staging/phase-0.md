## INTENT FOR
Rachel Doyle (Head of Communications) and the delivery team jointly.

## INTENT OUTCOME
Every one of the 8 source conflicts in the current requirements has a client-confirmed answer (or a conscious deferral), and the Grant Disbursement integration topology and payment gateway identity are confirmed, before any build phase starts.

## INTENT MEASURED BY
Zero unresolved source conflicts carried into Phase 1; the integration topology and gateway identity for Phase 2 are named, not assumed.

## INTENT MUST NOT
Must not start Phase 1 build work on an epic whose source-document conflict is still unresolved.

## PRE-DECIDED
- This engagement is scoped to the Community Care Fund rebuild only; the organization's separate Org Health Assessment findings are current-state context, not in-scope remediation.
- Defining what qualifies as a "special project" exception is a client-owned policy decision (Rachel + Legal), not a build task -- the build only provides enforcement/audit structure once that policy exists.

## PLAN-MODE QUESTIONS
- [ ] Confirm the 2-year re-application rule wording: does it block on a prior application being *funded*, or merely *submitted*? (blocks INT-004)
- [ ] Confirm whether every inbound email to the fund should create a case immediately, or whether the documented auto-reply-redirect behavior is intentional. (blocks INT-001)
- [ ] Confirm whether the Full Application should support partial save/resume, given its length, despite the current form's "no partial save" wording. (blocks INT-005)
- [ ] Confirm the panel's actual meeting cadence -- quarterly, or twice a year as the application form states. (blocks INT-010)
- [ ] Confirm the panel score editability/lock trigger point. (blocks INT-011)
- [ ] Confirm the Grant Disbursement integration topology: does Salesforce integrate directly with the EFT/payment gateway, or only with SAP? Confirm the gateway's identity. (blocks INT-008, the single largest open item in this engagement)
- [ ] Confirm whether Northwind Grid's Public Sector Grantmaking Salesforce license is available -- the architecture for Phases 1-3 assumes it, and the org does not have it provisioned today.
- [ ] Confirm the audit-report escalation model: a single tier (per the requirements document) or the three-tier chase/escalate/escalate sequence (per discovery). (blocks INT-013)

## BUILD-MODE QUESTIONS
- None -- this phase is discovery only, no build work.

## DATA MODEL
No build work in this phase.

## AUTOMATION
No build work in this phase.

## UI
No build work in this phase.

## SECURITY
No build work in this phase.

## REPORTS
No build work in this phase.

## SAMPLE DATA
Not applicable -- no build work in this phase.

## DATA SOURCES
Not applicable.

## ACCEPTANCE USER
- [ ] Rachel Doyle has confirmed an answer (or a conscious deferral, logged as a gap) for each of the 8 source conflicts named above.
- [ ] The Grant Disbursement integration topology and payment gateway identity are confirmed in writing.
- [ ] The Public Sector Grantmaking license question is resolved.

## ACCEPTANCE METADATA
- [ ] No metadata is deployed in this phase.
