# Zap 2 — Issue Resolution & Corrective Action Tracking

## Overview

Zap 2 automates the resolution and corrective-action tracking process for UrbanBite franchise compliance issues.

When an existing compliance issue is resolved or updated, HQ should not have to manually search through the Issues sheet and update multiple fields. This workflow finds the matching issue, updates its resolution information, checks whether follow-up is required, sends an alert when necessary, and records that the alert was sent.

---

## Business Problem

After Zap 1 identifies a compliance issue, the issue still needs to be resolved and tracked.

Without automation, HQ would need to manually:

* Find the matching issue in the Issues sheet
* Update the issue status
* Enter the resolution date
* Add resolution notes
* Record corrective action
* Record whether follow-up is required
* Send a follow-up notification when necessary
* Record that the notification was sent

This creates additional administrative work and can make issue tracking less consistent.

---

## Business Solution

Zap 2 connects the resolution process directly to the existing issue record.

A resolution/update is submitted through the connected Google Sheets workflow. Zapier finds the matching issue, generates a formatted resolution date, updates the existing issue, evaluates whether follow-up is required, and sends an HQ notification when follow-up is needed.

The workflow then records that the follow-up alert was sent, providing an audit trail.

---

## Workflow

```text
Google Sheets
New or Updated Spreadsheet Row
          ↓
Google Sheets
Lookup Spreadsheet Row
          ↓
Formatter by Zapier
Date/Time
          ↓
Google Sheets
Update Spreadsheet Row
          ↓
Filter by Zapier
Filter Conditions
          ↓
Gmail
Send Email
          ↓
Google Sheets
Update Spreadsheet Row
```

---

## Detailed Automation Logic

### Step 1 — Detect Resolution Update

**App:** Google Sheets
**Action:** New or Updated Spreadsheet Row

The workflow detects a new or updated row containing issue-resolution information.

This starts the resolution-tracking automation.

```text
Resolution Update
       ↓
Google Sheets
       ↓
Zap 2 Trigger
```

---

### Step 2 — Find the Matching Issue

**App:** Google Sheets
**Action:** Lookup Spreadsheet Row

Zapier searches the UrbanBite `Issues` sheet for the matching issue.

The lookup allows the workflow to update the existing issue instead of creating a duplicate record.

The row returned by the lookup becomes the target for the update steps.

```text
Resolution Information
        ↓
Lookup Issue
        ↓
Existing Issue Row
```

---

### Step 3 — Generate Resolution Date

**App:** Formatter by Zapier
**Action:** Date / Time

The workflow formats the spreadsheet timestamp into a consistent resolution date.

**Input:**

Timestamp from the spreadsheet

**Format:**

```text
MM/DD/YYYY HH:mm:ss
```

This creates a standardized value that can be saved to the issue record.

---

### Step 4 — Update the Existing Issue

**App:** Google Sheets
**Action:** Update Spreadsheet Row

The matching issue row identified by the Lookup step is updated.

The resolution information can include:

* Status
* Resolution Date
* Resolution Notes
* Corrective Action
* Follow-Up Required

The row being updated is mapped from the matching issue returned by the Lookup step.

```text
Lookup Matching Issue
        ↓
Update Existing Issue
        ↓
Resolution Information Saved
```

---

## Issue Tracking Fields

The `Issues` sheet includes fields that support resolution and follow-up tracking.

Relevant fields include:

```text
Status
Resolution_Date
Resolution_Notes
Corrective_Action
Follow_Up_Required
Follow_Up_Alert_Sent
```

These fields extend the original issue record so UrbanBite can track the issue through resolution and follow-up.

---

## Step 5 — Check Whether Follow-Up Is Required

**App:** Filter by Zapier
**Action:** Filter Conditions

The workflow checks the `Follow-Up Required` value.

**Condition:**

```text
Follow-Up Required = Yes
```

### If NO

The workflow ends because no additional follow-up notification is required.

### If YES

The workflow continues to Gmail and sends an HQ follow-up alert.

```text
Follow-Up Required?
        │
   ┌────┴────┐
  NO         YES
   ↓           ↓
 END       Gmail Alert
```

---

## Step 6 — Send Follow-Up Alert

**App:** Gmail
**Action:** Send Email

When follow-up is required, Zapier automatically sends an email notification to HQ.

The email uses mapped issue information so HQ can identify the relevant issue and review the required follow-up.

The notification prevents HQ from having to manually monitor the Issues sheet for follow-up requirements.

---

## Step 7 — Record Follow-Up Alert

**App:** Google Sheets
**Action:** Update Spreadsheet Row

After the follow-up email is sent, the existing issue row is updated to record that the alert was sent.

```text
Follow_Up_Alert_Sent = Yes
```

This creates an audit trail for the automated notification.

Instead of:

```text
Email sent
    ↓
No record
```

UrbanBite records:

```text
Email sent
    ↓
Follow_Up_Alert_Sent = Yes
    ↓
Issue audit trail
```

---

# Complete Zap 2 Flow

```text
                 ZAP 2
Issue Resolution & Corrective Action Tracking

                        ↓

        New or Updated Spreadsheet Row
                        ↓
              Lookup Spreadsheet Row
                        ↓
                 Formatter Date/Time
                        ↓
             Update Spreadsheet Row
                        ↓
                Follow-Up Required?
                   │            │
                  NO           YES
                   │            │
                   ↓            ↓
                  END      Gmail Send Email
                                ↓
                       Update Spreadsheet Row
                                ↓
                    Follow_Up_Alert_Sent = Yes
```

---

## Tools Used

* Google Sheets
* Formatter by Zapier
* Filter by Zapier
* Gmail

---

## Test Scenario

### Test — Follow-Up Required
```text
Follow-Up Required = Yes
```
<img width="738" height="584" alt="zap2email" src="https://github.com/user-attachments/assets/f07c6816-b9f2-4c2d-9e97-b4ecb5ffc884" />
### Expected Result

1. Google Sheets detects the updated row.
2. Zapier looks up the matching issue.
3. The resolution date is formatted.
4. The existing issue is updated.
5. Follow-up requirement is evaluated.
6. Gmail sends the follow-up alert.
7. The issue record is updated with:

```text
Follow_Up_Alert_Sent = Yes
```
<img width="281" height="194" alt="image" src="https://github.com/user-attachments/assets/237923d0-7f2e-458a-9c81-808a634de454" />
---

## Business Outcome

Zap 2 closes the resolution loop created by Zap 1.

The process now moves from:

```text
Issue Identified
      ↓
Issue Tracked
      ↓
Resolution Submitted
      ↓
Issue Updated
      ↓
Follow-Up Evaluated
      ↓
HQ Notified When Required
      ↓
Notification Recorded
```

This reduces manual spreadsheet maintenance and creates a more traceable corrective-action process.

---

## Relationship to Zap 1

Zap 1 and Zap 2 work together as part of the UrbanBite compliance automation system.

```text
ZAP 1
Compliance Submission
        ↓
Issue Detection
        ↓
Issue Creation
        ↓
HQ Notification
        ↓
Urgent Escalation
        ↓
Existing Issue
        ↓
ZAP 2
Resolution Update
        ↓
Issue Lookup
        ↓
Issue Updated
        ↓
Follow-Up Check
        ↓
HQ Follow-Up Alert
        ↓
Alert Recorded
```

Together, the workflows provide an operational audit trail from compliance inspection through corrective action and follow-up.
