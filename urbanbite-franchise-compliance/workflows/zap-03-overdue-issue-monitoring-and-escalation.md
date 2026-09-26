# Zap 3 — Overdue Issue Monitoring & Escalation

## Overview

Zap 3 is the daily overdue compliance monitoring and escalation workflow for the UrbanBite franchise compliance system.

The workflow is designed to help HQ monitor compliance issues that remain unresolved after their expected due date.

It runs automatically every morning, retrieves the relevant issue information from Google Sheets, processes the data through multiple Formatter steps, creates a structured escalation report, sends the report to HQ, and updates the issue record.

This workflow demonstrates **scheduled monitoring, business-rule-driven exception handling, multi-field data transformation, automated communication, and operational tracking.**

---

# Business Problem

UrbanBite's HQ Operations Team should not have to manually review the Issues sheet every day to determine which franchise compliance issues require escalation.

As the number of franchise locations and compliance issues increases, manual monitoring can create operational risks:

- Overdue issues may be missed.
- HQ must repeatedly review issue records.
- Escalation depends on manual follow-up.
- Important issue information may need to be manually compiled.
- Escalation records may not be updated consistently.

UrbanBite needs a repeatable process that turns overdue compliance information into an actionable HQ escalation.

---

# Business Solution

Zapier automates the daily monitoring and escalation workflow.

Every morning at 8:00 AM, the workflow:

1. Starts automatically using Schedule by Zapier.
2. Looks up the relevant compliance issue information in Google Sheets.
3. Processes the issue information through **9 Formatter by Zapier — Utilities: Line Item to Text steps**.
4. Builds a structured escalation report.
5. Sends the report to HQ through Gmail.
6. Updates the corresponding issue record in Google Sheets.

---

# Workflow

```text
Schedule by Zapier
        ↓
Google Sheets — Lookup Spreadsheet Row
        ↓
Formatter — Utilities — Line Item to Text #1
        ↓
Formatter — Utilities — Line Item to Text #2
        ↓
Formatter — Utilities — Line Item to Text #3
        ↓
Formatter — Utilities — Line Item to Text #4
        ↓
Formatter — Utilities — Line Item to Text #5
        ↓
Formatter — Utilities — Line Item to Text #6
        ↓
Formatter — Utilities — Line Item to Text #7
        ↓
Formatter — Utilities — Line Item to Text #8
        ↓
Formatter — Utilities — Line Item to Text #9
        ↓
Gmail — Send Email
        ↓
Google Sheets — Update Spreadsheet Row
