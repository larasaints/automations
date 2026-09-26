# Zap 3 — Overdue Issue Monitoring & Escalation

## Overview

**Purpose:** Automatically monitor compliance issues that require escalation, consolidate qualifying issues into a single daily escalation report, notify operations, and update the affected issue records.

**Automation:** UB - Overdue Issue Monitoring & Escalation

**Platform:** Zapier

**Primary Tools:** Schedule by Zapier, Google Sheets, Formatter by Zapier, Gmail

**Trigger:** Scheduled daily monitoring at 8:00 AM

---

## Business Problem

Compliance issues can remain unresolved after their assigned due date.

If overdue issues are monitored manually, operations teams may need to repeatedly review the Issues worksheet and determine which records require escalation.

This creates several risks:

* Overdue issues may be overlooked.
* Operations teams may not receive timely escalation.
* Multiple individual notifications can create unnecessary email volume.
* Staff may need to manually identify and update escalated records.
* There may be no clear record of when an issue was escalated.

Zap 3 creates a scheduled monitoring process that automatically identifies issues ready for escalation and produces one consolidated daily report.

---

# Workflow

```text
Schedule by Zapier
Daily — 8:00 AM
        ↓
Google Sheets
Find Issues
Escalation Queue = Ready
        ↓
Line-Item Processing
Prepare Issue Details
        ↓
Gmail
Send Consolidated Escalation Report
        ↓
Google Sheets
Update All Escalated Issue Rows
        ↓
Escalation Status = Escalated
Escalation Date = Current Date/Time
```

---

# Step-by-Step Workflow

## Step 1 — Scheduled Trigger

**App:** Schedule by Zapier

**Trigger:** Every Day

**Schedule:** 8:00 AM

**Time Zone:** Asia/Manila

Zap 3 runs automatically every morning at 8:00 AM.

This creates a recurring compliance-monitoring cycle without requiring a user to manually start the workflow.

The schedule acts as the monitoring trigger for the entire automation.

---

## Step 2 — Find Issues Ready for Escalation

**App:** Google Sheets

**Action:** Lookup Spreadsheet Rows (Advanced)

**Worksheet:** `Issues`

**Lookup Column:**

```text
Escalation_Queue
```

**Lookup Value:**

```text
Ready
```

**Multiple Search Results:** Return all results as line items

The automation searches the Issues worksheet for every issue currently marked as ready for escalation.

This is important because Zap 3 is designed to process **multiple overdue issues in one scheduled run**, rather than sending a separate automation run for each issue.

### Queue-based logic

```text
Escalation_Queue = Ready
        ↓
Include issue in today's escalation report
```

Issues that are not marked `Ready` are excluded from the escalation report.

---

# Step 3 — Prepare Issue Information

Zap 3 uses Formatter by Zapier to convert the returned Google Sheets line items into usable text for the consolidated email.

The workflow formats the following issue fields:

### Formatter 1 — Issue ID

**Source:** Issues Column A

```text
Issue_ID
```

### Formatter 2 — Report ID

**Source:** Issues Column B

```text
Report_ID
```

### Formatter 3 — Franchise ID

**Source:** Issues Column C

```text
Franchise_ID
```

### Formatter 4 — Location

**Source:** Issues Column D

```text
Location
```

### Formatter 5 — Category

**Source:** Issues Column E

```text
Category
```

### Formatter 6 — Priority

**Source:** Issues Column G

```text
Priority
```

### Formatter 7 — Date Reported

**Source:** Issues Column H

```text
Date_Reported
```

### Formatter 8 — Assigned To

**Source:** Issues Column I

```text
Assigned_To
```

### Formatter 9 — Due Date

**Source:** Issues Column K

```text
Due_Date
```

Each Formatter step uses the returned line items from the Google Sheets lookup.

The values are prepared for display as a consolidated operational report.

---

# Step 4 — Consolidated Escalation Report

**App:** Gmail

**Action:** Send Email

The automation sends one consolidated escalation report containing the issues identified during the daily monitoring run.

Instead of sending one email per overdue issue, the workflow groups the qualifying issues into a single operational notification.

### Report information includes:

* Issue ID
* Report ID
* Franchise ID
* Location
* Category
* Priority
* Date Reported
* Assigned To
* Due Date

This gives the operations team the information needed to review and prioritize overdue issues from one email.

---

# Step 5 — Update Escalated Issues

**App:** Google Sheets

**Action:** Update Spreadsheet Row(s)

After the escalation report is sent, the workflow updates all issue rows returned by Step 2.

The row numbers come dynamically from the Google Sheets lookup results.

The workflow updates:

```text
Escalation_Status = Escalated
```

and:

```text
Escalation_Date = Current Date/Time
```

All other issue fields remain unchanged.

---

## Dynamic Row Updating

This is an important part of the workflow design.

Rather than using a hard-coded spreadsheet row, Zap 3 uses the **row number returned by the lookup step**.

For example, if the lookup returns:

```text
Row 4
Row 5
Row 6
Row 7
```

the Update Spreadsheet Row(s) action updates those corresponding issue records.

This allows the automation to work dynamically as the Issues worksheet grows.

---

# Data Flow

```text
Schedule by Zapier
Daily — 8:00 AM
        │
        ▼
Issues Worksheet
        │
        ▼
Escalation Queue = Ready
        │
        ▼
Multiple Matching Issues
        │
        ▼
Formatter
Prepare Report Fields
        │
        ▼
Gmail
Consolidated Escalation Report
        │
        ▼
Google Sheets
Update Matching Issue Rows
        │
        ├── Escalation Status → Escalated
        │
        └── Escalation Date → Current Date/Time
```

---

# Example Scenario

During the daily 8:00 AM monitoring run, the Issues worksheet contains four records with:

```text
Escalation_Queue = Ready
```

Zap 3 identifies all four records.

Instead of sending four separate emails, the automation creates one c
