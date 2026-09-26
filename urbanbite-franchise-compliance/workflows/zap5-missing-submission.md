# Zap 5 — Missing Monthly Compliance Submission

## Overview

**Purpose:** Automatically identify active franchise locations that have not submitted their monthly compliance report, send a consolidated reminder, and record the month for which an alert was already sent.

**Automation:** UB - Missing Monthly Compliance Submission

**Platform:** Zapier

**Primary Tools:** Schedule by Zapier, Google Sheets, Formatter by Zapier, Gmail

**Trigger:** Scheduled monthly monitoring on the 1st day of each month at 8:00 AM

---

## Business Problem

Monthly compliance submissions require recurring monitoring across all active franchise locations.

Without automation, operations teams would need to manually:

* Review every active franchise.
* Determine which locations submitted their report.
* Compare submission records against the previous month.
* Identify missing submissions.
* Send reminders.
* Track which locations have already received an alert.
* Repeat the process every month.

Zap 5 automates this recurring process and includes logic to prevent duplicate alerts for the same monitoring month.

---

# Workflow

```text
Schedule by Zapier
Monthly — 1st Day — 8:00 AM
        ↓
Google Sheets
Franchisees Worksheet
        ↓
Find Rows
Monthly Compliance Queue = READY
        ↓
Formatter
Prepare Franchise Details
        ↓
Gmail
Consolidated Missing Submission Alert
        ↓
Google Sheets
Update Last Missing Alert Month
```

---

# Step-by-Step Workflow

## Step 1 — Scheduled Trigger

**App:** Schedule by Zapier

**Trigger:** Every Month

**Schedule:** 1st day of the month

**Time:** 8:00 AM

**Time Zone:** Asia/Manila

The automation runs automatically at the beginning of each month.

The workflow checks the previous month's compliance submission status.

---

# Step 2 — Retrieve Franchise Records

**App:** Google Sheets

**Action:** Get Many Spreadsheet Rows (Advanced)

**Worksheet:** `Franchisees`

This step retrieves the franchise records that will be evaluated by the monitoring workflow.

The worksheet contains information such as:

* Franchise ID
* Franchise Name
* Location
* Region
* Franchise Manager
* Manager Email
* Status

The monitoring fields used by Zap 5 are also maintained in this worksheet.

---

# Step 3 — Find Franchisees Ready for Missing-Submission Alert

**App:** Google Sheets

**Action:** Lookup Spreadsheet Rows (Advanced)

**Worksheet:** `Franchisees`

**Lookup Column:**

```text id="02q6v5"
Monthly_Compliance_Queue
```

**Lookup Value:**

```text id="umg9w7"
READY
```

**Multiple Search Results:** Return all results as line items

The lookup returns all active franchise locations that are missing the required monthly compliance submission and have not already received an alert for the current monitoring month.

---

# Monthly Monitoring Logic

Zap 5 uses helper columns in the `Franchisees` worksheet.

### Monitoring Month

The system determines the month being monitored.

Example:

```text id="q6c7y7"
2026-08
```

This represents the previous month when the monitoring cycle runs in September.

---

### Submission Status

The system checks the `Compliance_Reports` worksheet to determine whether each franchise submitted a compliance report during the monitoring period.

Possible result:

```text id="ijygl2"
Submitted
```

or:

```text id="fclzxu"
Missing
```

---

### Last Missing Alert Month

The system records the month for which a missing-submission alert was already sent.

Example:

```text id="g2ob0b"
2026-08
```

---

### Monthly Compliance Queue

The final queue determines whether the franchise should be included in the current alert.

A franchise is placed in the queue when:

```text id="x9zv0n"
Active
+
Missing
+
No alert has been sent for the current monitoring month
```

The resulting queue value is:

```text id="a8qk4v"
READY
```

---

# Step 4 — Prepare Franchise Information

Zap 5 uses Formatter by Zapier to convert the returned line items into report-ready text.

The workflow prepares the following fields:

### Formatter 1 — Franchise ID

**Source:** Franchisees Column A

```text id="h7h2oe"
Franchise_ID
```

### Formatter 2 — Franchise Name

**Source:** Franchisees Column B

```text id="55gh11"
Franchise_Name
```

### Formatter 3 — Location

**Source:** Franchisees Column C

```text id="9x5m76"
Location
```

### Formatter 4 — Manager

**Source:** Franchisees Column E

```text id="7tgyiu"
Franchise_Manager
```

### Formatter 5 — Monitoring Month

**Source:** Franchisees Column H

```text id="s20q93"
Monitoring_Month
```

### Formatter 6 — Submission Status

**Source:** Franchisees Column I

```text id="rqv4k5"
Monthly_Submission_Status
```

These values are used to build the consolidated missing-submission report.

---

# Step 5 — Consolidated Missing Submission Alert

**App:** Gmail

**Action:** Send Email

The automation sends one consolidated email containing the active franchise locations that have not submitted their compliance report for the monitored month.

The report includes information such as:

* Franchise ID
* Franchise Name
* Location
* Manager
* Monitoring Month
* Submission Status

Instead of sending separate emails for every franchise location, the operations team receives one centralized monitoring report.

---

# Step 6 — Record Alert Month

**App:** Google Sheets

**Action:** Update Spreadsheet Row(s)

The workflow updates the matching franchise records using the row numbers returned by the lookup.

The primary field updated is:

```text id="p7r6iz"
Last_Missing_Alert_Month
```

The value comes from:

```text id="0hm4c6"
Monitoring_Month
```

For example:

```text id="pyy6hm"
Last_Missing_Alert_Month = 2026-08
```

This records that the August missing-submission alert has already been sent.

---

# Duplicate Alert Prevention

One of the key features of Zap 5 is its duplicate-prevention logic.

After an alert is sent:

```text id="ykj15e"
Monitoring_Month = 2026-08
```

and:

```text id="t1d4tq"
Last_Missing_Alert_Month = 2026-08
```

The queue then evaluates to blank rather than `READY`.

This prevents the same franchise from repeatedly appearing in the missing-submission alert for the same month.

---

# New Month Logic

When the monitoring month changes, the system can automatically identify a new missing submission.

For example:

```text id="8h2vlf"
Previous cycle:

Monitoring Month = 2026-08
Last Alert Month = 2026-08
Queue = blank
```

When the next monitoring cycle begins:

```text id="l5x3cb"
New cycle:

Monitoring Month = 2026-09
Last Alert Month = 2026-08
Queue = READY
```

If the September submission is missing, the franchise becomes eligible for a new alert.

This allows the same automation to continue operating month after month without manually resetting the queue.

---

# Data Flow

```text
Schedule by Zapier
Monthly — 1st Day — 8:00 AM
        │
        ▼
Franchisees Worksheet
        │
        ▼
Monthly Compliance Queue = READY
        │
        ▼
Multiple Matching Franchisees
        │
        ▼
Formatter
Prepare Franchise Details
        │
        ▼
Gmail
Consolidated Missing Submission Alert
        │
        ▼
Google Sheets
Update Last Missing Alert Month
```

---

# Example Scenario

At the beginning of September, the system checks August compliance submissions.

Suppose several active franchise locations have:

```text
Monitoring Month = 2026-08
Monthly Submission Status = Missing
Last Missing Alert Month ≠ 2026-08
```

Their queue becomes:

```text
Monthly Compliance Queue = READY
```

Zap 5 then:

1. Retrieves the qualifying franchise records.
2. Prepares the franchise details.
3. Creates one consolidated missing-submission report.
4. Sends the report through Gmail.
5. Updates each matching franchise record.
6. Records `2026-08` as the last alert month.

If the same Zap runs again while the monitoring month is still August, those franchisees are no longer returned by the `READY` queue.

---

# Key Automation Logic

## Active Franchise Check

Only active franchise locations are eligible for monitoring.

```text id="y8w69p"
Status = Active
```

---

## Missing Submission Check

The system identifies whether the franchise has submitted a compliance report during the monitored month.

```text id="z1l2hc"
Monthly Compliance Status = Missing
```

---

## Duplicate Prevention

The system compares:

```text id="j11ckh"
Monitoring Month
```

against:

```text id="8m5w4h"
Last Missing Alert Month
```

If they match, another alert is not queued.

---

## Consolidated Reporting

Multiple missing franchise submissions are grouped into one email:

```text id="z1b8yq"
Franchise 1 ─┐
Franchise 2 ─┤
Franchise 3 ─┼──► One Gmail Monitoring Report
Franchise 4 ─┘
```

---

# Business Outcome

Zap 5 converts a recurring monthly compliance review into an automated monitoring process.

### Operational benefits

* Automatic monthly monitoring
* No manual franchise-by-franchise review
* Identification of missing compliance submissions
* Consolidated operational notifications
* Duplicate-alert prevention
* Month-to-month monitoring continuity
* Dynamic multi-record updates
* Clear audit trail of missing-submission alerts

---

# Tools Used

| Tool                                        | Role                                                      |
| ------------------------------------------- | --------------------------------------------------------- |
| **Schedule by Zapier**                      | Starts the monthly monitoring cycle                       |
| **Google Sheets**                           | Stores franchise records and submission-monitoring fields |
| **Formatter by Zapier**                     | Prepares franchise information for the report             |
| **Gmail**                                   | Sends the consolidated missing-submission alert           |
| **Google Sheets Update Spreadsheet Row(s)** | Records the month an alert was sent                       |

---

# Automation Design Pattern

**Schedule → Evaluate Submission Status → Check Alert History → Queue → Consolidate → Notify → Record Alert Month**

This Zap demonstrates scheduled recurring monitoring, spreadsheet-based business logic, duplicate prevention, multi-record processing, consolidated notifications, and state tracking across monthly cycles.
