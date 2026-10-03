# Northwind Grid — Community Care Fund: Solution Design

## Architecture Foundations

**Org strategy.** Single org, extending the production org Northwind Grid already runs (Platform, Marketing Cloud, Data 360, migrated from Microsoft Dynamics). Nothing in discovery points to a regulatory or scale driver for a second org `[assumption: no stated business-separation requirement]`.

**Base pattern.** Salesforce's Public Sector Grantmaking module — built on Platform plus Experience Cloud — is the standard fit for this exact lifecycle: application intake, review, award, disbursement, and progress reporting `[KA-9728]`. The org's current footprint doesn't show this module provisioned today, so adopting it is a licensing decision for Northwind Grid to confirm, not something this design assumes unilaterally `[assumption: Public Sector Grantmaking module availability and licensing]`.

**Data model.** Case stays the backbone object for applications, continuing the pattern already in place rather than introducing a new custom object `[extends: current Case-creation pattern]`. New additions: a Panel Score child object (one record per panellist per application, with Field Audit Trail enabled) and an Exception Reason / Approval Rationale field set on Case, giving the special-project enforcement structure somewhere to write to once Northwind Grid and Legal define the policy itself.

**Sharing model.** Org-wide defaults start at Private for Case and the new Panel Score object, opened only where a defined role needs visibility — eligibility and panel decisions carry real fairness and regulatory weight, so the default should be the most restrictive setting that still works.

**Security.** Bank account, ABN, and tax reference fields get Shield Platform Encryption given their sensitivity and the absence of any stated data-residency policy today `[extends: standard Shield pattern for sensitive financial PII]`. The Experience Cloud guest-user profile for the public EOI and Full Application forms is locked to form submission only, with no broader object access `[assumption: standard Experience Cloud guest-user baseline]`.

**DevOps.** Keep the existing Azure DevOps plus Gitflow pipeline — the June 2026 Org Health Assessment already rated this pattern aligned with best practice `[extends: Org Health Assessment finding SG-03]`. This engagement adds new automation (routing rules, score locks, escalation flows) that will need a named owner post-go-live; Northwind Grid currently has no Center of Excellence or Design Authority, a gap the Health Assessment flagged at the platform level and this engagement doesn't resolve on its own.

**T-shirt sizing note.** The sizes below express relative complexity, not effort — they're not hour-convertible and aren't meant to be multiplied by a rate to produce a price. For a timeline range, run `roadmap`; for indicative pricing, run `commercials` once a rate is validated.

---

## Solution by Business Process

### Intake & Case Creation (E01)

**Business context.** Applicants submit an Expression of Interest by public web form or email. Today, the web-form path is roughly automated; the email path is not — a sender who doesn't follow up on an auto-reply redirect leaves no record an application was ever attempted, which is the specific failure behind two incidents that reached journalists.

**Solution approach.** Both channels funnel into the same Case object so later de-duplication logic runs once, not twice. Email-to-Case creates a system-of-record case from every inbound email immediately, with any redirect to the web form as a courtesy follow-up rather than the only response `[KA-3884]`. The public web form runs on Form Framework inside an Experience Cloud site — the Public Sector/Grantmaking pattern purpose-built for multi-stage applications, which also sets up the reviewer workspace E04 needs later `[KA-9719]`.

**Supporting architecture.** Case assignment moves from a single static rule to Omni-Channel capacity-based routing, matching the "workload balancing" reality already in place and holding up better under the seasonal spikes that strained the team last year `[KA-4377]`. The exact email-vs-web intake split and the de-duplication rule across channels remain open — see the Discovery Brief's Open Questions.

### Eligibility Screening (E02)

**Business context.** Every EOI is checked against a 2km-of-asset rule and a 2-year re-application policy. The 2km check runs today as a manual lookup against a separate mapping tool, responsible for roughly 40% of eligibility disputes. The exact wording of the 2-year rule is itself unresolved — the requirements document contains two conflicting versions (see the Discovery Brief, question 1).

**Solution approach.** A native Salesforce point-radius proximity check (GEOLOCATION/DISTANCE) against the asset's coordinates automates the 2km rule for the majority of cases `[KA-14019]`. Because Northwind Grid's assets are substations and transmission corridors — not single points — a point-radius check will be an approximation; a true polygon/boundary-layer integration (most likely via Geoscape, once confirmed as the tool of record) is a Phase 2 option if point-radius precision proves insufficient `[assumption: polygon precision requirement — confirm with client]`.

**Supporting architecture.** The eligibility outcome is written with its basis — which criterion passed or failed, or whether it was a special-project override — not just a pass/fail status, directly closing the "no reasoning behind the decision" gap raised in discovery. This epic's scope stays deliberately neutral on the funded-vs-submitted wording conflict until Northwind Grid confirms it.

### Full Application Capture (E03)

**Business context.** Once an EOI is accepted, the applicant completes a longer Full Application — budget breakdown, two competitive quotes, a mandatory co-applicant, and supporting documents. The form today states it cannot be saved partway through.

**Solution approach.** Form Framework again, carrying forward from E01's intake pattern, with the branching logic the form already requires (GST status, amount-changed-since-EOI) built as OmniStudio conditional questions rather than a flat form `[KA-9655]`.

**Supporting architecture.** The co-applicant's attestation ("aware they are part of the application") is captured as a typed-name-and-checkbox pattern rather than a dedicated e-signature product — no central-KB pattern for a two-party e-signature workflow exists on an in-scope cloud, and a grant application doesn't carry the same formality bar as a binding contract `[assumption: confirm formality bar with Legal]`. Whether the form should support saving progress (in tension with its current "no partial save" wording) stays an open question for Northwind Grid to resolve.

### Review & Panel Decisioning (E04)

**Business context.** Applications under $50,000 go to the Community Marketing Lead and Head of Communications; applications over $50,000 go to an 11-person panel, 5 of whom have no Salesforce access today and score via spreadsheet with manual transcription into the CRM — about 40 hours a year of work. Panel-scoring integrity, an immutable record of who scored what and when, is a stated top-three priority.

**Solution approach.** An Experience Cloud "Grantmaking" site gives the 5 external panellists a reviewer workspace directly in the system, replacing the spreadsheet entirely `[KA-9511]`. Licensing is login-based, metered by daily unique logins — a good fit for panellists who review only a few times a year rather than needing a full internal seat `[KA-10444]`. This is a real new license cost to budget, not yet discussed with Northwind Grid.

**Supporting architecture.** Field Audit Trail logs every change to a panel score, but logging alone doesn't *prevent* an edit — so a validation-rule or Flow lock blocks score changes once a case reaches its final decision stage. This resolves the tension in the requirements regardless of which reading of "editable until decision" the business confirms, since both readings agree a score shouldn't change after the decision is made; only the exact trigger point for the lock needs confirming `[assumption: lock-after-decision trigger point]`.

### Grant Disbursement (E05)

**Business context.** Approved grants require a vendor to be set up twice — once in Salesforce, once in SAP — because Finance's accounts-payable team won't pay a vendor not in their vendor master. Entity-name mismatches between what the applicant typed and the official business registry cause roughly a third of applications to bounce back after the applicant has already been told they're approved. This is the single highest-stated priority for this engagement.

**Solution approach.** A record-triggered Flow invoking Salesforce's Generate Payment Instructions action creates the payment request and sends it to SAP; SAP's returned payment ID writes back onto the case so Salesforce always knows the real status — Salesforce's own documented pattern for exactly this ERP hand-off `[KA-9482]`. A pre-submission validation callout against the official business registry catches entity-name mismatches before they ever reach SAP, rather than after `[assumption: registry lookup availability and exact validation ruleset]`.

**Supporting architecture.** With this integration plus the existing Geoscape and EFT-gateway connections, middleware (MuleSoft or an equivalent) is the recommended pattern over point-to-point once the payment gateway itself is identified `[KA-0039]`. Bank, ABN, and tax fields carry Shield Platform Encryption. Whether Salesforce integrates directly with the payment gateway or only with SAP — a real conflict between the requirements and the discovery interview — determines how the "Funded – Payment Held" status actually gets its data, and needs resolving before this integration is built.

### Post-Funding Audit Tracking (E06)

**Business context.** Funded recipients must submit an audit report within two years. Today this is enforced by a manual calendar reminder; the policy is public and the regulator has already asked about it, and a recent compliance review found gaps the organisation couldn't fully explain across five years of grants.

**Solution approach.** A due-date field on the Case, paired with a scheduled Flow, drives a three-step sequence: chase the recipient, escalate to the Community Marketing Lead, then escalate to the Head of Communications if it's still outstanding. A simple scheduled Flow fits a single due-date track better than Entitlement Milestones, which are built for more complex multi-tier SLAs `[assumption: no additional SLA tiers needed beyond the three described]`.

**Supporting architecture.** The requirements document today only describes a single escalation step — building to that literally would under-deliver two of the three steps the business actually wants, so this needs explicit confirmation before development starts (Discovery Brief, question 8). Whether a structured intake mechanism for the audit report itself is in scope, or whether this epic only tracks and escalates on top of however a report happens to arrive, is also still open.
