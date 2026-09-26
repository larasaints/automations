# Zap 1 — New Compliance Submission

## Overview

**Purpose:** Automatically process new franchise compliance submissions, create compliance records, identify reported issues, and escalate urgent issues to the operations team.

**Automation:** UB - New Franchise Compliance Submission

**Platform:** Zapier

**Primary Tools:** Google Forms, Google Sheets, Formatter by Zapier, Gmail

**Trigger:** New compliance submission received through the Google Form

---

## Business Problem

Franchise compliance reports can contain multiple operational checks and may identify issues requiring immediate attention.

Without automation, operations teams would need to:

* Review every submitted compliance report manually.
* Create compliance records.
* Create separate issue records when problems are reported.
* Determine which issues require urgent escalation.
* Notify the appropriate operations team.
* Calculate follow-up deadlines.

Zap 1 automates this process from submission through issue escalation.

---

## Workflow

```text
Google Form
New Compliance Submission
        ↓
Google Sheets
Form Responses 1
        ↓
Zapier Trigger
New Spreadsheet Row
        ↓
Formatter
Format Submission Timestamp
        ↓
Google Sheets
Create Compliance Report
        ↓
Filter
Issues Found = Yes?
        ↓
       YES
        ↓
Formatter
Format Issue Timestamp
        ↓
Google Sheets
Create Issue Record
        ↓
Gmail
Compliance Issue Alert
        ↓
Filter
Priority = Urgent?
        ↓
       YES
        ↓
Gmail
Urgent Escalation
        ↓
Google Sheets
Update Urgent Issue Record
        ↓
Formatter
Calculate Due Date
```

---

## Step-by-Step Workflow

### Step 1 — Trigger: New Compliance Submission

**App:** Google Sheets

**Trigger:** New Spreadsheet Row

**Source:** `Form Responses 1`

The Google Form collects the franchise compliance submission. The responses are automatically stored in the Google Sheets response worksheet.

When a new response creates a row, Zapier starts the workflow.

**Trigger data includes information such as:**

* Franchise ID
* Location
* Manager
* Compliance check results
* Required documents
* Store photos
* Overall status
* Issues found
* Issue description
* Issue priority

---

### Step 2 — Format Submission Timestamp

**App:** Formatter by Zapier

**Tool:** Date/Time

The original submission timestamp is formatted into a consistent value that can be used when creating the compliance record.

**Output format:**

```text
YYYYMMDD-HHmmss
```

This standardized timestamp supports the creation of unique report identifiers.

---

### Step 3 — Create Compliance Report

**App:** Google Sheets

**Action:** Create Spreadsheet Row

**Worksheet:** `Compliance_Reports`

The automation creates a permanent compliance report record using the submitted information.

The record includes fields such as:

* Report ID
* Submission Date
* Franchise ID
* Location
* Manager
* Safety Check
* Inventory Check
* Equipment Check
* Required Documents
* Store Photos
* Overall Status
* Issues Found
* Issue Description
* Issue Priority
* Resolution Status

The generated report ID follows the project's structured naming convention.

**Example:**

```text
RPT-FR-003-20260923-141535
```

---

## Decision Point — Was an Issue Reported?

### Step 4 — Filter

**App:** Filter by Zapier

**Condition:**

```text
Issues Found = Yes
```

If no issue was reported, the issue-management branch does not continue.

If an issue was reported, the automation continues to create an issue record.

This prevents unnecessary issue records from being created for clean compliance submissions.

---

### Step 5 — Format Issue Timestamp

**App:** Formatter by Zapier

**Tool:** Date/Time

The submission timestamp is formatted again for use in the issue record and issue identifier.

**Output format:**

```text
YYYYMMDD-HHmmss
```

---

### Step 6 — Create Issue Record

**App:** Google Sheets

**Action:** Create Spreadsheet Row

**Worksheet:** `Issues`

When a compliance submission identifies an issue, Zapier creates a corresponding issue record.

The issue record stores operational information such as:

* Issue ID
* Report ID
* Franchise ID
* Location
* Category
* Description
* Priority
* Date Reported
* Assigned To
* Status
* Due Date
* Resolution Status
* Escalation Status

**Example issue ID:**

```text
ISS-FR-003-20260923-141535
```

This creates a direct relationship between the compliance report and the issue record.

---

### Step 7 — Compliance Issue Alert

**App:** Gmail

**Action:** Send Email

A notification is sent to the operations team when a compliance submission identifies an issue.

The alert provides the information needed for the operations team to begin reviewing and addressing the issue.

---

## Decision Point — Is the Issue Urgent?

### Step 8 — Filter

**App:** Filter by Zapier

**Condition:**

```text
Priority = Urgent
```

Only urgent issues continue to the escalation branch.

This creates a second level of conditional routing within the automation.

---

### Step 9 — Urgent Escalation

**App:** Gmail

**Action:** Send Email

If the issue is marked as urgent, Zapier sends a separate escalation notification to the operations team.

This ensures urgent compliance issues receive additional visibility instead of relying only on the standard issue notification.

---

### Step 10 — Update Urgent Issue Record

**App:** Google Sheets

**Action:** Update Spreadsheet Row

The urgent issue record is updated using the dynamically created issue row.

This allows the escalation information to remain associated with the correct issue rather than relying on a hard-coded spreadsheet row.

---

### Step 11 — Calculate Follow-Up Due Date

**App:** Formatter by Zapier

**Tool:** Date/Time

The automation calculates the follow-up deadline for urgent issues.

**Calculation:**

```text
Submission Date + 1 day
```

**Output format:**

```text
MM/DD/YYYY
```

The resulting date is used as the urgent issue's follow-up due date.

---

# Data Flow

```text
Google Form
     │
     ▼
Form Responses 1
     │
     ▼
Zapier
     │
     ├──► Compliance_Reports
     │
     └──► Issues
              │
              ▼
        Priority Check
          │       │
       Normal   Urgent
          │       │
          ▼       ▼
       Gmail    Gmail
                Escalation
                  │
                  ▼
              Issue Update
```

---

# Key Automation Logic

### Conditional Routing

The workflow uses two decision points:

**Decision 1**

```text
Issues Found = Yes?
```

This determines whether an issue record should be created.

**Decision 2**

```text
Priority = Urgent?
```

This determines whether an additional escalation notification should be sent.

---

## Record Relationships

The automation creates a relationship between compliance reports and issues.

```text
Compliance Report
RPT-FR-003-20260923-141535
        │
        └──────► Issue
                 ISS-FR-003-20260923-141535
```

This allows operations teams to trace an issue back to the original compliance submission.

---

# Example Scenario

A franchise submits its monthly compliance inspection.

The submission indicates:

```text
Issues Found: Yes
Priority: Urgent
```

Zap 1 automatically:

1. Detects the new submission.
2. Formats the submission timestamp.
3. Creates a compliance report.
4. Detects that an issue was reported.
5. Creates an issue record.
6. Sends a standard compliance issue alert.
7. Detects that the issue is urgent.
8. Sends an urgent escalation email.
9. Updates the corresponding issue record.
10. Calculates the follow-up deadline.

The operations team therefore receives the required notifications without manually monitoring the form responses.

---

# Business Outcome

Zap 1 reduces manual compliance administration by connecting submission intake, record creation, conditional issue routing, notifications, and escalation.

### Operational benefits

* Faster processing of new compliance submissions
* Consistent compliance record creation
* Automatic issue tracking
* Conditional urgent escalation
* Standardized timestamps and identifiers
* Reduced manual spreadsheet updates
* Traceability between compliance reports and issues
* Faster visibility into urgent operational risks

---

# Tools Used

| Tool                    | Role                                           |
| ----------------------- | ---------------------------------------------- |
| **Google Forms**        | Collects compliance submissions                |
| **Google Sheets**       | Stores form responses and operational records  |
| **Zapier**              | Orchestrates the workflow                      |
| **Formatter by Zapier** | Standardizes timestamps and calculates dates   |
| **Filter by Zapier**    | Controls conditional routing                   |
| **Gmail**               | Sends compliance alerts and urgent escalations |

---

## Automation Design Pattern

**Event → Process → Create Record → Evaluate → Route → Notify → Update**

This Zap demonstrates a multi-step operational automation pattern combining data processing, conditional logic, record management, and escalation.


## Next Workflow

**[WORKFLOW ZAP 2 — Issue Resolution and Corrective Action**](https://github.com/larasaints/automations/blob/c51444d2753b3c1e27e7c872d88b67fa26043ed7/urbanbite-franchise-compliance/workflows/zap-02-issue-resolution-and-corrective-action.md)

The next workflow handles the corrective-action side of the process:

**Resolution Form → Find Issue → Update Issue → Record Resolution Date → Record Resolution Notes**
