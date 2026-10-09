# 🏠 AI Real Estate Lead Qualification System

**An AI-powered real estate lead qualification system built with GoHighLevel to automate initial lead engagement, qualification, CRM data organization, opportunity routing, human handoff, and performance visibility.**

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [High-Level Architecture](#-high-level-architecture)
- [Tech Stack](#-tech-stack)
- [Features](#-features)
- [Dashboard & Reporting](#-dashboard--reporting)
- [Installation](#-installation)
- [Usage](#-usage)
- [Testing](#-testing)
- [Loom Walkthrough](#-loom-walkthrough)
- [Business Value](#-business-value)

---

## 📖 Project Overview

Real estate teams often receive new leads that require immediate attention, but manually qualifying every lead can consume valuable time and create inconsistent follow-up.

This project demonstrates an automated AI qualification system that engages new leads, gathers relevant information, evaluates their qualification status, and organizes them within the CRM.

### The system is designed to help a real estate team:

- ✅ Respond to new leads quickly
- ✅ Collect structured qualification information
- ✅ Identify high-priority opportunities
- ✅ Separate qualified and lower-priority leads
- ✅ Flag leads requiring human attention
- ✅ Prevent unqualified leads from cluttering active opportunities
- ✅ Monitor qualification performance through a CRM dashboard

---

## 🏗️ High-Level Architecture

```text
New Lead
    ↓
AI Conversation
    ↓
Lead Qualification
    ↓
AI Data Extraction
    ↓
CRM Contact Update
    ↓
Qualification Status
    ↓
Lead Routing
    ↓
Opportunity Pipeline
    ↓
Human Handoff When Required
    ↓
Dashboard & Reporting
```


---

## 🛠️ Tech Stack

| Technology | Purpose |
|:-----------|:--------|
| **GoHighLevel** | CRM, workflows, contacts, opportunities, pipeline, and dashboard |
| **Conversation AI** | Conversational lead qualification |
| **CRM Workflows** | Automation and lead routing |
| **Custom Contact Fields** | Structured qualification data |
| **Opportunity Pipeline** | Lead progression and sales visibility |
| **Tags** | Lead categorization and workflow tracking |
| **Dashboard** | Qualification and pipeline reporting |

---

## ✨ Features

### 🤖 AI Lead Qualification

The AI engages new leads conversationally and collects the information required by the sales team.

The qualification process supports different lead types, including:
- Buyer
- Seller
- Investor
- Renter

---

### 📋 Structured CRM Data

Qualification information is stored in structured CRM fields rather than remaining only within the conversation.

Example data includes:
- Lead type
- Budget
- Timeline
- AI score
- Qualification status

---

### 🎯 Lead Classification

The system categorizes leads into different qualification outcomes:

| Qualification Status | Purpose |
|:--------------------:|:--------|
| **Hot Lead** | High-priority lead requiring immediate attention |
| **Qualified** | Lead meeting the qualification criteria |
| **Nurture** | Lead that may require future engagement |
| **Unqualified** | Lead that does not meet the active qualification criteria |

*Additional system states are used for Error and AI Handoff Required situations.*

---

### 🔀 Automated Lead Routing

Based on the qualification outcome, the system routes leads to the appropriate next step within the CRM and opportunity pipeline.

---

### 🚨 Human Handoff

Leads requiring human intervention can be identified and surfaced for team follow-up. This allows automation to handle the initial qualification process while keeping human agents involved when necessary.

---

### 🛡️ Error Handling

The system accounts for qualification errors and handoff scenarios so that leads do not silently fall through the workflow.

---

## 📊 Dashboard & Reporting

### AI Qualification Dashboard

The project includes a CRM dashboard designed to monitor:

1. **Hot Leads** - Count of hot leads
2. **Qualified Leads** - Count of qualified contacts
3. **Nurture Leads** - Count of nurture contacts
4. **Unqualified / Error / AI Handoff Required** - Combined count of leads requiring attention or falling outside the qualified categories
5. **Opportunity Counts by Status** - Opportunity distribution
6. **Leads by Lead Type** - Buyer, Seller, Investor, and Renter distribution
7. **Contacts by Tag** - Qualification workflow tagging
8. **Total Contacts** - Total contacts in the test set

> **Note:** The Unqualified / Error / AI Handoff Required KPI is intentionally a combined count. It accumulates leads belonging to any of those categories, while tags/statuses allow the individual categories to be differentiated.

---

### Dashboard Test Data

The dashboard was validated using six sample contacts representing different qualification outcomes:

| Contact | Lead Type | AI Score | Qualification |
|:--------|:---------:|:--------:|:--------------|
| Maria Santos | Buyer | 85 | Hot Lead |
| Juan Dela Cruz | Seller | 72 | Qualified |
| Ana Reyes | Buyer | 58 | Nurture |
| Pedro Garcia | Renter | 25 | Unqualified |
| Sofia Lopez | Investor | 92 | Hot Lead |
| Miguel Torres | Buyer | 68 | Qualified |

**Expected qualification distribution:**

```text
Hot Lead       → 2
Qualified      → 2
Nurture        → 1
Unqualified    → 1
-------------------
Total          → 6
```

The dashboard also tracks the broader combined category for **Unqualified / Error / AI Handoff Required** rather than treating those states as a single qualification outcome.

---


---

### Dashboard Metrics

The current dashboard contains eight widgets:

1. **Qualified** — count of qualified contacts
2. **Nurture** — count of nurture contacts
3. **Hot Leads** — count of hot leads
4. **Unqualified / Error / AI Handoff Required** — combined count of leads requiring attention or falling outside the qualified categories
5. **Opportunity Counts by Status** — opportunity distribution
6. **Leads by Lead Type** — Buyer, Seller, Investor, and Renter distribution
7. **Contacts by Tag** — qualification workflow tagging
8. **Total Contacts** — total contacts in the test set

---

## 📁 Repository Structure


```text
ai-real-estate-lead-qualification/
│
├── README.md
├── LICENSE
│
├── docs/
│   ├── system-architecture.md
│   
│
├── screenshots/
│   ├── 3 workflows.png
│   ├── contacts.png
│   ├── opprtunities-pipeline.png/lead-samples
│   └── dashboard.png
```

---


---

## ⚙️ Installation

This project is implemented within GoHighLevel rather than as a standalone software package.

To reproduce the general system:

1. Set up a GoHighLevel account or test sub-account
2. Create the required contact fields
3. Configure the qualification statuses and lead types
4. Create the real estate opportunity pipeline
5. Configure the AI qualification experience
6. Build the supporting CRM workflows
7. Configure the dashboard widgets
8. Add test contacts
9. Run the qualification workflow
10. Verify the resulting contact data, tags, opportunities, and dashboard metrics

> **Note:** The repository intentionally does not include private workflow exports, exact AI prompts, scoring formulas, internal conditions, field mappings, or complete CRM configuration.

---

## 🚀 Usage

The system follows this general process:

### 1. New Lead
A new real estate lead enters the CRM.

### 2. AI Conversation
The AI begins the initial qualification conversation.

### 3. Qualification
The system collects relevant information such as lead type, budget, timeline, and other qualification data.

### 4. CRM Update
The collected information is organized within the lead's CRM record.

### 5. Classification
The lead receives a qualification outcome such as:
- Hot Lead
- Qualified
- Nurture
- Unqualified

### 6. Routing
The lead is routed to the appropriate CRM or opportunity workflow.

### 7. Human Handoff
When human intervention is required, the lead is flagged for follow-up.

### 8. Dashboard Monitoring
The dashboard provides an overview of qualification activity, lead distribution, pipeline status, and contacts.

---

## ✅ Testing

The system was designed with a six-contact test set covering multiple lead types and qualification outcomes.

### Validation includes:

- ✅ Qualification status accuracy
- ✅ Lead type distribution
- ✅ AI score storage
- ✅ Opportunity creation
- ✅ Pipeline routing
- ✅ Tag assignment
- ✅ Human handoff identification
- ✅ Dashboard metric accuracy

### Expected qualification distribution:


**2 Hot Leads + 2 Qualified + 1 Nurture + 1 Unqualified = 6 contacts.**

---
### Loom Walkthrough
[View the Loom Video](https://www.loom.com/share/1702cbee673a4aeba5e459784b45feab)

---

## 💼 Business Value

The system demonstrates how AI and CRM automation can reduce manual qualification work while improving sales-team visibility.

### Potential business benefits include:

- 🚀 Faster initial lead response
- 📊 More consistent qualification
- 📁 Better organization of lead information
- ⚡ Faster identification of high-priority opportunities
- ⌨️ Reduced manual data entry
- 🤝 Improved human handoff
- 🎯 Cleaner opportunity pipeline
- 📈 Better visibility into lead distribution and qualification performance

---

## 📸 Screenshots

### System Architecture
<img width="1671" height="941" alt="AI Real Estate Lead Automation Architecture" src="https://github.com/user-attachments/assets/5c15ad6d-9499-4056-a2e2-4c143bc226d5" />

### 3 WORKFLOWS
<img width="1278" height="468" alt="workflow-ai-qualification" src="https://github.com/user-attachments/assets/aada762c-aadb-4298-953a-b943f829d05c" />
<img width="1289" height="505" alt="workflow-lead-routing-process" src="https://github.com/user-attachments/assets/4592a819-6128-469c-b895-843a9b990dca" />
<img width="1278" height="468" alt="workflow-ai-qualification" src="https://github.com/user-attachments/assets/24bcbaf8-a023-4336-aa78-8abad525438b" />


### AI Conversation
<img width="1536" height="808" alt="contact 2" src="https://github.com/user-attachments/assets/d18139c0-23bf-49b4-8aed-5beebab5b871" />
<img width="1535" height="798" alt="contact" src="https://github.com/user-attachments/assets/e95bbd8e-7f16-44b6-926e-e52753972043" />
<img width="1128" height="368" alt="contact" src="https://github.com/user-attachments/assets/da5c76ef-1625-4227-8e0d-40ad501c2580" />


### Opportunity Pipeline
<img width="798" height="498" alt="opportunities" src="https://github.com/user-attachments/assets/d994df3b-f114-4d1d-a234-8739cda620be" />


### AI Qualification Dashboard
<img width="1128" height="441" alt="dashboard" src="https://github.com/user-attachments/assets/4f9e65ae-d1f0-4679-821d-a60839ad46d4" />


---


