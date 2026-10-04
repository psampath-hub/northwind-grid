# Scoping Context — Northwind Grid

> Reference role: **optional background** for the build agent. Surfaces the consulting-stage scoping work that produced this engagement. Read only when you need to understand *why* a particular phase boundary or trade-off was chosen.
>
> Not load-bearing. The phase briefs and `01-engagement-intent.md` are the operative documents.

## Current-state challenges
- **Eligibility checks against the 2km-of-asset rule are manual, run against a separate mapping tool with no boundary layer in the CRM; this accounts for an estimated 40% of eligibility disputes.**
- **No integration between Salesforce and SAP for vendor/payment setup; recipients are set up twice, and an estimated 1/3 of applications bounce on entity-name mismatches between the applicant-entered name and the ATO/ABN register.**
- **Panel scores are editable in Salesforce after the panel meeting with no change history — a field overwrite destroys the prior value, so a post-decision adjustment leaves no trace.**
- **The 'special project' exception to the 2-year re-application rule has no formal definition; it is tracked only in the Head of Communications' personal, unshared document and was applied inconsistently by an acting director during her leave.**
- **Payment-gateway outcomes (settled/failed) do not flow back to Salesforce; a case can show 'Funded' for weeks after a payment has actually bounced.**
- **Audit-report follow-up relies on a manual calendar reminder and a monthly report; an estimated 20% of audit reports are late or missing, and a recent compliance review could not fully account for 5 years of audit status.**
- **5 of 11 approval-panel members lack Salesforce logins and score via spreadsheet; scores are manually transcribed into the CRM before each panel meeting, costing an estimated 40 hours/year.**
- **Application volume has grown roughly 8x since 2019 (50/yr to ~400/yr) and the board has approved a further ~2.5x increase to ~1,000/yr by 2028, with no process maturity change to match.**

## Business impacts
- **No defensible record exists for special-project overrides or panel-scoring decisions, creating regulatory and reputational exposure (anticipated Senate estimates scrutiny; two missed ministerial SLAs, one escalated to the CEO).** — Regulatory / reputational
- **End-to-end disbursement for large grants takes 6-8 weeks against a recipient expectation of about a week, spending the goodwill built by approving the grant on payment-processing friction.** — Disbursement delay
- **Manual panel-score transcription costs an estimated 40 hrs/year; MP-escalation briefing notes are manually reassembled from case history across multiple Salesforce screens for every constituent complaint, with no reusable case-summary template.** — Operational cost
- **Community Marketing Leads are already straining at current volume (two on medical leave during last year's spike); the process has no slack to absorb the board-approved growth to 1,000 applications/year.** — Staff strain

## Confidence at a glance
- **Epics:** 6 total — Confirmed: 2, Assumed: 4, Unknown: 0

## Stakeholders mentioned in scope
Stakeholders are not enumerated in `strategy.json` schema; refer to `discovery-notes/` for named individuals.
