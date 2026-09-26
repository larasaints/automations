# Zap 2 — Issue Resolution & Corrective Action

## Overview

**Purpose:** Automatically process issue-resolution submissions, locate the corresponding issue record, update its resolution information, and send a follow-up notification when additional action is required.

**Automation:** UB - Issue Resolution & Corrective Action

**Platform:** Zapier

**Primary Tools:** Google Forms, Google Sheets, Formatter by Zapier, Gmail

**Trigger:** New Issue Resolution Form Response

---

## Business Problem

Once a compliance issue has been identified, the operations team needs a reliable way to record the resolution and determine whether additional follow-up is required.

Without automation, staff may need to:

* Search for the correct issue manually.
* Update the issue record.
* Record the resolution date.
* Add resolution notes and corrective actions.
* Determine whether additional follow-up is required.
* Send a follow-up notification.
* Manually track whether the notification was already sent.

Zap 2 connects the issue-resolution form directly to the existing issue record and automates the follow-up process.

---

## Workflow

```text
Issue Resolution Google Form
        ↓
New Spreadsheet Row
        ↓
Google Sheets
Lookup Issue by Issue_ID
        ↓
Formatter
Format Resolution Timestamp
        ↓
Google Sheets
Update Existing Issue Record
        ↓
Filter
Follow-Up Required = Yes?
        ↓
       YES
        ↓
Gmail
Follow-Up Alert
        ↓
Google Sheets
Mark Follow-Up Alert Sent = Yes
```

---

## Step-by-Step Workflow

### Step 1 — Trigger: New Issue Resolution Submission

**App:** Google Sheets

**Trigger:** New Spreadsheet Row

**Source:** `Issue Resolution` Google Form response sheet

When an issue-resolution form is submitted, the response is added to the connected Google Sheets worksheet.

This new row triggers Zap 2.

The form provides the information needed to identify the original issue and record the resolution.

Typical information includes:

* Issue ID
* Resolution information
* Resolution notes
* Corrective action
* Follow-up requirement
* Resolution status

---

### Step 2 — Find the Existing Issue

**App:** Google Sheets

**Action:** Lookup Spreadsheet Row

**Lookup Field:** `Issue_ID`

The automation uses the Issue ID submitted through the resolution form to locate the existing issue in the `Issues` worksheet.

This prevents the workflow from creating a duplicate issue.

Instead, it connects the resolution submission to the issue record that was originally created by Zap 1.

### Record relationship

```text
Original Compliance Submission
          ↓
     Issue Record
     ISS-FR-003-...
          ↑
          │
Issue Resolution Form
```

---

### Step 3 — Format Resolution Timestamp

**App:** Formatter by Zapier

**Tool:** Date/Time

The resolution timestamp is converted into a standardized date and time format before being written to the issue record.

**Output format:**

```text
MM/DD/YYYY HH:mm:ss
```

This keeps resolution records consistent within the `Issues` worksheet.

---

### Step 4 — Update the Existing Issue

**App:** Google Sheets

**Action:** Update Spreadsheet Row

The issue found in Step 2 is updated dynamically using the matching Issue ID.

The automation can update information such as:

* Resolution Date
* Resolution Notes
* Corrective Action
* Status
* Follow-Up Required

The update is applied to the existing issue record rather than creating another row.

This preserves the original issue history and keeps the issue record centralized.

---

## Decision Point — Is Follow-Up Required?

### Step 5 — Filter

**App:** Filter by Zapier

**Condition:**

```text
Follow-up Required = Yes
```

The filter determines whether the resolved issue still requires additional operational follow-up.

### If No

The workflow ends after the issue record has been updated.

### If Yes

The workflow continues to the notification step.

This prevents unnecessary follow-up emails for issues that have been fully resolved.

---

### Step 6 — Follow-Up Alert

**App:** Gmail

**Action:** Send Email

If the resolution submission indicates that follow-up is required, Zapier sends a notification to the appropriate operations recipient.

The notification provides visibility that the issue requires additional action even though a resolution has already been submitted.

---

### Step 7 — Record Follow-Up Alert

**App:** Google Sheets

**Action:** Update Spreadsheet Row

After the follow-up notification is sent, the corresponding issue record is updated:

```text
Follow_Up_Alert_Sent = Yes
```

This creates an audit trail showing that the follow-up notification was sent.

---

# Data Flow

```text
Issue Resolution Form
          │
          ▼
Form Response Sheet
          │
          ▼
      Zapier
          │
          ▼
Lookup Issue by Issue_ID
          │
          ▼
Format Resolution Date
          │
          ▼
Update Existing Issue
          │
          ▼
   Follow-Up Required?
       │          │
      No         Yes
       │          │
       │          ▼
       │        Gmail
       │     Follow-Up Alert
       │          │
       │          ▼
       │     Update Issue
       │     Alert Sent = Yes
       │
       ▼
      END
```

---

# Key Automation Logic

### Record Matching

The workflow uses the submitted **Issue ID** to find the correct existing issue.

This is important because the automation is not simply adding another record—it is updating the operational record associated with the original compliance issue.

### Conditional Follow-Up

The workflow uses:

```text
Follow-Up Required = Yes
```

as a decision point.

Only issues requiring additional action generate a follow-up notification.

### Notification Tracking

After the follow-up email is sent:

```text
Follow_Up_Alert_Sent = Yes
```

is recorded in the issue record.

This provides visibility into the notification status.

---

# Example Scenario

An UrbanBite franchise has an open compliance issue:

```text
Issue ID: ISS-FR-003-20260923-141535
Status: Open
```

The franchise submits an issue-resolution form.

The form indicates:

```text
Resolution Status: Resolved
Follow-Up Required: Yes
```

Zap 2 automatically:

1. Receives the issue-resolution submission.
2. Looks up the matching Issue ID.
3. Formats the resolution timestamp.
4. Updates the existing issue record.
5. Detects that follow-up is required.
6. Sends a follow-up alert.
7. Records `Follow_Up_Alert_Sent = Yes`.

The operations team therefore does not need to manually search the Issues sheet or remember to send the follow-up notification.

---

# Business Outcome

Zap 2 creates a controlled handoff between issue resolution and operational follow-up.

### Operational benefits

* Faster issue record updates
* Accurate matching using Issue ID
* Centralized resolution history
* Automatic follow-up routing
* Reduced manual spreadsheet searching
* Consistent resolution timestamps
* Notification tracking
* Better visibility into unresolved follow-up actions

---

# Tools Used

| Tool                    | Role                                     |
| ----------------------- | ---------------------------------------- |
| **Google Forms**        | Collects issue-resolution submissions    |
| **Google Sheets**       | Stores and updates issue records         |
| **Zapier**              | Orchestrates the resolution workflow     |
| **Formatter by Zapier** | Standardizes the resolution timestamp    |
| **Filter by Zapier**    | Determines whether follow-up is required |
| **Gmail**               | Sends follow-up notifications            |

---

# Automation Design Pattern

**Event → Identify Record → Process → Update → Evaluate → Notify → Record Notification**

This Zap demonstrates a record-update automation pattern using lookup logic, conditional routing, operational notifications, and audit tracking.
