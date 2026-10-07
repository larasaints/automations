# AI Real Estate Lead Qualification System

An AI-powered real estate lead qualification system built with **GoHighLevel and Conversation AI**.

The system automatically starts conversations with new leads, collects qualification information, extracts structured data from the conversation, calculates qualification outcomes, routes leads through a CRM pipeline, identifies leads requiring human intervention, and provides dashboard visibility for the sales team.

---

## Project Overview

Real estate teams often receive leads without enough information to determine whether they are ready to buy or sell.

Manually qualifying every lead can result in:

* Slow response times
* Inconsistent qualification
* Missed high-intent opportunities
* Poor CRM data quality
* Unnecessary manual follow-up
* Limited visibility into AI errors and handoffs

This project solves that problem by creating an automated AI qualification workflow.

### High-Level Architecture

```text
New Lead
   │
   ▼
Contact Created
   │
   ▼
Conversation AI
   │
   ├───────────────► Human Handoff
   │
   ├───────────────► Time Out / Error
   │
   ▼
AI Extract Data
   │
   ▼
Update Contact Fields
   │
   ▼
AI-Qualification-Complete
   │
   ▼
Workflow 2
   │
   ├── Hot Lead
   │      └── Human Follow-Up
   │
   ├── Qualified
   │      └── Sales Notification
   │
   ├── Nurture
   │      └── Long-Term Follow-Up
   │
   └── Unqualified
          └── Archive / No Opportunity
   │
   ▼
GHL Dashboard
```

---

## Objectives

The system is designed to:

1. Start an AI conversation automatically when a new contact is created.
2. Ask qualification questions naturally.
3. Avoid unnecessarily repeating information already provided.
4. Extract structured lead information from the conversation.
5. Store the information in CRM custom fields.
6. Calculate an AI qualification score.
7. Assign a qualification status.
8. Route the lead to the appropriate pipeline stage.
9. Identify leads requiring human intervention.
10. Create opportunities for qualified leads.
11. Prevent duplicate opportunities.
12. Notify the sales team about high-priority leads.
13. Track AI errors and handoffs.
14. Provide dashboard-level operational visibility.

---

# Tech Stack

| Technology        | Purpose                                                    |
| ----------------- | ---------------------------------------------------------- |
| GoHighLevel       | CRM, conversations, workflows, opportunities and dashboard |
| Conversation AI   | AI lead qualification                                      |
| AI Extract Data   | Structured data extraction from conversation transcripts   |
| GHL Workflows     | Automation and business logic                              |
| GHL Opportunities | Lead pipeline management                                   |
| GHL Custom Fields | Structured qualification data                              |
| GHL Dashboard     | Reporting and monitoring                                   |
| JSON              | Configuration and workflow documentation                   |
| Markdown          | Technical documentation                                    |

---

# Core Features

## 1. Automated Lead Qualification

A new contact automatically enters the AI qualification workflow.

```text
Contact Created
      ↓
Conversation AI
```

The AI begins with:

> Hi, {{contact.name}}! Thanks for reaching out. Are you looking to buy or sell a property?

The AI then continues the qualification conversation naturally.

---

## 2. Qualification Data Collection

The system collects:

* Lead Type
* Property Type
* Target Location
* Budget
* Financing
* Purchase Timeline
* Pre-Approved Status
* Motivation

The AI should ask one question at a time and avoid asking for information that the lead has already provided.

---

## 3. AI Data Extraction

After the conversation is complete, the transcript is passed to the AI extraction layer.

The system extracts:

```json
{
  "lead_type": "Buyer",
  "property_type": "Condo",
  "target_location": "Makati",
  "budget": "5000000",
  "financing": "Mortgage",
  "purchase_timeline": "0–3 Months",
  "pre_approved": "Yes",
  "motivation": "Relocating for work",
  "qualification_status": "Hot Lead",
  "ai_handoff_required": "Yes"
}
```

---

## 4. Qualification Scoring

Leads are categorized using an AI qualification score.

|  Score | Status      | Action                    |
| -----: | ----------- | ------------------------- |
| 90–100 | Hot Lead    | Immediate human follow-up |
|  70–89 | Qualified   | Sales notification        |
|  40–69 | Nurture     | Long-term follow-up       |
|   0–39 | Unqualified | No opportunity            |

---

## 5. Pipeline Routing

Pipeline:

**Real Estate Lead Qualification**

Stages:

1. New Lead
2. Hot Lead
3. Qualified
4. Nurture
5. Unqualified

### Hot Lead

A Hot Lead:

* Receives `AI-Qualified`
* Sets `AI Handoff Required = Yes`
* Sets `AI Handoff Status = Pending`
* Creates an opportunity in **Hot Lead**
* Requires human follow-up
* Sends an urgent internal notification

### Qualified

A Qualified lead:

* Receives Buyer or Seller tagging
* Creates an opportunity in **Qualified**
* Sends an internal notification

### Nurture

A Nurture lead:

* Receives `AI-Nurture`
* Creates an opportunity in **Nurture**
* Does not generate an immediate notification

### Unqualified

An Unqualified lead:

* Receives `AI-Unqualified`
* Does not create an opportunity
* Does not generate a notification

---

# 6. Human Handoff

The AI stops qualification when:

* The lead asks for an agent
* The lead requests a human
* The lead needs assistance outside the AI's role
* The qualification process times out
* A workflow error occurs

Human handoff uses:

```text
AI Handoff Required = Yes
AI Handoff Status = Pending
```

---

# 7. Error Handling

The system distinguishes between normal qualification outcomes and system errors.

### Qualification timeout

```text
Time Out
   ↓
AI-Qualification-Error
   ↓
AI Handoff Required = Yes
   ↓
Human-Handoff
```

### Workflow error

System-level failures are logged and surfaced for technical review.

Error information can include:

* Error type
* Error timestamp
* Failed workflow/action
* Contact ID
* Error notes
* Handoff status

---

# 8. Dashboard

The AI Lead Qualification Dashboard provides operational visibility.

### Dashboard Widgets

**Hot Leads**

Shows the number of opportunities currently in the Hot Lead stage.

**AI Handoff Required**

Shows leads requiring human agent review.

**AI Errors**

Shows qualification records with an AI handoff status of Error.

**Opportunity Counts by Status**

Displays Open, Won and Lost opportunity distribution.

**Opportunity Revenue Over Time**

Displays pipeline value trends in Philippine Pesos.

---

# Custom Fields

## Qualification Fields

| Field             | Type        |
| ----------------- | ----------- |
| Lead Type         | Dropdown    |
| Property Type     | Dropdown    |
| Target Location   | Single Line |
| Budget            | Single Line |
| Financing         | Dropdown    |
| Purchase Timeline | Dropdown    |
| Pre-Approved      | Dropdown    |
| Motivation        | Long Text   |

## AI Fields

| Field                | Type        |
| -------------------- | ----------- |
| AI Score             | Number      |
| Qualification Status | Dropdown    |
| AI Handoff Required  | Dropdown    |
| AI Handoff Status    | Dropdown    |
| AI Handoff Date      | Date        |
| AI Error Notes       | Long Text   |
| Date Qualified       | Date        |
| GHL Contact ID       | Single Line |

---

# Tags

```text
AI-Qualified
AI-Nurture
AI-Unqualified
AI-Qualification-Complete
AI-Qualification-Error
Buyer
Seller
Human-Handoff
```

---

# Workflow Architecture

## Workflow 1 — AI Real Estate Lead Qualification

**Trigger:**

```text
Contact Created
```

### Flow

```text
Contact Created
      ↓
Conversation AI Bot
      │
      ├── Human Handoff
      │       ↓
      │     Human
      │
      ├── No Condition Met
      │       ↓
      │    Wait 15 sec
      │       ↓
      │  AI Extract Data
      │       ↓
      │ Update Contact Fields
      │       ↓
      │ AI-Qualification-Complete
      │
      └── Time Out
              ↓
       AI-Qualification-Error
              ↓
       AI Handoff Required = Yes
              ↓
          Human-Handoff
```

---

# Workflow 2 — AI-Qualification-Complete

**Trigger:**

```text
Contact Tag Added
AI-Qualification-Complete
```

### Flow

```text
AI-Qualification-Complete
          ↓
    Data Validation
          │
          ├── ERROR
          │    ├── AI-Qualification-Error
          │    ├── Human-Handoff
          │    ├── Handoff Required = Yes
          │    └── Handoff Status = Error
          │
          └── VALID
                ↓
          Qualification Status
                │
        ┌───────┼────────┬────────────┐
        ↓       ↓        ↓            ↓
      Hot    Qualified  Nurture    Unqualified
        ↓       ↓        ↓            ↓
      Human   Sales    Nurture      No Opp
     Follow   Alert    Pipeline
      Up
```

---

# Installation

This project is primarily a **GoHighLevel configuration project**, not a traditional software application.

## Requirements

* GoHighLevel account
* Conversation AI access
* Workflow access
* Opportunity pipeline access
* Dashboard/reporting access
* Appropriate custom-field permissions

## Setup

### 1. Create the Pipeline

Create:

```text
Real Estate Lead Qualification
```

with:

```text
New Lead
Hot Lead
Qualified
Nurture
Unqualified
```

### 2. Create Custom Fields

Create the fields listed in the Custom Fields section.

### 3. Create Tags

Create the tags listed in the Tags section.

### 4. Configure Conversation AI

Configure the AI personality, intent and qualification instructions.

### 5. Build Workflow 1

Configure:

```text
Contact Created
→ Conversation AI Bot
→ AI Extract Data
→ Contact Field Updates
→ AI-Qualification-Complete
```

### 6. Build Workflow 2

Configure:

```text
AI-Qualification-Complete
→ Validation
→ Qualification Routing
→ Opportunity Creation
→ Notifications
```

### 7. Configure Error Handling

Configure timeout and workflow-error handling.

### 8. Build Dashboard

Create the AI Lead Qualification Dashboard and add the specified metrics.

---

# Usage

Once configured, the system operates automatically.

### Example

A new lead enters the CRM:

```text
John
Buyer
Condo
Makati
₱5M
Mortgage
Pre-approved
0–3 months
```

Conversation AI collects the information.

The extraction layer structures the data.

The workflow determines:

```text
AI Score: 94
Qualification Status: Hot Lead
AI Handoff Required: Yes
AI Handoff Status: Pending
```

The opportunity is created:

```text
Pipeline:
Real Estate Lead Qualification

Stage:
Hot Lead
```

The sales team receives an urgent notification and takes over the conversation.

---

# Testing

The system should be tested using multiple scenarios.

| Scenario             | Expected Result                        |
| -------------------- | -------------------------------------- |
| Qualified Buyer      | Qualified opportunity + Buyer tag      |
| Qualified Seller     | Qualified opportunity + Seller tag     |
| Hot Lead             | Hot Lead opportunity + Human follow-up |
| Nurture Lead         | Nurture opportunity                    |
| Unqualified Lead     | No opportunity                         |
| Human Request        | Human-Handoff                          |
| AI Timeout           | AI-Qualification-Error + Human-Handoff |
| Validation Error     | Error status + Human-Handoff           |
| Duplicate Submission | No duplicate opportunity               |

---

# Security

Do not commit:

* API keys
* Access tokens
* Passwords
* Private client information
* Production contact records
* Personal customer data
* GHL credentials

Use environment variables for any external integrations.

---

# Project Status

**Status:** Portfolio / Test Project

The system is designed as a demonstration of:

* AI workflow automation
* CRM automation
* Conversational AI
* Structured data extraction
* Lead qualification
* Opportunity management
* Human-in-the-loop automation
* Error handling
* Operational dashboards

---

# License

This project is provided for portfolio and educational purposes.

See `LICENSE` for details.
