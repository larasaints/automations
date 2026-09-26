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

---
### System Architecture

The automation architecture below shows how the five specialized Zaps connect with Google Forms, Google Sheets, Gmail, and the compliance dashboard.

<img width="1672" height="941" alt="UB architecture" src="https://github.com/user-attachments/assets/1e64615c-14a5-40ed-8987-727ea9895367" />

---
### Workflow Documentation

Each automation has its own technical documentation covering the trigger, workflow steps, automation logic, data handling, notifications, and business outcome.

| Workflow                                           | Purpose                                                               | Workflow                                           |              
| -------------------------------------------------- | --------------------------------------------------------------------- | -------------------------------------------------- |
| **ZAP 01 — New Compliance Submission**             | Captures new compliance submissions, creates compliance records, identifies reported issues, and triggers appropriate alerts. | [View Workflow Documentation](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/workflows/zap1-compliance-submission.md)             |
| **ZAP 02 — Issue Resolution & Corrective Action**  | Processes issue-resolution submissions, updates issue records, and triggers follow-up notifications when required.     | [View Workflow Documentation](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/workflows/zap2-issue-resolution.md)  |
| **ZAP 03 — Overdue Issue Monitoring & Escalation** | Automatically identifies overdue issues and sends a consolidated escalation report.     | [View Workflow Documentation](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/workflows/zap3-overdue-issue-monitoring-and-escalation.md) |
| **ZAP 04 — Document Expiration Monitoring**        | Monitors franchise documents and identifies expired or upcoming expirations requiring renewal action.   | [View Workflow Documentation](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/workflows/zap4-document-expiration-monitoring.md)        |
| **ZAP 05 — Missing Monthly Compliance Submission** | Checks active franchises for missing monthly submissions and sends consolidated reminders while preventing duplicate alerts.    | [View Workflow Documentation](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/workflows/zap5-missing-submission.md) |

---
## Project Evidence
The project includes screenshots demonstrating the configured workflows, automated notifications, and operational dashboard.

### Automation Screenshots
* [ZAP 01 — New Compliance Submission](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/screenshots/zap1%20workflow.jpg)
* [ZAP 02 — Issue Resolution & Corrective Action](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/screenshots/zap2%20workflow.jpg)
* [ZAP 03 — Overdue Issue Monitoring & Escalation](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/screenshots/zap3%20workflow.jpg)
* [ZAP 04 — Document Expiration Monitoring](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/screenshots/zap4%20workflow.jpg)
* [ZAP 05 — Missing Monthly Compliance Submission](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/screenshots/zap5%20workflow.jpg)

### Operations Dashboard

[View UrbanBite Compliance Dashboard](https://github.com/larasaints/automations/blob/3d7f0d567d97089b5b73bd3d5d5b388590732851/urbanbite-franchise-compliance/screenshots/operational%20dashboard.jpg)

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

---

## Key Automation Capabilities

This project demonstrates practical automation capabilities including:

* Multi-step Zapier workflow design
* Scheduled automation and recurring monitoring
* Google Sheets as an operational data layer
* Conditional filtering and routing
* Dynamic spreadsheet record updates
* Automated issue escalation
* Consolidated operational email reporting
* Document expiration monitoring
* Duplicate-alert prevention logic
* Cross-workflow data relationships
* Operational KPI dashboarding

## Project Outcome

The resulting system demonstrates how recurring franchise compliance processes can be transformed from manual monitoring into a structured, automated operational workflow.

The project was designed as a portfolio case study to demonstrate automation design, workflow logic, data management, exception handling, notification systems, and operational reporting.




