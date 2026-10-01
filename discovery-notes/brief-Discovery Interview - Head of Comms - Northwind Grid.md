# Scoping Brief: Community Care Fund — Discovery Interview with Head of Communications

**Source**: Discovery Interview - Head of Comms - Northwind Grid.md
**Date**: 12 June 2026
**Participants**: Rachel Doyle (Head of Communications, Northwind Grid — business owner of the Community Care Fund); Sanjay Rao (Discovery Architect, Salesforce Professional Services)
**Original length**: ~3,300 words (~30-min transcript)

---

## Business Context
- Community Care Fund is a board-driven community grant program, owned by Comms since its 2019 launch; rationale: Northwind Grid is a regulated utility running critical infrastructure through communities and wants to "be a good neighbour."
- Board has approved doubling the fund over three years, targeting ~1,000 applications/year by 2028 (up from ~50/year at 2019 launch, ~400/year today).
- Program complexity has grown sharply: from small local sponsorships (~$2K) to large capital works/climate-adaptation grants — largest cited example is a $2.3M application, personally approved by Rachel.
- Regulator scrutiny of community funding decisions is increasing; escalations can reach CEO level (a missed ministerial deadline was escalated to the CEO).
- Rachel frames this Salesforce engagement as the first serious effort to address long-standing process gaps she has raised repeatedly without budget/traction.

## Current State
- Intake: public EOI form on website (5 questions) captures an estimated ~60% of applications [Assumed — Rachel's estimate, not a measured figure]; remaining ~40% arrive via direct email to Rachel, a shared inbox, or people met at community forums, with inconsistent/manual case creation. Two lost-application incidents have reached journalists in the past 18 months.
- Volume is highly seasonal/spiky: bushfire season (Jan–Mar), winter storm damage, and a pre-June-30 wave tied to community groups' own funding acquittal deadlines. Example: 400 EOIs arrived in 6 weeks last February; 2 of 4 CMLs were on medical leave by April from the strain.
- Eligibility routing to 4 Community Marketing Leads (CMLs) is nominally geography-based but is really workload balancing (3 CMLs in Melbourne, 1 in Perth, covering an "enormous" territory).
- "Two-kilometre rule" (project must be within 2km of a Northwind Grid asset) is checked manually against a separate mapping tool — no boundary layer exists in the CRM. ~40% of eligibility disputes are on this boundary line.
- "Special project" exception (allows re-application within 2 years of prior funding) has no formal definition beyond "determined by the Head of Communications." Rachel tracks her own precedent in a personal, non-shared Word document. While Rachel was on leave (April), her acting director approved 3 exceptions Rachel would not have — surfaced only when a CML raised it, creating a fairness/consistency problem.
- Eligibility outcomes are logged in the CRM only as "eligible," with no reasoning captured — special-project overrides are indistinguishable from standard passes.
- No automated nudge exists for outstanding full applications; CMLs manually monitor a "pending full application" bucket. Turnaround ranges from 3 days to never.
- Case reassignment to Rachel for review fires a binary email alert on every case (~40/week), with no filtering by grant value; managed today via a biweekly pipeline review with her PA — described as workable now but not at 1,000 applications/year.
- Panel review (grants >$50K, ~60/year, ~15 per quarterly meeting): 11 panellists (6 internal — Rachel, General Counsel, Operations, ESG, Finance, MD's chief of staff; 5 external — community sector reps, an academic, an indigenous liaison), each scoring 0–10 across 5 criteria.
- 5 of 11 panellists (all external) lack Salesforce logins and score via spreadsheet; scores are manually transcribed into the CRM before each meeting — ~8–10 hours per panel, ~40 hours/year of manual data entry.
- Panel scores remain editable in the CRM until a case is accepted/rejected, with no audit trail (field overwrites destroy prior values). Rachel says scores are sometimes adjusted post-meeting to justify contentious outcomes, with no way to detect this.
- MP escalations: rejected applications generate ~1–2 constituent-driven MP calls/month, each requiring a 24-hour briefing note manually assembled from ~6 CRM screens. Senior Advisor spends ~15% of her time on this alone.
- Ministerial correspondence occurs ~quarterly (usually bushfire-recovery related) with a 10-business-day SLA; missed twice in the past year due to slow case-history retrieval — the second miss escalated to the CEO.
- Disbursement: CRM holds payment details (bank account, ABN, entity name) from the full application, but Finance's SAP system requires a separate vendor-master setup — every recipient is set up twice. SAP setup (owned by AP) involves a 5-page form, ABN certificate, signed bank-details form, and 3 approval levels; 5-business-day SLA routinely stretches to 3 weeks.
- Entity-name mismatches between applicant-entered name and the ATO/ABN register cause AP to bounce vendor creation back to Rachel's team on an estimated ~1/3 of applications — after the applicant has already been told they're approved, damaging goodwill.
- Legal signs funding agreements and, for grants >$100K, reviews ASIC/ABN check documentation; Legal's 5-day SLA is routinely missed. End-to-end time from panel approval to payment for large grants is typically 6–8 weeks, though recipients expect payment much sooner.
- Payment failures occur at an AP-owned EFT/payments gateway and are not reflected back into Salesforce — the CRM case still shows "Funded" even after a bounced payment. Failure notices go to an unmonitored shared inbox; the gap surfaces only when the recipient calls, ~3 weeks later.
- ~20 larger, newer grants currently use staged/milestone payments (50% signing / 25% milestone 1 / 25% completion), tracked entirely in a spreadsheet on Rachel's PA's laptop — no CRM scheduling or reminders exist; a clear single point of failure.
- Post-funding audit reports (receipts for simple grants, outcomes/impact reports for larger ones) are submitted by email/PDF to a shared inbox or directly to individual team members, with inconsistent storage — some attached to the CRM case, some in personal inboxes, some untraceable.
- The public two-year audit-follow-up policy commitment is not systematically enforced: no CRM alert exists; enforcement relies on a manual calendar reminder plus a monthly Salesforce report run by Rachel's PA. An estimated ~20% of audit reports are late or missing [Assumed — Rachel's estimate].
- In a recent compliance review, the regulator asked for "evidence of monitoring"; the Senior Advisor spent 3 days manually building a 5-year audit-status spreadsheet, still found unexplained gaps, and the organization sent the regulator a letter acknowledging the gaps.

## Desired State
Rachel's stated top three fixes, in her own priority order:
1. **Integrated vendor setup** — Salesforce and SAP exchanging validated entity-name and bank-detail data to eliminate duplicate vendor setup; estimated to remove ~3 weeks from disbursement cycle time and reduce goodwill damage from paperwork bounce-backs.
2. **Real audit tracking** — a system-driven mechanism that knows when a grant was paid, tracks when the audit report is due, chases the recipient automatically, and escalates to a CML and then to Rachel if overdue.
3. **Panel scoring integrity** — an immutable record of who scored what and when, with no undetected post-decision edits; framed as both a compliance and a fairness requirement.

Additional implied desired-state needs (not stated as explicitly as the above three) [Assumed]:
- Value-based alert filtering so only high-value cases require Rachel's direct review.
- A CRM-native boundary/geofencing layer for the two-kilometre eligibility rule.
- A documented, structured definition and logged rationale for "special project" exceptions.
- A genuine "Funded – Payment Held" status in Salesforce reflecting real payment-gateway outcomes (this one explicitly requested by Rachel, not inferred).

## Integrations
- **Salesforce ↔ SAP (vendor master)** — no integration today; causes duplicate manual setup and entity-name mismatch rejections. Rachel's #1 priority fix.
- **Salesforce ↔ EFT/payments gateway** (platform name not given, owned by AP) — payment success/failure status does not flow back to Salesforce. [Unknown: platform identity]
- **Public website EOI form → Salesforce case creation** — partially automated for the ~60% of applications submitted via the web form; mechanics not detailed. [Unknown]
- **External mapping/GIS tool** used by CMLs for the two-kilometre boundary check — separate from the CRM, no integration. [Unknown: tool identity]

## Users & Roles
- **Rachel Doyle** — Head of Communications; business owner; sole decision-maker on "special project" exceptions; reviews reassigned cases.
- **4 Community Marketing Leads (CMLs)** — 3 in Melbourne, 1 in Perth; handle eligibility checks and full-application chasing. Two were on medical leave by April during last year's volume spike.
- **Rachel's PA** — runs biweekly pipeline review, maintains the staged-payment spreadsheet, runs the monthly audit-overdue report.
- **Senior Advisor** — spends ~15% of time on MP briefing notes; led the 3-day manual compliance data pull.
- **11-person approval panel** — 6 internal (Rachel, General Counsel, Operations, ESG, Finance, MD's Chief of Staff) + 5 external (community sector reps, an academic, an indigenous liaison). The 5 external panellists lack Salesforce licenses/logins.
- **Acting Director** — covered for Rachel during April leave; approved 3 disputed special-project exceptions.
- **Legal / General Counsel** — signs funding agreements; reviews ASIC/ABN documentation for grants >$100K.
- **Finance / AP** — owns SAP vendor-master setup and the EFT/payments platform.

## Timeline & Constraints
- Board-approved plan to double the fund over three years, targeting ~1,000 applications/year by 2028.
- Seasonal intake spikes: Jan–Mar (bushfire-related), winter (storm damage), pre-June-30 (EOFY acquittal deadline for community groups).
- Existing SLAs routinely missed: SAP vendor setup (5 business days → up to 3 weeks); Legal review (5 days); ministerial correspondence (10 business days, missed twice in the past year); MP briefing notes (24-hour turnaround).
- Full disbursement cycle for large grants: 6–8 weeks from panel approval to payment vs. recipient expectation of ~1 week.
- Sanjay to return a synthesized draft of priority scope to Rachel in about a week (from 12 June 2026).
- Sanjay to send 3–4 follow-up questions on disbursement via email, spread across multiple emails per Rachel's explicit request.

## Budget Signals
- Board has approved funding to double the Community Care Fund itself by 2028 — a program-level investment commitment, not a Salesforce project budget figure. [Assumed: no investment range stated for the Salesforce work]
- Rachel notes she has raised the staged-payment tracking gap "every quarter" but there "hasn't been budget to fix it" until this assessment — signals budget discussion occurred at some level, but no figures were shared.
- No hourly rates, project costs, or pricing were discussed in this interview.

## Compliance & Security
- Regulator is increasingly scrutinizing community funding decisions, including explicit questions about the two-year audit-report follow-up policy (a public commitment).
- A recent compliance review requested "evidence of monitoring"; the organization could not fully account for 5 years of audit-report status and sent a letter acknowledging gaps.
- Rachel anticipates a possible Senate estimates question about the basis for funding decisions and currently cannot produce a defensible record for special-project eligibility overrides.
- Panel scoring has no audit trail — scores are editable post-decision with no change history — a flagged compliance exposure given regulator interest.
- Legal requires ASIC/ABN check documentation for grants over $100,000 as a governance control before disbursement.
- No data residency or applicant/recipient PII-handling requirements (e.g., for bank details) were discussed. [Unknown]

## Decisions Made
- None — this was an exploratory discovery interview; Sanjay explicitly framed it as "no slides, nothing prepared." Rachel's "three things" are stated priorities/preferences, not committed scope decisions.

## Action Items
- Sanjay Rao to synthesize this interview with other discovery inputs and return a draft of priority scope to Rachel Doyle within ~1 week of 12 June 2026.
- Sanjay Rao to send Rachel 3–4 follow-up questions specifically on the disbursement/payments process, via email, spread across multiple separate emails (per Rachel's request, not all at once).

## Open Questions & Ambiguity
- Exact system mechanics behind the ~60%/40% website-vs-email intake split are unclear — no confirmed automated workflow for case creation from the web form. [Unknown]
- Identity of the AP-owned EFT/payments gateway platform is unspecified. [Unknown]
- Identity of the external mapping/GIS tool used for the two-kilometre boundary check is unspecified. [Unknown]
- No formal, documented definition exists for a "special project" exception — currently sole informal discretion of the Head of Communications, tracked in a personal, non-shared document. [Unknown — governance gap]
- True scope of panel score "gaming" is unquantified; Rachel deliberately avoided confirming intent ("I don't want to say people are gaming it. But it happens"). [Unknown]
- The ~20% late/missing audit-report figure is Rachel's estimate, not a system-verified metric. [Assumed]
- Whether the 5-question EOI form needs to change was not discussed. [Unknown]
- No discussion of current Salesforce edition, licensing, data volumes, or technical architecture — org details are inferred only from process narrative. [Unknown]

## Key Quotes
> "The gap between the two is, uh — significant." — Rachel Doyle (process on paper vs. reality)

> "Last year we had one application for two point three million dollars. And I approved that one. So the range of what we're doing has just exploded, and the process hasn't caught up." — Rachel Doyle

> "We're planning for a thousand applications a year by 2028. Which — at the current process maturity, is not going to happen without something breaking." — Rachel Doyle

> "If someone applies for a grant and we lose their application, that's a story... I've had two of those go to journalists in the last eighteen months." — Rachel Doyle

> "Honestly? I make it up. I try to be consistent... Nobody else sees that doc." — Rachel Doyle, on deciding "special project" exceptions

> "We're going to have a Senate estimates question about this at some point. And they will ask, on what basis did you fund X, and I need to be able to answer that. Right now I can't." — Rachel Doyle

> "The score field just — updates. And the old value's gone." — Rachel Doyle, on the lack of a panel-scoring audit trail

> "If we had a 'Funded – Payment Held' state — like, a real one, that Salesforce knew about — that would fix so much." — Rachel Doyle

> "There hasn't been budget to fix it, and honestly this Salesforce assessment is the first serious effort I've seen to actually get after it." — Rachel Doyle

> "One. Fix the double vendor setup with SAP... Two. Real audit tracking... Three. Panel scoring integrity." — Rachel Doyle, closing priorities
