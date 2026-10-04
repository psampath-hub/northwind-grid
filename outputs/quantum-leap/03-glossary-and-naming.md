# Glossary & Naming — Northwind Grid

> Reference role: **authoritative naming source**. The build agent consults this file every time it's about to invent an object API name, field API name, picklist value, label, or custom permission name.

If a name appears here, **use it verbatim**. If a name doesn't appear here, **ask the user before inventing one**.

## Object naming conventions
Use the following naming patterns. Anything else, **ask the user**.

- Custom objects: `Pascal_Snake_Case__c` (e.g. `Partner_Referral__c`)
- Custom fields: `Pascal_Snake_Case__c` (e.g. `Lead_Score__c`)
- Boolean fields: prefix with `Is_` or `Has_`
- Picklist fields: singular noun (e.g. `Source_Detail__c`, not `Source_Details__c`)

## Field naming conventions
Match the conventions above. When extending a standard object (e.g., Lead, Account, Opportunity), prefix custom fields with the team or feature name (e.g. `Sales_Lead_Score__c`, not just `Score__c`).

## Picklist values
_(No picklist values captured. The build agent must ask the user for picklist values before creating any picklist field. When confirmed, append the values here so subsequent agents can read them.)_

## Custom permissions and permission sets
_(No custom permissions or permission sets named in scope yet. Default convention: one permission set per persona — e.g. `Northwind_Sales_User`, `Northwind_Sales_Manager`. Confirm with user before creating.)_

## Client-specific terms
- **CCF** — Community Care Fund — the grant program being rebuilt
- **CML** — Community Marketing Lead — the 4 regional staff who triage eligibility and chase applications
- **EOI** — Expression of Interest — Stage 1 of the two-stage CCF application process
- **two-kilometre rule** — Eligibility criterion requiring a project be within 2km of a Northwind Grid asset (substation, transmission corridor)
- **special project** — An undocumented, Head-of-Communications-discretion exception allowing re-application within 2 years of prior funding
- **panel** — The 11-person approval body (6 internal, 5 external) that reviews grants over $50K
- **DigiTech** — Northwind Grid's existing Salesforce implementation partner
- Assess EOIs
- Capture Expressions
- Case Creation
- Community Marketing Lead
- Eligibility Screening
- Full Application
- Full Application Capture
- Funding Audit Tracking
- Grant Disbursement
- Panel Decisioning
- Payment Held

---

## What's missing here

This file was generated from the Scopezilla scope. It is **not exhaustive** — it captures the names that appeared explicitly in epics, stories, and discovery. When the build agent needs a name not listed here, it must ask the user to confirm before proceeding.
