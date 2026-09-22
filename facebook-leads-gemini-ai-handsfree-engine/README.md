# 🤖 AI-Powered Facebook Lead Nurturing & Real Estate Automation Engine

An enterprise-grade automation pipeline designed for **Casa De Lara by SMDC** properties (Shore Residences Pasay & Jazz Residences Makati) to instantly capture inbound leads via real-time webhooks, log profiles into a CRM, execute routing logic, and leverage **Gemini AI** to dispatch automated responses via Gmail.

## 🚀 The Core Business Challenge
Eliminates follow-up delays by ensuring every lead receives a tailored property overview in under 2 minutes without manual intervention. Manual lead intake introduces critical latency, lowering conversion rates as hot buyer intent cools down within minutes. This pipeline provides complete hands-free system isolation, safely scaling operations without increasing headcount.

## 🛠️ Technical Architecture & Pipeline Topology
The workflow operates via a tightly decoupled, real-time asynchronous processing layout:

1. **Ingestion Layer (`facebook-lead-ads:New Lead (Instant)`)**: Intercepts real-time Webhook data from Facebook Instant Forms the exact millisecond a user interacts with the ad campaign.
2. **Database Tracking Layer (`google-sheets:addRows`)**: Commits raw lead data to a central database ledger, storing structured rows for user contact profiles, specific property interests, and targeting parameters.
3. **Logic Routing & Branch Segmentation (`builtin:BasicRouter`)**: Evaluates structural data parameters to divide traffic instantly into parallel property pipelines:
   * **Path Alpha:** Routes and manages incoming inquiries centered entirely on **Jazz Residences (Makati City)**.
   * **Path Beta:** Routes and manages incoming inquiries centered entirely on **Shore Residences (Pasay City)**.
4. **Operations & Scheduling Layer (`google-calendar:createEvent`)**: Books tracking milestones, client reminders, or viewing time blocks directly onto the master Google Calendar to retain immediate agent visibility.
5. **Context-Aware GenAI & Communication Layer (`gemini-ai` / `gmail:sendEmail`)**: Passes the localized lead context to Gemini using a strict system instruction matrix to write optimized, high-converting copy. It then hooks the raw text payload directly into Gmail to shoot an automated, outbound email straight to the client's inbox.
6. **Throughput Rate-Limiting Guardrail (`builtin:Sleep`)**: Executes a structural processing delay at the end of each operational branch, insulating free-tier API boundaries from concurrent request throttling during heavy campaign delivery.

## 📊 Business Rules & Segmented Profile Intelligence
* **Instant Dynamic Hook**: Captures immediate user attention by promising a 2-minute transparent turnaround directly on the primary ad copy, removing user friction caused by generic "PM sent" delays.
* **Granular Location Filtering**: Evaluates form-selected tags to separate high-tier corporate profiles searching in the Makati financial district from leisure-focused staycation leads prioritizing Pasay airport accessibility.

## 📊 System Architecture (Backend Workflow)
Below is the event-driven logical pipeline built to process, filter, and route the inbound real estate lead data:<br>
<img width="622" height="462" alt="make workflow" src="https://github.com/user-attachments/assets/a6e2967e-60b4-4713-b803-17a751915d7f" alt="Make.com System Architecture" width="500"/>


---
## 📅 Data Ingestion & State Database (Google Sheets)
This sheet functions as the operational database layer, catching inbound submissions and storing AI categorization states alongside calculated response strings: <br>
<img width="1280" height="500" alt="data ingestion" src="https://github.com/user-attachments/assets/e39b7662-36e5-439c-a0a5-02f44be1b5ac" alt="Google Sheets Data Tracking Layer" width="500"/>


---
## 📧 Live Output Generation Demo (Frontend Result)
This is the final production-ready automated email template dynamically calculated and instantly fired into the buyer's inbox by the engine:

### 🏢 Unit 1 Gmail Outbound AI Output Demo Test 01 & 02

| 🟢 Jazz Residences Available | 🔴 Jazz Residences Unavailable |
| :---: | :---: |
| <img width="1226" height="528" alt="Test Jazz Available" src="https://github.com/user-attachments/assets/0fac5b21-0f20-401f-bf32-eca036146033" alt="Jazz Available" width="280"/> | <img width="1202" height="542" alt="Test Jazz Unavailable" src="https://github.com/user-attachments/assets/1f72251d-9511-4f5c-becc-a18e834da6ad" alt="Jazz Unavailable" width="280"/> |

### 🏖️ Unit 2 Gmail Outbound AI Output Demo Test 03 & 04

| 🟢 Shore Residences Available | 🔴 Shore Residences Unavailable |
| :---: | :---: |
| <img width="1299" height="599" alt="Test Shore Available" src="https://github.com/user-attachments/assets/6df69fd2-0d30-4536-8c32-68016f10bc2a" alt="Shore Available" width="280"/> |<img width="1254" height="598" alt="Test Shore Unavailable" src="https://github.com/user-attachments/assets/440535ac-aa70-45ae-8d25-d4b4e9bf05bb" alt="Shore Unavailable" width="280"/> |
