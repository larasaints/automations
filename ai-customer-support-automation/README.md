# AI Customer Support Automation

An AI-powered customer support workflow built with **n8n, Google Gemini, Airtable, Gmail, and Webhooks**.

This automation receives customer support messages, uses AI to classify and understand requests, generates a draft response, logs the ticket, and automatically routes sensitive or uncertain requests for human review.

---

## Project Overview

Customer support teams often spend significant time manually reviewing incoming messages, categorizing requests, creating tickets, and deciding which cases need escalation.

This workflow automates the initial support triage process while keeping humans in control of sensitive cases such as refund requests and low-confidence requests.

### Business Problem

Without automation, support teams may need to:

* Read every incoming customer message manually
* Determine the type of request
* Create or update support records
* Draft an initial response
* Identify requests that require escalation
* Monitor the status of each request
* Handle workflow failures manually

This can create repetitive work and increase the risk of inconsistent handling.

---

## Solution

The automation creates an AI-assisted customer support intake and triage system.

### Workflow

```text
Customer Message
       ↓
Webhook
       ↓
Duplicate Check
       ↓
AI Classification — Google Gemini
       ↓
Parse AI Result
       ↓
Create Support Ticket — Airtable
       ↓
AI Draft Response — Google Gemini
       ↓
Update Ticket
       ↓
Human Review Check
      ↙       ↘
    YES        NO
     ↓          ↓
Gmail Alert   Complete
     ↓
Airtable Status Update
```

---

## Business Objective

The goal is to reduce repetitive customer support triage work while maintaining human oversight for requests that require additional attention.

The system is designed to:

* Automatically categorize incoming requests
* Generate an initial customer-facing draft
* Store structured support information
* Detect duplicate messages
* Identify low-confidence requests
* Escalate refund-related requests
* Notify the support team when human review is required
* Track ticket processing status
* Log workflow errors

---

## AI Classification

Google Gemini analyzes each customer message and assigns exactly one category:

* **Billing**
* **Access**
* **Scheduling**
* **Technical Support**
* **General Question**
* **Refund Request**

The AI also generates:

* Confidence score
* Human review indicator
* Refund indicator
* Brief classification reason

The AI is instructed to return structured JSON so the workflow can reliably process the classification results.

---

## Human Review Logic

The workflow uses deterministic business rules in addition to the AI classification.

A request is routed for human review when **any** of the following conditions are met:

```text
Refund Request = TRUE
OR
Confidence Score < 80
OR
AI Human Review Required = TRUE
```

This prevents the workflow from relying solely on the AI's review decision.

### Examples

**Refund Request**

A customer reports being charged twice or requests money back.

→ Refund Request
→ Human Review Required
→ Internal Gmail notification

**Low Confidence**

A customer sends a vague message that does not clearly identify the issue.

→ Low confidence score
→ Human Review Required
→ Internal Gmail notification

**Normal Request**

A customer asks a straightforward question with high classification confidence.

→ No human review required
→ Ticket marked Completed

---

## AI Draft Response

After classification, Google Gemini generates a short customer-facing draft response.

The AI is instructed to:

* Be clear and professional
* Avoid inventing company policies or account information
* Avoid claiming that a refund has been approved
* Acknowledge refund requests without making final decisions
* Treat the output as a draft for a human support agent

This allows the support team to start with an AI-generated response while maintaining human control over final communication.

---

## Airtable Ticket Logging

Airtable acts as the support ticket database.

Each processed request can contain:

* Ticket ID
* Message ID
* Customer Name
* Customer Email
* Customer Message
* Category
* Confidence Score
* Draft Response
* Human Review Required
* Refund Request
* Status
* Created At
* Processed At
* Error Message

### Status Tracking

The workflow uses status values to track the processing lifecycle:

```text
Received
   ↓
Processing
   ↓
Completed

OR

Processing
   ↓
Human Review

OR

Processing
   ↓
Error
```

This provides operational visibility into the state of each support request.

---

## Duplicate Prevention

Before processing a new customer message, the workflow checks Airtable using the incoming **Message ID**.

If the Message ID already exists:

```text
Message Received
       ↓
Duplicate Check
       ↓
Duplicate Found
       ↓
Stop Processing
```

This prevents the same customer message from creating duplicate support tickets or triggering duplicate notifications.

---

## Internal Human Review Notification

When human review is required, Gmail sends an internal notification containing relevant information such as:

* Customer information
* Customer message
* Request category
* Confidence score
* Refund status
* AI-generated draft response
* Reason for review

The support team can then review the request before sending a final response.

---

## Error Handling

A separate n8n error workflow handles unexpected workflow failures.

```text
Workflow Error
      ↓
n8n Error Trigger
      ↓
Airtable Error Log
      ↓
Gmail Error Alert
```

The error workflow records the failure and sends an internal notification so the issue can be identified and addressed.

This provides basic monitoring and operational visibility without requiring the main workflow to handle every possible failure scenario directly.

---

## Testing

The workflow was tested using multiple scenarios.

### Test 1 — Normal Customer Question

**Message:**

> What are your customer support hours?

**Result:**

* Category: General Question
* Confidence: 98%
* Refund Request: No
* Human Review: No
* Status: Completed
* AI Draft Response: Generated

### Test 2 — Refund Request

**Example:**

> I was charged twice for my subscription.

**Result:**

* Category: Refund Request
* Refund Request: Yes
* Human Review: Yes
* Status: Human Review
* Internal Gmail notification: Sent

### Test 3 — Low Confidence Request

**Example:**

> Something is wrong with my account and I need help with this issue.

**Result:**

* Confidence: 45%
* Human Review: Yes
* Draft Response: Generated
* Status: Human Review

### Test 4 — Duplicate Message

A previously processed Message ID was submitted again.

**Result:**

* Existing Airtable record detected
* Duplicate processing prevented
* No duplicate support ticket created

### Test 5 — Error Handling

A workflow error was simulated and the error workflow was triggered.

**Result:**

* Error recorded in Airtable
* Gmail error alert sent successfully

---

## Tools & Technologies

| Tool              | Purpose                                   |
| ----------------- | ----------------------------------------- |
| **n8n**           | Workflow automation and orchestration     |
| **Google Gemini** | AI classification and response generation |
| **Airtable**      | Ticket database and status tracking       |
| **Gmail**         | Internal support and error notifications  |
| **Webhook**       | Customer message intake                   |

---

## Key Design Decisions

### AI + Deterministic Rules

AI handles natural-language understanding and classification, while deterministic workflow rules handle critical escalation conditions.

This allows the system to use AI where interpretation is needed while keeping important business rules predictable.

### Human-in-the-Loop

The automation does not attempt to fully replace human support.

Refund requests and uncertain cases are routed to a human before a final response is sent.

### Structured AI Output

The classification prompt requires valid JSON so n8n can reliably extract and process:

* Category
* Confidence score
* Human review status
* Refund status
* Classification reason

### Duplicate Prevention

Message IDs are checked before processing to prevent duplicate tickets and notifications.

### Centralized Logging

Airtable provides a central record of customer requests, classifications, drafts, review status, and processing status.

---

## Business Outcome

This workflow demonstrates how AI and automation can support a customer service operation by handling repetitive first-level triage tasks.

Instead of manually processing every incoming request, the system can:

**Receive → Understand → Classify → Draft → Log → Escalate when needed**

This creates a more structured support intake process while preserving human oversight for sensitive or uncertain requests.

---

## Portfolio Demonstration

A Loom walkthrough is available to demonstrate the workflow, configuration, testing, and business use case.

**Loom Demo:**


---

## Security & Credentials

No API keys, passwords, authentication tokens, or private credentials are included in this repository.

The live n8n workflow, connected accounts, API credentials, webhook configuration, and private business data are kept separate from the public portfolio documentation.

---

## Portfolio Documentation

This repository documents the workflow architecture, business logic, AI prompts, testing process, and results.

The live workflow configuration and connected credentials are kept private and are not included in the repository.

---

## Disclaimer

This is a portfolio project created to demonstrate AI automation, workflow design, customer support operations, and system integration skills.

The business, customer data, and scenarios used in this project are fictional or test data.
