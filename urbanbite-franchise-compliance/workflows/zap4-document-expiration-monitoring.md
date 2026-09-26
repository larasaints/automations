# Zap 4 — Document Expiration Monitoring

## Overview

**Purpose:** Automatically monitor franchise compliance documents and identify documents that are expired or approaching expiration, then send a consolidated operational alert.

**Automation:** UB - Document Expiration Monitoring

**Platform:** Zapier

**Primary Tools:** Schedule by Zapier, Google Sheets, Formatter by Zapier, Gmail

**Trigger:** Scheduled daily monitoring at 8:00 AM

---

## Business Problem

Franchise locations may be required to maintain permits, certificates, licenses, and other compliance documents.

When expiration dates are monitored manually, operations teams may overlook documents that are:

* Already expired
* Approaching expiration
* Still awaiting renewal
* Requiring immediate operational attention

Manual monitoring also requires repeatedly reviewing the Documents worksheet and determining which records need action.

Zap 4 automates this monitoring process by using the document expiration date and queue logic already maintained in Google Sheets.

---

# Workflow

```text
Schedule by Zapier
Daily — 8:00 AM
        ↓
Google Sheets
Documents Worksheet
        ↓
Find Rows
Expiration Queue = READY
        ↓
Identify Status
EXPIRING / EXPIRED
        ↓
Formatter
Prepare Document Details
        ↓
Gmail
Consolidated Document Expiration Alert
```

---

# Step-by-Step Workflow

## Step 1 — Scheduled Trigger

**App:** Schedule by Zapier

**Trigger:** Every Day

**Schedule:** 8:00 AM

**Time Zone:** Asia/Manila

Zap 4 runs automatically every morning to check the current status of franchise compliance documents.

No manual monitoring is required to start the process.

---

## Step 2 — Find Documents Requiring Attention

**App:** Google Sheets

**Action:** Lookup Spreadsheet Rows (Advanced)

**Worksheet:** `Documents`

**Lookup Column:**

```text id="txk9bv"
Expiration_Queue
```

**Lookup Value:**

```text id="f37kpi"
READY
```

**Multiple Search Results:** Return all results as line items

The automation searches for every document currently marked `READY`.

This allows the workflow to process multiple expiring or expired documents in one scheduled run.

---

# Document Queue Logic

The `Expiration_Queue` column is calculated in Google Sheets.

The queue identifies documents that require attention based on their expiration date and renewal status.

The logic considers:

* Whether the document is already expired
* Whether the document expires within the next 30 days
* Whether it has already been renewed
* Whether an alert has already been sent

Documents that do not require attention remain outside the automation queue.

---

## Step 3 — Determine Expiration Type

The `Expiration_Type` field identifies why the document is in the queue.

Possible values are:

```text id="0xwq0k"
EXPIRING
```

or

```text id="4x98cy"
EXPIRED
```

This allows the final notification to distinguish between documents approaching expiration and documents that have already expired.

---

# Step 4 — Prepare Document Information

Zap 4 uses Formatter by Zapier to convert the returned Google Sheets line items into report-ready values.

The workflow prepares the following fields:

### Formatter 1 — Document ID

**Source:** Documents Column A

```text id="r4j3ul"
Document_ID
```

### Formatter 2 — Franchise ID

**Source:** Documents Column B

```text id="j0a6yf"
Franchise_ID
```

### Formatter 3 — Location

**Source:** Documents Column C

```text id="q3o3c1"
Location
```

### Formatter 4 — Document Type

**Source:** Documents Column D

```text id="1pqxjm"
Document_Type
```

### Formatter 5 — Expiration Date

**Source:** Documents Column G

```text id="8j0xj5"
Expiration_Date
```

### Formatter 6 — Renewal Status

**Source:** Documents Column J

```text id="pl9w6p"
Renewal_Status
```

### Formatter 7 — Expiration Type

**Source:** Documents Column L

```text id="5c5d6b"
Expiration_Type
```

These values are then used to build the consolidated notification.

---

# Step 5 — Consolidated Document Expiration Alert

**App:** Gmail

**Action:** Send Email

The automation sends one consolidated document expiration report containing all documents returned by the queue lookup.

The notification includes:

* Status
* Document ID
* Franchise ID
* Location
* Document Type
* Expiration Date
* Renewal Status

The email allows the operations team to review expiring and expired documents from one centralized report.

---

# Example Alert

A daily monitoring run may identify:

| Status   | Document ID | Franchise | Location    | Document Type   | Expiration Date | Renewal Status |
| -------- | ----------- | --------- | ----------- | --------------- | --------------- | -------------- |
| EXPIRING | DOC-001     | FR-001    | Makati      | Business Permit | 09/30/2026      | Pending        |
| EXPIRED  | DOC-002     | FR-002    | Imus        | Sanitary Permit | 09/20/2026      | Pending        |
| EXPIRING | DOC-005     | FR-005    | Quezon City | Sanitary Permit | 10/05/2026      | Pending        |

The actual number of documents varies depending on the current expiration queue.

---

# Data Flow

```text
Schedule by Zapier
Daily — 8:00 AM
        │
        ▼
Documents Worksheet
        │
        ▼
Expiration Queue = READY
        │
        ▼
Multiple Matching Documents
        │
        ▼
Formatter
Prepare Document Details
        │
        ▼
Gmail
Document Expiration Alert
```

---

# Key Automation Logic

## Expiration Monitoring

The Google Sheets helper logic evaluates document expiration dates.

Documents can enter the queue when:

```text id="ujj5b3"
Expiration Date < Today
```

or:

```text id="i9r1ps"
Expiration Date ≤ Today + 30 days
```

The queue also prevents unnecessary processing when a document has already been renewed or an alert has already been sent.

---

## Expiration Classification

Once a document qualifies for monitoring, the system classifies it as:

```text id="r70ym6"
EXPIRING
```

or:

```text id="70n8rq"
EXPIRED
```

This classification is displayed directly in the operational alert.

---

# Business Outcome

Zap 4 transforms document expiration monitoring into a recurring automated compliance process.

### Operational benefits

* Daily expiration monitoring
* Automatic identification of expired documents
* Early visibility into upcoming expirations
* Consolidated notification instead of individual alerts
* Renewal status visibility
* Reduced manual spreadsheet review
* Centralized document compliance monitoring

---

# Tools Used

| Tool                    | Role                                                 |
| ----------------------- | ---------------------------------------------------- |
| **Schedule by Zapier**  | Starts the daily document monitoring cycle           |
| **Google Sheets**       | Stores documents and calculates the expiration queue |
| **Formatter by Zapier** | Prepares document information for the report         |
| **Gmail**               | Sends the consolidated expiration alert              |

---

# Automation Design Pattern

**Schedule → Evaluate Expiration Queue → Classify → Consolidate → Notify**

This Zap demonstrates scheduled compliance monitoring, rule-based queue management, multi-record processing, and consolidated operational notifications.
