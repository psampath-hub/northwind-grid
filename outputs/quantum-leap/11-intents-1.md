# Intent Statements — Phase 1 (Northwind Grid)

> Reference role: the **load-bearing build target** for Phase 1. Each intent below is one capability — one firing trigger or user action, one outcome, one walkthrough. Build one at a time. The phase brief (`10-phase-1.md`) is orchestration; this file is what to build.
>
> **For architects:** walk these with the customer to assign priority and answer open questions. Edit `data/intents.json` (canonical) or this file directly — the next quantum-leap run re-renders from JSON.

## INT-001 — Capture an Expression of Interest through every intake channel without losing it

epic `E01` · priority _(unassigned)_ · confidence _Assumed_ · surface `automation`

### 1. Outcome

Every inbound application attempt -- by web form or email -- produces a trackable case immediately, closing the gap that has already caused two lost-application incidents reported to journalists.

### 2. Build target

- The public Expression of Interest web form creates a case on submission
- An inbound email to any of the fund's known intake addresses creates a case immediately, independent of any auto-reply
- A courtesy auto-reply may still redirect an emailed applicant to the web form, but it never substitutes for case creation
- The case stores which channel it arrived through

### 3. Guardrails

- Must not rely on the applicant acting on a redirect for a case to exist.
- Must capture the raw inbound content (web fields or email body) on the case before any further processing.

### 4. Out of scope

- Must not resolve which specific email addresses count as fund intake addresses -- that list is a client input, not a build decision.
- Must not implement cross-channel duplicate detection (handled by INT-002).

### 5. Acceptance

A community group emails the fund's shared inbox asking about a grant, without using the web form. A Community Marketing Lead opens the case queue the same day and finds a new case already created from that email, with the original message attached.

### 6. Dependencies

- **External:** Public-facing EOI web form — confirmed list of fund intake email addresses and the web form's field-to-case mapping _(owner: Northwind Grid Communications team)_

### 7. Grounding

- **Requirements traced:** REQ-1, REQ-2, REQ-4

### Open questions

- [ ] **Q-001** — What are all the email addresses applicants currently use to reach the fund (shared inbox, direct staff emails, forum contacts)? (Resolver: Rachel Doyle / Communications team)
- [ ] **Q-002** — Should the courtesy auto-reply to an email submission still redirect to the web form, or is a direct confirmation preferred now that the email itself creates a case? (Resolver: Rachel Doyle)

---

## INT-002 — Route a new application to a Community Marketing Lead without manual triage

epic `E01` · priority _(unassigned)_ · confidence _Assumed_ · surface `automation`

### 1. Outcome

A new case is assigned to a Community Marketing Lead automatically, balanced across the team's real capacity rather than a single static rule -- so a seasonal spike doesn't concentrate load on one person the way it did last year.

### 2. Build target

- Each new case is assigned to one of the four Community Marketing Leads based on current workload, not a fixed geography rule
- Applications arriving via different channels for the same underlying applicant/project are flagged as likely duplicates for the assigned lead to review, rather than auto-merged

### 3. Guardrails

- Must not route purely on an applicant's address if territory data isn't reliable -- the current geography-based routing is already known to be informal.

### 4. Out of scope

- Must not auto-merge duplicate cases without human review.

### 5. Acceptance

During a bushfire-season surge, 60 EOIs arrive in two days. A Community Marketing Lead who was already handling a heavy caseload receives noticeably fewer new assignments than a colleague with a lighter load that week.

### 6. Dependencies

- **Internal (build first):** INT-001

### 7. Grounding

- **Requirements traced:** REQ-5, REQ-8

### Open questions

- [ ] **Q-003** — What should count as a 'likely duplicate' across channels -- same organisation name, same project address, same contact email? (Resolver: Rachel Doyle + a Community Marketing Lead)

---

## INT-003 — Check an application against the 2km proximity rule automatically

epic `E02` · priority _(unassigned)_ · confidence _Assumed_ · surface `automation`

### 1. Outcome

The 2km-of-asset eligibility check runs automatically against stored asset locations, removing the manual external-tool lookup responsible for roughly 40% of today's eligibility disputes.

- **Baseline:** ~40% of eligibility disputes trace to the 2km boundary check
- **Target:** materially reduced dispute rate on this specific criterion
- **Window:** first full review cycle post go-live

### 2. Build target

- An application's stated project location is checked against Northwind Grid's known asset locations
- The system records a pass/fail result for this criterion plus the measured distance, visible to the reviewer
- A project close to the boundary (within a defined margin) is flagged for manual confirmation rather than auto-failed

### 3. Guardrails

- Must not silently auto-reject a borderline case -- flag it for human review instead.
- Must record the distance measured, not just pass/fail, so a disputed result can be explained.

### 4. Out of scope

- Must not build a true polygon/boundary-shaped check for transmission corridors in this phase -- start with a point-radius check against asset coordinates; a boundary-shaped upgrade is a later decision if point-radius accuracy proves insufficient.
- Must not resolve which external mapping tool (if any) the proximity check should integrate with -- confirm the asset-location data source first.

### 5. Acceptance

A community group submits an EOI for a project 1.9km from a substation. The eligibility check records a pass with the measured distance visible on the case, instead of a Community Marketing Lead needing to manually look it up in a separate tool.

### 7. Grounding

- **Requirements traced:** REQ-9

### Open questions

- [ ] **Q-004** — Is Geoscape confirmed as the authoritative source of Northwind Grid asset location data for this check, and in what format/location does that data live today? (Resolver: Rachel Doyle + a Community Marketing Lead)
- [ ] **Q-005** — What margin of distance (e.g. within 100m of the 2km line) should trigger manual review instead of an automatic pass/fail? (Resolver: Rachel Doyle)

---

## INT-004 — Enforce the 2-year re-application rule once its exact wording is confirmed

epic `E02` · priority _(unassigned)_ · confidence _Unknown_ · surface `automation`

### 1. Outcome

An applicant's eligibility for re-application within 2 years of a prior application is checked consistently against one confirmed rule, rather than two conflicting readings in the source documents.

### 2. Build target

- The system checks an applicant's case history for a prior application within the last 2 years
- The eligibility outcome records which specific rule reading was applied and why
- A special-project exception, once the business defines its criteria, can override this check with a logged reason

### 3. Guardrails

- Must not pick between the 'funded' and 'submitted' readings of the 2-year rule unilaterally -- this build target cannot be finalized until Northwind Grid confirms which one is correct.

### 4. Out of scope

- Must not define what qualifies as a 'special project' exception -- that policy decision belongs to Northwind Grid and Legal, not this build.

### 5. Acceptance

An applicant who was rejected (not funded) 18 months ago re-applies. Depending on which rule Northwind Grid confirms, the system either passes or fails this applicant consistently, with the specific rule and reasoning visible on the case.

### 6. Dependencies

- **Internal (build first):** INT-003

### 7. Grounding

- **Requirements traced:** REQ-9

### Open questions

- [ ] **Q-006** — Does the 2-year re-application rule block an applicant whose prior application was merely submitted, or only one that was actually funded? (Resolver: Rachel Doyle + Legal)

---

## INT-005 — Capture the Full Application with its real conditional-field logic

epic `E03` · priority _(unassigned)_ · confidence _Confirmed_ · surface `experience-cloud`

### 1. Outcome

An eligible applicant completes the Full Application -- including GST-conditional funding fields, an amount-changed-since-EOI check, and the required co-applicant and document checklist -- without the conditional logic being guessed at by the build.

### 2. Build target

- The GST status question branches to the correct funding-amount field
- If the requested amount changed since the EOI, the applicant explains what changed, and the system compares the new quotes against the new amount
- The applicant provides a co-applicant's name, position, email, and phone, with a declaration the co-applicant is aware of their inclusion
- The system enforces the document checklist: two competitive quotes (or a justification for one), with letters of support and other documentation optional

### 3. Guardrails

- Must capture the co-applicant's details even though no independent verification step exists yet for their consent.

### 4. Out of scope

- Must not implement a dedicated e-signature product for the co-applicant attestation -- a typed-name-and-checkbox pattern is sufficient unless Legal says otherwise.
- Must not support partial-save/resume on this form in this phase -- the current form explicitly states it cannot be saved partially; revisit only if Northwind Grid confirms otherwise.

### 5. Acceptance

An applicant whose requested amount increased since their EOI completes the Full Application, explains the change, and the system flags for the reviewer whether the two new quotes sum to the new requested amount.

### 6. Dependencies

- **Internal (build first):** INT-003

### 7. Grounding

- **Requirements traced:** REQ-12, REQ-13, REQ-14, REQ-15

### Open questions

- [ ] **Q-007** — Should the Full Application support saving progress and resuming later, given its length and document requirements, despite the current form's 'no partial save' wording? (Resolver: Rachel Doyle + Legal)

