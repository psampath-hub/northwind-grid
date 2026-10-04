# Northwind Grid — Community Care Fund: Executive Summary

## At a Glance

- **Current state pain:** Applications are growing 8x since 2019 while eligibility, panel scoring, and audit tracking still run on manual processes and spreadsheets, with no defensible record for a regulator already asking questions.
- **Transformation vision:** By the time the fund reaches 1,000 applications a year, every eligibility decision, panel score, and audit follow-up is defensible on demand, and a recipient gets paid without the weeks-long paperwork round-trip they face today.
- **Top value drivers:** Eliminating duplicate SAP vendor setup and manual panel-score transcription; building a defensible audit trail; giving external reviewers a proper workspace instead of a spreadsheet.
- **Biggest risk or unknown:** The disbursement integration's exact design — whether Salesforce talks directly to the payment gateway or only to SAP — isn't yet confirmed, and it's the single largest driver of this program's timeline.
- **Recommended first step:** A short discovery phase to resolve eight conflicting requirements and confirm the disbursement integration before committing to a build sequence.

---

## Overview

The Community Care Fund already runs on Salesforce. This program extends that platform rather than replacing it, closing three gaps the business has named as priorities: a validated data exchange with the finance system that eliminates duplicate vendor paperwork, a real audit-tracking mechanism that replaces a manual calendar reminder, and a tamper-evident record of panel-review scoring. The board has approved doubling fund volume by 2028, and today's manual processes are already straining at current volume — that growth target is why this work is time-sensitive rather than optional.

## Scope Summary

Six epics span the fund's full lifecycle: intake and case creation, eligibility screening, full application capture, panel review and decisioning, grant disbursement, and post-funding audit tracking. The three highest-priority fixes — the finance-system integration, audit tracking, and panel-scoring integrity — sit inside this scope and are sequenced early in the delivery plan.

## Solution Highlights

The architecture builds on Salesforce's standard pattern for exactly this kind of program: an Experience Cloud site gives community applicants and the fund's external review panel a dedicated workspace, replacing the public web form and the panel's current spreadsheet workaround in one move. A validated data exchange checks applicant details against the finance system before a payment request is ever sent, closing the mismatch problem that currently bounces roughly a third of approved applications back for rework. Panel scores pick up a permanent change history, so a decision can be defended after the fact rather than reconstructed from memory.

## Implementation Approach

The program is phased in four stages. A short discovery phase resolves outstanding conflicts in the current requirements and confirms the disbursement integration's design before any build work starts. From there, the program hardens the core application process, then delivers the finance-system integration as its own phase given its size, then closes with panel-scoring integrity, the external reviewer workspace, and audit tracking. A benchmark-based estimate puts the full program at 13 to 28 weeks; this is a planning range grounded in comparable implementations, not a committed delivery date.

## Risks and Mitigations

The disbursement integration carries the most risk: its design depends on confirming how Salesforce and the finance system's payment gateway actually exchange data, a detail not yet settled with the finance team. Resolving this in the discovery phase, before build work on that piece begins, keeps it from becoming a late-stage surprise. A second risk sits with the external review panel: giving panellists access to a new system requires licensing to be budgeted and a short onboarding effort for reviewers who use the system only a few times a year. Both risks are named and sequenced early rather than discovered mid-build.

## Assumptions and Confidence Level

Two of the six epics in scope are confirmed against the current requirements; the remaining four carry assumptions that discovery is expected to close, and one carries a genuine open question on its technical approach. This pattern is typical for an enhancement to a live, in-use process: the gaps are concentrated in exactly the areas this program exists to fix, not spread evenly across stable, working parts of the system. A discovery pass focused on the open requirements conflicts closes most of this before build work begins.

## Next Steps and Recommendations

Resolve the open requirements conflicts and confirm the disbursement integration's design in the discovery phase outlined above. From there, the program is ready to move into delivery planning, including the named team and roles needed to execute it.

---

*This document contains forward-looking statements regarding implementation timelines, expected outcomes, and capabilities. Actual results depend on factors including but not limited to client readiness, decision velocity, scope finalization, and integration partner availability.*

*This duration range is a benchmark-derived estimate based on general implementation patterns and is not a committed delivery timeline. Actual duration depends on team composition, client responsiveness, data quality, and scope changes discovered during delivery.*

*All Salesforce product names are trademarks of Salesforce, Inc.*
