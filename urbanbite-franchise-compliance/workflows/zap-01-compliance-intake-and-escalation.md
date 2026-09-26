# Zap 1 — New Franchise Compliance Submission & Escalation

**Compliance Submission → Issue Detection → HQ Notification → Urgent Escalation**

## Overview

This workflow automates the processing of monthly franchise compliance inspections for the UrbanBite Franchise Network.

When a franchise manager submits the monthly compliance inspection, the workflow records the submission, creates a compliance report, detects reported issues, creates an issue record, notifies HQ, and escalates urgent issues with a tracked due date.

---

## Business Problem

UrbanBite manages 100+ franchise locations across multiple regions.

HQ needs to:

* Receive monthly compliance submissions
* Maintain a centralized compliance record
* Identify failed or reported requirements
* Track issues requiring corrective action
* Notify HQ when issues are identified
* Escalate urgent issues immediately
* Maintain an audit trail for escalation and deadlines

Without automation, these activities could require repetitive manual review and follow-up.

---

## Workflow Architecture

```text
Franchise Manager
       ↓
Google Compliance Form
       ↓
New Spreadsheet Row
       ↓
Format Submission Timestamp
       ↓
Create Compliance Report
       ↓
Issue Identified?
    ┌──┴──┐
   NO     YES
   ↓       ↓
  END   Format Issue Timestamp
           ↓
       Create Issue
           ↓
       Email HQ
           ↓
      Priority = URGENT?
        ┌───┴───┐
       NO       YES
       ↓         ↓
   Normal     Urgent
   workflow   escalation
                 ↓
            Urgent Email
                 ↓
            Update Issue
                 ↓
        Escalation Status
           = Escalated
                 ↓
          Due Date = +1 day
```

---

## Trigger

**Google Sheets — New Spreadsheet Row**

The Google Compliance Form is connected to a Google Sheets response sheet.

When a franchise manager submits an inspection, a new spreadsheet row is created and triggers the Zap.

---

## Automation Steps

### 1. New Spreadsheet Row

**App:** Google Sheets
**Event:** New Spreadsheet Row

Captures the newly submitted monthly compliance inspection.

---

### 2. Format Submission Timestamp

**App:** Formatter by Zapier
**Action:** Date/Time

Processes the inspection timestamp into the format required for the compliance report.

---

### 3. Create Compliance Report

**App:** Google Sheets
**Action:** Create Spreadsheet Row

Creates a structured record in the `Compliance_Reports` sheet.

The record connects the inspection submission to the UrbanBite compliance database.

---

### 4. Detect Reported Issue

**App:** Filter by Zapier
**Condition:**

`Did you identify any compliance issues? = Yes`

If no issue was identified, the workflow stops at this branch.

If an issue was identified, the workflow continues to issue creation.

---

### 5. Format Issue Timestamp

**App:** Formatter by Zapier
**Action:** Date/Time

Generates/processes the timestamp used for the issue record.

---

### 6. Create Issue

**App:** Google Sheets
**Action:** Create Spreadsheet Row

Creates a new record in the `Issues` sheet.

The issue is connected to the originating compliance submission so the problem can be tracked through the corrective-action process.

---

### 7. Send Normal HQ Alert

**App:** Gmail
**Action:** Send Email

HQ receives an automated notification when a compliance issue has been identified.

This creates immediate visibility without requiring HQ to manually monitor the compliance sheet.

---

### 8. Check Issue Priority

**App:** Filter by Zapier
**Condition:**

`Priority = Urgent`

The workflow separates urgent issues from normal/high-priority issues.

Non-urgent issues remain in the normal issue-tracking process.

Urgent issues continue to the escalation path.

---

### 9. Send Urgent Escalation

**App:** Gmail
**Action:** Send Email

A separate urgent escalation notification is sent to HQ when the issue priority is `Urgent`.

This creates a distinct notification path for higher-priority operational issues.

---

### 10. Update Issue — Escalation Tracking

**App:** Google Sheets
**Action:** Update Spreadsheet Row

The specific issue record is updated with escalation information.

**Escalation_Status:**

`Escalated`

**Escalation_Date:**

Timestamp of the escalation.

---

### 11. Calculate Urgent Due Date

**App:** Formatter by Zapier
**Action:** Date/Time

For the current UrbanBite business rule, urgent issues receive a target due date of:

**+1 day**

This establishes a corrective-action deadline for urgent issues.

---

## Current Business Rules

| Priority | Target Resolution |
| -------- | ----------------: |
| Low      |            7 days |
| Medium   |            5 days |
| High     |            3 days |
| Urgent   |             1 day |

**Current Zap implementation:** the automated due-date calculation has been implemented for the **Urgent** path using a +1 day rule.

The other priority targets are part of the defined UrbanBite business rules and can be automated in a later enhancement.

---

## Data Flow

```text
Compliance Form
      ↓
Form Response Sheet
      ↓
Compliance_Reports
      ↓
Issues
      ↓
HQ Notification
      ↓
Urgent Escalation
      ↓
Issue Update
      ↓
Resolution Tracking
```

Each issue can therefore be traced through the process:

**Compliance Report → Issue → Resolution**

---

## Test Cases

### Test 1 — Fully Compliant

**Franchise ID:** FR-001
**Location:** Makati
**Manager:** Anna Cruz

* Safety: Pass
* Inventory: Pass
* Equipment: Pass
* Documents: Complete
* Photos: Submitted
* Issues: No
* Overall Status: Compliant

**Expected behavior:**

The compliance report is created, but no issue is created and no escalation occurs.

---

### Test 2 — High Priority Issue

**Franchise ID:** FR-002
**Location:** Imus
**Manager:** Carlo Reyes

* Equipment: Fail
* Documents: Expired
* Issues: Yes
* Category: Equipment
* Description: Freezer is not maintaining the required temperature.
* Priority: High
* Overall Status: Needs Attention

**Observed behavior:**

* Compliance report created
* Issue created
* Normal HQ issue alert sent
* Urgent escalation not triggered

---

### Test 3 — Urgent Issue

**Franchise ID:** FR-003

**Priority:** Urgent

**Observed behavior:**

* Compliance report created
* Issue created
* Normal HQ issue alert sent
* Urgent escalation email sent
* Issue marked `Escalated`
* Escalation timestamp recorded
* Due date calculated as +1 day

---

## Tools Used

| Tool                | Purpose                              |
| ------------------- | ------------------------------------ |
| Google Forms        | Franchise compliance data collection |
| Google Sheets       | Compliance and issue database        |
| Zapier              | Workflow automation                  |
| Formatter by Zapier | Date/time processing                 |
| Filter by Zapier    | Conditional business logic           |
| Gmail               | HQ notifications and escalation      |

---

## Automation Concepts Demonstrated

* Event-driven automation
* Multi-step workflow design
* Data transformation
* Conditional logic
* Exception detection
* Issue creation
* Priority-based routing
* Automated notifications
* Escalation workflows
* Due-date calculation
* Operational audit trails
* Cross-record traceability

---

## Business Outcome

The workflow turns a franchise compliance submission into a structured operational process:

**Submit → Record → Detect → Create Issue → Notify → Escalate → Track**

Instead of relying on manual monitoring, HQ receives automated visibility when compliance issues are reported and receives a separate escalation when an issue is classified as urgent.

---

## Next Workflow

**[WORKFLOW ZAP 2 — Issue Resolution and Corrective Action**](https://github.com/larasaints/automations/blob/c51444d2753b3c1e27e7c872d88b67fa26043ed7/urbanbite-franchise-compliance/workflows/zap-02-issue-resolution-and-corrective-action.md)

The next workflow handles the corrective-action side of the process:

**Resolution Form → Find Issue → Update Issue → Record Resolution Date → Record Resolution Notes**
