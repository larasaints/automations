# UrbanBite Franchise Compliance Automation

> A self-directed workflow automation project designed to centralize franchise compliance monitoring, issue tracking, document expiration alerts, and operational escalation.

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

Franchise operations teams often need to monitor compliance across multiple locations while handling reports, corrective actions, document renewals, and follow-ups.

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

I designed UrbanBite as a centralized compliance automation system where operational events are captured, processed, monitored, and escalated through connected workflows.

Instead of relying on a single automation, the solution uses five specialized workflows, each responsible for a different operational process.

### System Flow

**Google Forms / Google Sheets**
↓
**Data Processing & Validation**
↓
**Compliance Records & Issue Tracking**
↓
**Automated Monitoring**
↓
**Alerts & Escalations**
↓
**Operations Dashboard**

### Five Automation Workflows

| Automation                                     | Purpose                                                                                                                       |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **01 — New Compliance Submission**             | Captures new compliance submissions, creates compliance records, identifies reported issues, and triggers appropriate alerts. |
| **02 — Issue Resolution & Corrective Action**  | Processes issue-resolution submissions, updates issue records, and triggers follow-up notifications when required.            |
| **03 — Overdue Issue Monitoring & Escalation** | Automatically identifies overdue issues and sends a consolidated escalation report.                                           |
| **04 — Document Expiration Monitoring**        | Monitors franchise documents and identifies expired or upcoming expirations requiring renewal action.                         |
| **05 — Missing Monthly Compliance Submission** | Checks active franchises for missing monthly submissions and sends consolidated reminders while preventing duplicate alerts.  |

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

