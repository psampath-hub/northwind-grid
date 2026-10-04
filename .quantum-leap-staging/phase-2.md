## INTENT FOR
Finance/Accounts Payable processing grant payments; recipients awaiting disbursement; Rachel Doyle's stated #1 priority.

## INTENT OUTCOME
A validated Salesforce-to-SAP data exchange eliminates duplicate vendor setup and the entity-name mismatch that bounces roughly a third of approved applications, and the "Funded -- Payment Held" status reflects a real, confirmed payment-gateway outcome.

## INTENT MEASURED BY
Mismatch bounce-back rate reduced toward zero; disbursement cycle time shortened by removing the duplicate setup round-trip; no case silently shows "Funded" after an actual payment failure.

## INTENT MUST NOT
Must not build the settlement-outcome integration (INT-008) before the integration topology and gateway identity are confirmed in Phase 0 -- this is the single largest open risk in the engagement.

## PRE-DECIDED
- A validation callout against the official business registry runs before any payment request reaches SAP, not after.
- A record-triggered Flow generates the payment instruction and sends it to SAP; SAP's returned payment identifier writes back onto the case.
- Staged/milestone disbursements get their own tracking structure, migrating the ~20 existing spreadsheet-tracked grants before go-live.

## PLAN-MODE QUESTIONS
- [ ] What validation ruleset is intended for the entity-name check -- exact match, fuzzy tolerance, or an interactive confirmation with the applicant? (INT-006)
- [ ] Does Salesforce derive GL/cost-centre coding automatically, or does Finance/AP append it manually? (INT-007)
- [ ] Does Salesforce integrate directly with the EFT/payment gateway, or only with SAP? What is the gateway's identity? (INT-008 -- this should already be confirmed from Phase 0; re-verify before build)

## BUILD-MODE QUESTIONS
- Confirm the SAP integration's exact API surface (REST/SOAP, auth model) before building INT-007.
- Confirm the registry lookup service's API before building INT-006.

## DATA MODEL
Bank account, ABN, and tax reference fields carry Shield Platform Encryption. A staged-disbursement data model ties multiple payment stages to a single grant record. See INT-006 through INT-009 for load-bearing detail.

## AUTOMATION
Pre-submission registry validation (INT-006) gates the payment-request Flow (INT-007). Settlement-outcome tracking (INT-008) and milestone reminders for staged grants (INT-009) complete the disbursement lifecycle.

## UI
No new user-facing surface in this phase beyond status visibility on the existing case layout.

## SECURITY
Shield Platform Encryption applies to all bank/ABN/tax fields given their sensitivity and the absence of a stated data-residency policy.

## REPORTS
Not load-bearing for this phase.

## SAMPLE DATA
Recommend test cases covering: a clean entity-name match, a deliberate mismatch, a staged-grant schedule, and a simulated payment failure.

## DATA SOURCES
| Source | Feeds | Status |
|---|---|---|
| Official business registry (ABN/ABR) | INT-006 | Not yet confirmed as an integration target |
| SAP vendor master | INT-007, INT-008 | Existing system, integration surface unconfirmed |
| AP-owned EFT/payment gateway | INT-008 | Identity unconfirmed |
| Existing staged-payment spreadsheet | INT-009 | Live today, on one person's laptop |

## ACCEPTANCE USER
- [ ] An applicant with a mismatched entity name is prompted to correct it before their application proceeds to disbursement, not after SAP rejects it.
- [ ] A failed payment shows "Funded -- Payment Held" on the case within the same business day, with the assigned Community Marketing Lead notified.
- [ ] A migrated staged grant generates a milestone reminder ahead of its next due date.

## ACCEPTANCE METADATA
- [ ] SAP integration deployed and tested end-to-end against a sandbox vendor-setup flow.
- [ ] Registry validation callout deployed and tested against both a match and a deliberate mismatch.
- [ ] All ~20 existing staged grants migrated and verified against the source spreadsheet.
