# Intent Statements — Phase 2 (Northwind Grid)

> Reference role: the **load-bearing build target** for Phase 2. Each intent below is one capability — one firing trigger or user action, one outcome, one walkthrough. Build one at a time. The phase brief (`10-phase-2.md`) is orchestration; this file is what to build.
>
> **For architects:** walk these with the customer to assign priority and answer open questions. Edit `data/intents.json` (canonical) or this file directly — the next quantum-leap run re-renders from JSON.

## INT-006 — Validate entity and bank details against the official business registry before sending a payment request to SAP

epic `E05` · priority _(unassigned)_ · confidence _Unknown_ · surface `integration`

### 1. Outcome

An applicant's entered entity name is checked against the official business registry before a vendor-setup request ever reaches SAP, eliminating the entity-name mismatch that bounces roughly a third of approved applications back to the applicant after they've already been told they're approved.

- **Baseline:** ~1/3 of applications bounce on entity-name mismatch during SAP vendor setup
- **Target:** mismatch bounce-back rate reduced toward zero
- **Window:** first full disbursement cycle post go-live

### 2. Build target

- When a Full Application captures entity and bank details, the entity name is checked against the official business registry
- A mismatch is flagged to the applicant for correction before the application proceeds to disbursement, not after
- Once validated, the entity and bank details are reused for any staged or split disbursements against the same grant

### 3. Guardrails

- Must not send unvalidated entity/bank data onward to SAP.

### 4. Out of scope

- Must not define the exact match tolerance (exact string match vs. fuzzy match) -- that ruleset needs confirming before this can be fully built.
- Must not re-validate bank details automatically on every staged payment -- flag staleness only if a defined time threshold is set.

### 5. Acceptance

An applicant types 'The Foo Community Trust' as their entity name, but the official registry records 'Foo Community Trust Incorporated'. The system flags the mismatch to the applicant for correction before the award proceeds, instead of after SAP rejects the vendor setup.

### 7. Grounding

- **Requirements traced:** REQ-26, REQ-27

### Open questions

- [ ] **Q-008** — What validation ruleset is intended -- exact match against the registry, a fuzzy-match tolerance, or a live lookup confirmed interactively with the applicant? (Resolver: Rachel Doyle + Finance/AP)

---

## INT-007 — Send a validated payment request to SAP and track its vendor-setup outcome

epic `E05` · priority _(unassigned)_ · confidence _Unknown_ · surface `integration`

### 1. Outcome

A validated grant award generates a single payment request to SAP, with SAP's resulting vendor/payment identifier written back to the case -- eliminating the duplicate manual vendor setup Finance and the fund team each do today.

### 2. Build target

- Once a grant is approved and the funding agreement is signed, the system generates a payment request to SAP covering recipient, amount, date, purpose, and GL/cost-centre coding
- SAP's returned payment/vendor identifier is written back onto the case
- The system prevents a duplicate payable from being created if the request is resent after a timeout

### 3. Guardrails

- Must not allow a disbursement to exist in Salesforce with no matching payable in SAP -- no back-door payments.
- Must guard against creating two SAP payables for one Salesforce disbursement record on a retried request.

### 4. Out of scope

- Must not resolve whether Salesforce derives GL/cost-centre coding automatically or Finance appends it manually -- confirm with Finance/AP first.

### 5. Acceptance

A $75,000 grant is approved and the agreement signed. The case shows a single payment request sent to SAP, and once SAP processes it, the case displays SAP's vendor-setup confirmation -- with no second, duplicate request created even if the integration call is retried.

### 6. Dependencies

- **Internal (build first):** INT-006
- **External:** SAP (vendor master, Accounts Payable) — a confirmed integration surface for receiving payment requests and returning vendor-setup outcomes _(owner: Northwind Grid Finance / Accounts Payable)_

### 7. Grounding

- **Requirements traced:** REQ-26, REQ-28

### Open questions

- [ ] **Q-009** — Does Salesforce derive GL/cost-centre coding automatically, or does Finance/AP append it manually after receiving the request? (Resolver: Finance/AP)

---

## INT-008 — Reflect the real payment outcome on the case, including a genuine held-payment state

epic `E05` · priority _(unassigned)_ · confidence _Unknown_ · surface `integration`

### 1. Outcome

A case shows the true settlement outcome of a disbursement -- settled, pending, or genuinely held on failure -- so the fund team and recipient both know a payment has failed within days, not three weeks after the recipient calls.

### 2. Build target

- The case receives the disbursement outcome (settled / failed / pending) from whichever system actually reports it
- A failed disbursement sets the case to a real 'Funded -- Payment Held' state, distinct from a successful 'Funded' state
- A held payment notifies the Community Marketing Lead, with escalation if it isn't addressed

### 3. Guardrails

- Must not leave a case showing 'Funded' once a payment has actually failed.

### 4. Out of scope

- Must not build this integration until the topology is confirmed -- whether Salesforce integrates directly with the EFT/payment gateway, or only with SAP (which would then need to relay the outcome back). This is the single largest open item blocking this intent's build.

### 5. Acceptance

A recipient's bank account details are rejected by the payment gateway. Within the same business day, the case shows 'Funded -- Payment Held' and the assigned Community Marketing Lead has received a notification -- rather than the recipient discovering the failure by calling three weeks later.

### 6. Dependencies

- **Internal (build first):** INT-007
- **External:** AP-owned EFT/payment gateway (identity not yet confirmed) — confirmation of which system (SAP or the gateway directly) reports settlement outcomes to Salesforce, and what integration surface it exposes _(owner: Northwind Grid Finance / Accounts Payable)_

### 7. Grounding

- **Requirements traced:** REQ-29, REQ-30

### Open questions

- [ ] **Q-010** — Does Salesforce integrate directly with the EFT/payment gateway, or only with SAP (which relays the outcome)? What is the gateway's identity? (Resolver: Rachel Doyle + Finance/AP)

---

## INT-009 — Track staged/milestone disbursements and migrate the existing spreadsheet

epic `E05` · priority _(unassigned)_ · confidence _Assumed_ · surface `automation`

### 1. Outcome

Staged grant payments (signing / milestone / completion) are scheduled and reminded inside Salesforce, retiring the spreadsheet on one person's laptop that is today the only record of ~20 active staged grants.

### 2. Build target

- A grant with a staged payment schedule (e.g. 50% / 25% / 25%) records each stage as a separate disbursement tied to the same grant
- The system reminds the assigned Community Marketing Lead ahead of each upcoming milestone date
- The ~20 currently active staged grants are migrated from the existing spreadsheet into this structure before go-live

### 3. Guardrails

- Must not go live without migrating the ~20 existing staged grants -- otherwise their schedules are lost.

### 4. Out of scope

_(no explicit non-goals captured)_

### 5. Acceptance

A grant with a milestone due in 10 days generates a reminder to the assigned Community Marketing Lead automatically, for a grant that was migrated from the prior spreadsheet and already has two of its three stages recorded.

### 6. Dependencies

- **Internal (build first):** INT-007

### 7. Grounding

- **Requirements traced:** REQ-31

### Open questions

_(no open questions captured)_

