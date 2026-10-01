<!-- Source: discovery-notes/Community Care Fund - High Level Requirements.xlsx · Retrieved: 2026-07-24 · Via: openpyxl conversion -->

# Community Care Fund — High Level Requirements

Converted from the "High Level Requirements" sheet of `Community Care Fund - High Level Requirements.xlsx`. Requirements retain their original numbering. Feature numbers are cleaned up (the source has floating-point drift, e.g. `1.9999999999999998` → `2`). A second sheet, "Table format", is an earlier variant without requirement numbers and with slightly different eligibility wording — noted at the end.

---

## Capability 1 — Fund Application Intake

### Feature 1 — Expression of Interest application

- **REQ-1** — The system shall allow applicants to submit an EOI form electronically, directly available in the public application website.
  - **Business rule:** The Expression of Interest (EOI) form shall contain approximately five mandatory questions.
- **REQ-2** — The system shall, alternatively, receive EOI applications submitted via email and as a response redirect applicants to the public application website to submit an EOI form.
- **REQ-3** — The system should enable applicants to attach files (if required) to the EOI form.

### Feature 2 — Case creation and Management

- **REQ-4** — The system shall automatically create a case for the application submitted.
- **REQ-5** — The system shall apply case management rules and assign the case to Community Marketing Lead (CML).
- **REQ-6** — The system shall allow the reviewers to reach out to the applicant via email in case more information is required.
- **REQ-7** — The applicant should be able to provide the additional information through email and the reviewer should be able to attach it to the case.
- **REQ-8** — The system shall aid the CML to manually link a case to an existing account/contact or a new account/contact created. The aid could be in the form of suggested matches.

---

## Capability 2 — EOI Review and Screening

### Feature 3 — Eligibility check

- **REQ-9** — The system shall support assessment of whether the applicant is eligible for the fund and flag if any criteria are not met.
  - **Business rule — Eligibility Criteria Definition.** An application is considered eligible only if all of the following conditions are met:
    1. The project location is within 2 km of the defined boundary.
    2. No previous application has been funded within the last 2 years.
    3. As an exception, if the application qualifies as a special project type they may reapply for a fund within the 2 years.
    4. Other required funding has been confirmed as met.
    5. A previous audit report has been submitted IF previously successful.
  - **Business rule — Status Update, Eligibility Passed.** If the application satisfies all eligibility criteria, the system shall set the application status to **Pending Accepted**.
  - **Business rule — Status Update, Eligibility Failed.** If the application fails to satisfy any of the eligibility criteria, the system shall set the application status to **Pending Rejected**.

### Feature 4 — EOI assessment outcome

- **REQ-10** — The system shall allow the CML to manually accept or reject an application after it is finally reviewed by them.
  - **Business rule — EOI Acceptance.** If the CML manually accepts an application, the system shall update the application status to **EOI Accepted**.
  - **Business rule — EOI Rejection.** If the CML manually rejects an application, the system shall update the application status to **EOI Rejected**.
- **REQ-11** — The system shall notify applicants of the EOI decision outcome via email.
  - **Business rule:** If EOI is accepted, the invite from REQ-12 should be included here.

---

## Capability 3 — Full application submission

### Feature 5 — Invite full application

- **REQ-12** — The system shall invite eligible applicants to submit a full application.
  - **Business rule — Transition to Full Application.** If the defined progression criteria are met and the EOI is accepted, the system shall update the case status to **Full Application**.
- **REQ-13** — The system shall send an email to the applicant with the full application form link.

### Feature 6 — Full application capture

- **REQ-14** — The system shall allow applicants to submit a full application with required details and documents.
  - **Business rule — Application Received Status.** If the full application is submitted and successfully received by the system, the system shall update the case status to **Application Received**.
- **REQ-15** — The system shall support uploading and storing supporting documentation.

---

## Capability 4 — Full Application Review

### Feature 7 — Full application assessment

- **REQ-16** — The system shall trigger an alert to the Head of Comms to review the application.
  - **Business rule:** Possibly reassign the case to them as well.
- **REQ-17** — The system shall enable assessment of full applications against funding criteria.
- **REQ-18** — The system shall allow assessors to request additional information from applicants.
- **REQ-19** — The system shall enable the assessor to shortlist or not shortlist an application.
  - **Business rule — Shortlisting Decision.** If an application is shortlisted, the system shall update the case status to **Shortlisted**. If an application is not shortlisted, the system shall update the case status to **Pending Rejected**.

### Feature 8 — Full application Shortlist

- **REQ-20** — The system shall further route shortlisted applications to relevant individual review by panel members.
- **REQ-21** — The system shall allow each panel member to add a score to each application.
  - **Business rule:** Application scores should remain editable until accepted or rejected.
- **REQ-22** — The system shall allow the assessors to generate a report or filter the applications to view a filtered set of applications.
- **REQ-23** — The system shall allow the panel to approve or reject an application on the basis of a panel discussion reviewing all the collated scores.
  - **Business rule — Application Decision.** If an application is approved, the system shall update the case status to **Application Accepted**. If an application is rejected, the system shall update the case status to **Application Rejected**.

### Feature 9 — Applicant outcome communication

- **REQ-24** — The system shall notify applicants of approval and support requests for additional information from applicants where required via email.
- **REQ-25** — The system shall notify applicants of rejection in a rejected scenario after all applications are processed.

---

## Capability 5 — Fund Management

### Feature 10 — Grant disbursement (outbound payments)

This is **disbursement** — paying the awarded grant to the successful recipient. Not the collection of any funds. The CRM initiates and tracks; the actual money movement happens in the corporate ERP and the connected payment disbursement service.

- **REQ-26** — The system shall enable initiation of a grant disbursement once a funding agreement is executed with the recipient.
  - **Business rule — Funding Initiated.** If disbursement is initiated for an application, the system shall update the case status to **Funded**.
- **REQ-27** — The system shall capture the recipient's payment details (registered legal entity name, bank account, tax reference) at award time and hold them against the case.
  - **Business rule:** Payment details are captured once and re-used for any staged or split disbursements against the same grant.
- **REQ-28** — The system shall push a payment request to the corporate ERP (accounts payable) covering: recipient, amount, payment date, purpose / grant reference, and GL / cost-centre coding.
  - **Business rule:** Every disbursement in the CRM must have a matching payable in the ERP before funds are released. No back-door payments.
- **REQ-29** — The system shall integrate with the payment disbursement service (bank / EFT gateway) to release funds against the ERP-approved payable and receive a settlement acknowledgement.
  - **Business rule:** The system does not itself move money — it triggers the disbursement service and records the outcome.
- **REQ-30** — The system shall receive and record the disbursement outcome (**Settled** / **Failed** / **Pending**) against the case, and update case status accordingly.
  - **Business rule — Disbursement Settled.** If the payment settles successfully, case status remains **Funded** and the settlement reference is stored on the case.
  - **Business rule — Disbursement Failed.** If the payment fails (e.g. bank rejection, invalid details), the system shall flag the case for CML follow-up and set the case status to **Funded – Payment Held**.
- **REQ-31** — The system shall support staged / milestone-based disbursements against a single grant, where the funding agreement schedules multiple releases.
  - **Business rule:** Each stage is a separate ERP payable and a separate disbursement — the case tracks progress across the full schedule until all stages settle.

### Feature 11 — Post-Funding Reporting

- **REQ-32** — The system shall support collection of post-funding audit reports from recipients.
- **REQ-33** — The system shall enable monitoring of funding usage against agreed purposes.
  - **Business rule — Case Closure on Audit Completion.** If the audit report is submitted and marked as complete (ticked), the system shall update the case status to **Closed**.
- **REQ-34** — The system shall wait for 2 years once a fund is processed to an applicant and, if the audit report is not sent by the customer, an alert is sent to the CML.

---

## Capability 6 — Enterprise Governance, Quality & Scalability

### Feature 12 — Governance & Compliance

- **REQ-35** — The solution shall align with organisational digital, security, and information governance standards.

### Feature 13 — Data Quality Management

- **REQ-36** — The solution shall support ongoing data quality management.

### Feature 14 — Quality Assurance & Validation

- **REQ-37** — The solution shall support controlled testing and validation prior to live communications.

### Feature 15 — Operational Efficiency

- **REQ-38** — The solution shall minimise reliance on specialist technical resources for day-to-day operations.

### Feature 16 — Platform Evolution

- **REQ-39** — The solution shall support incremental enhancement over time.

---

## Capability 7 — Consent, Privacy & Audit Compliance

### Feature 17 — Regulatory Compliance

- **REQ-40** — The solution shall ensure compliance with applicable consent, privacy, and electronic communications regulations.

### Feature 18 — Audit & Assurance

- **REQ-41** — The solution shall support audit and assurance activities where required.

---

## Capability 8 — Operating Model, Ownership & Usability

### Feature 19 — Platform Governance & Ownership

- **REQ-42** — The solution shall support clearly defined ownership for platform configuration, content, data, and compliance activities.

### Feature 20 — Data Stewardship & Accountability

- **REQ-43** — The solution shall support defined data stewardship and accountability for ongoing data quality.

### Feature 21 — Operational Transition

- **REQ-44** — The solution shall support transition to business-as-usual operations post-implementation.

### Feature 22 — User Access & Usability

- **REQ-45** — The solution shall be usable by business users with designated access.
  - **Business rule:** Access must be easily grantable and across existing CRM profiles.

---

## Notes on the second sheet ("Table format")

The source workbook has a second sheet, `Table format`, which is an earlier variant of the same content — no requirement numbers, no feature numbers, and a few substantive differences worth flagging:

- **REQ-9 (Eligibility)** — earlier eligibility list reads:
  1. Project location within 2 km of the defined boundary
  2. **No previous application has been *submitted* within the last 2 years** (main sheet says "*funded*")
  3. Other required funding confirmed as met
  4. **The application qualifies as a special project type** (main sheet frames this as an *exception* to rule 2, not a criterion)
  5. A previous audit report has been submitted (main sheet qualifies "IF previously successful")
- **REQ-16 (Head of Comms alert)** — the "Possibly reassign the case to them as well" note is absent in the Table-format sheet.
- **REQ-21 (Panel scoring)** — the editability rule ("scores remain editable until accepted or rejected") is absent.
- **REQ-11 (EOI notification)** — the "include the REQ-12 invite" note is absent.

The main "High Level Requirements" sheet supersedes it — treat this Markdown as canonical.
