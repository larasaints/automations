# UrbanBite Franchise Compliance Automation

> A multi-Zap workflow automation project designed to automate franchise inspections, issue tracking, monthly compliance monitoring, document expiration alerts, and operational escalation.

**Project Type:** Workflow Automation / Operations <br>
**Business:** Fictional Franchise Network — UrbanBite <br>
**Role:** Automation Designer & Builder <br>
**Tools:** Zapier, Google Sheets, Google Forms, Gmail <br>

---

## Project Overview

UrbanBite Franchise Compliance Automation is a simulated operations system designed to demonstrate how recurring franchise compliance processes can be automated and monitored from a centralized workflow.

The system connects form submissions, Google Sheets data, automated notifications, escalation logic, and an operations dashboard to reduce manual monitoring and provide visibility into compliance issues.

The solution consists of five interconnected automation workflows covering compliance submissions, issue resolution, overdue escalation, document expiration, and missing monthly reports.


### Business Problem

UrbanBite manages multiple franchise locations that need regular compliance inspections. Without a centralized workflow, inspection results, unresolved issues, expiring documents, and monthly compliance checks can require repetitive manual monitoring and follow-up.

When these activities are managed manually, several operational problems can occur:
* Compliance submissions may be missed or delayed.
* Issues requiring corrective action can remain unresolved.
* Overdue issues may not be escalated promptly.
* Business permits and other required documents can approach expiration without timely follow-up.
* Monthly compliance submissions require repetitive manual checking.
* Operations teams may lack a centralized view of current compliance risks.

### The Automation Challenge

The objective of this project was to design a system that could automatically:
1. Capture and organize compliance submissions.
2. Identify issues and route them for corrective action.
3. Monitor unresolved and overdue issues.
4. Detect expired and upcoming document expirations.
5. Identify franchise locations with missing monthly submissions.
6. Send consolidated operational alerts instead of requiring manual daily checks.
7. Provide a centralized dashboard for compliance visibility.

### Automation Solution
I designed a five-Zap automation system that connects franchise inspection forms, operational spreadsheets, scheduled monitoring, issue resolution, escalation, expiration alerts, and monthly compliance reporting.

Instead of relying on a single automation, the solution uses five specialized workflows, each responsible for a different operational process.

### System Flow

**Google Forms / Google Sheets** <br>
↓  <br>
**Data Processing & Validation**  <br>
↓ <br>
**Compliance Records & Issue Tracking**  <br>
↓ <br>
**Automated Monitoring**  <br>
↓ <br>
**Alerts & Escalations**  <br>
↓ <br>
**Operations Dashboard**  <br>

### Five Automation Workflows  <br>

| Automation                                     | Purpose                                                                                                                       |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **ZAP 01 — New Compliance Submission**             | Captures new compliance submissions, creates compliance records, identifies reported issues, and triggers appropriate alerts. |
| **ZAP 02 — Issue Resolution & Corrective Action**  | Processes issue-resolution submissions, updates issue records, and triggers follow-up notifications when required.            |
| **ZAP 03 — Overdue Issue Monitoring & Escalation** | Automatically identifies overdue issues and sends a consolidated escalation report.                                           |
| **ZAP 04 — Document Expiration Monitoring**        | Monitors franchise documents and identifies expired or upcoming expirations requiring renewal action.                         |
| **ZAP 05 — Missing Monthly Compliance Submission** | Checks active franchises for missing monthly submissions and sends consolidated reminders while preventing duplicate alerts.  |

### Centralized Data Structure

Google Sheets acts as the operational data layer for the system.

The solution uses separate worksheets for:
* Franchise information
* Compliance reports
* Issues and corrective actions
* Required documents
* Form submissions
* Dashboard reporting

This structure allows each automation to read from and update the same operational data source while keeping different processes organized.

