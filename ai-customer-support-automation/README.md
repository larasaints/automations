# AI Customer Support Automation

An AI-powered customer support workflow built with **n8n, Google Gemini, Airtable, Gmail, and Webhooks**.

This automation streamlines the initial handling of customer support messages by automatically classifying requests, generating draft responses, identifying cases that require human review, preventing duplicate processing, tracking ticket status, and handling workflow errors.

---

## 📌 Project Overview

### Business Problem

Customer support teams often spend significant time performing repetitive tasks when new customer messages arrive:

* Reading and understanding each request
* Determining the type of issue
* Creating and updating support records
* Drafting initial responses
* Identifying requests that require escalation
* Tracking ticket status
* Monitoring workflow failures

When these steps are handled manually, support teams can spend valuable time on administrative work instead of focusing on customer issues that require human attention.

### Solution

This workflow uses **n8n and AI** to automate the initial support workflow.

When a new customer message is received, the system:

1. Receives the customer message through a webhook.
2. Checks Airtable for duplicate messages.
3. Uses Google Gemini to classify the request.
4. Generates an AI confidence score.
5. Determines whether the request involves a refund.
6. Determines whether human review is required.
7. Creates a support ticket in Airtable.
8. Generates a customer-facing draft response.
9. Updates the ticket with the AI-generated response.
10. Routes sensitive or uncertain requests for human review.
11. Sends an internal Gmail notification when review is required.
12. Tracks ticket status in Airtable.
13. Automatically logs workflow errors and sends an internal error notification.

---

## 🎯 Business Objective

The goal is not to completely replace human support agents.

Instead, the automation handles repetitive first-level support tasks so human agents can focus on:

* Sensitive customer requests
* Refund-related issues
* Unclear requests
* Low-confidence AI classifications
* Cases requiring human judgment

The workflow creates a **human-in-the-loop support process** where AI handles repetitive tasks while people retain control over important decisions.

---

## ⚙️ Workflow Architecture

```text
Customer Message
       ↓
Webhook
       ↓
Duplicate Check
       ↓
IF Duplicate?
   ├── TRUE → Stop
   │
   └── FALSE
          ↓
   Gemini AI Classification
          ↓
      Parse JSON
          ↓
   Airtable - Create Ticket
          ↓
   Gemini AI Draft Response
          ↓
   Airtable - Update Ticket
          ↓
   Check Human Review
       ├── TRUE
       │    ↓
       │   Gmail Internal Alert
       │    ↓
       │   Airtable Update
       │    ↓
       │   Human Review
       │
       └── FALSE
            ↓
         Completed
```

### Error Handling Workflow

```text
Main Workflow Error
       ↓
Error Trigger
       ↓
Airtable - Error Log
       ↓
Gmail - Error Alert
```

The error workflow operates separately from the main workflow and is automatically triggered when the configured main workflow encounters an error.

---

# 🔄 Main Workflow

## 1. Webhook — Receive Customer Message

The workflow begins when a new customer support message is received through an n8n webhook.

### Example input

```json
{
  "message_id": "MSG-001",
  "customer_name": "John Smith",
  "customer_email": "john@example.com",
  "message": "I was charged twice for my subscription."
}
```

The webhook provides the information required for the workflow to process the request.

---

## 2. Airtable — Duplicate Check

Before processing the message, the workflow searches Airtable using the incoming **Message ID**.

This prevents the same customer message from being processed multiple times.

### Duplicate logic

```text
Message ID already exists?
        ↓
   YES → Stop
   NO  → Continue
```

This provides basic duplicate prevention and helps avoid:

* Duplicate tickets
* Duplicate AI processing
* Duplicate notifications
* Unnecessary workflow executions

---

## 3. Gemini — AI Classification

Google Gemini analyzes the customer message and classifies it into exactly one support category.

### Supported categories

* Billing
* Access
* Scheduling
* Technical Support
* General Question
* Refund Request

Gemini also returns:

* Confidence score
* Human review recommendation
* Refund flag
* Classification reason

### Example AI output

```json
{
  "category": "Refund Request",
  "confidence_score": 100,
  "human_review_required": false,
  "refund_request": true,
  "reason": "The customer is reporting a double charge, which requires review."
}
```

The AI response is parsed into structured JSON so the individual values can be used by later n8n nodes.

---

# 🤖 AI Classification Rules

The classification prompt instructs Gemini to:

* Select exactly one category.
* Identify refund-related requests.
* Provide a confidence score from 0–100.
* Identify whether human review is required.
* Avoid inventing information.
* Return structured JSON.

The prompt is kept directly inside the workflow so it can be easily reviewed and edited.

---

# 👤 Human Review Logic

The workflow uses deterministic business rules in n8n to make sure sensitive cases are routed to a human.

### Human review is required when:

```text
Refund Request = TRUE
OR
Confidence Score < 80
OR
AI Human Review Required = TRUE
```

The IF node uses **ANY / OR** logic.

### Example

A customer message with:

```text
Refund Request = TRUE
Confidence = 100
AI Human Review = FALSE
```

still goes to human review because:

```text
Refund Request = TRUE
```

This prevents the AI's individual output from overriding an important business rule.

---

# ✍️ AI Draft Response

After classification, a second Gemini step generates a customer-facing draft response.

The AI is instructed to:

* Be professional and helpful.
* Keep the response clear.
* Avoid inventing company policies or information.
* Never claim that a refund has been approved.
* Avoid making final decisions on sensitive requests.
* Treat the output as a draft for a human support agent.

### Example

For a refund-related request, the AI can acknowledge the customer's concern and explain that the request will be reviewed without falsely promising a refund.

This keeps the workflow **human-in-the-loop**.

---

# 🗃️ Airtable — Ticket Logging

Airtable acts as the support ticket database and workflow status tracker.

### Support Tickets fields

| Field                 | Purpose                            |
| --------------------- | ---------------------------------- |
| Ticket ID             | Unique support ticket identifier   |
| Message ID            | Unique incoming message identifier |
| Customer Name         | Customer information               |
| Customer Email        | Customer contact information       |
| Customer Message      | Original customer request          |
| Category              | AI-generated support category      |
| Confidence Score      | AI confidence level                |
| Draft Response        | AI-generated response              |
| Human Review Required | Human escalation flag              |
| Refund Request        | Refund detection flag              |
| Status                | Current ticket status              |
| Created At            | Ticket creation date               |
| Processed At          | Processing completion date         |
| Error Message         | Error information when applicable  |

---

# 📊 Status Tracking

The workflow tracks the support request through different stages.

### Processing

When the ticket is initially created:

```text
Status = Processing
```

### Completed

If the request does not require human review:

```text
Status = Completed
```

### Human Review

If the request meets any human-review condition:

```text
Status = Human Review
Human Review Required = TRUE
```

### Error

Workflow errors are recorded separately:

```text
Status = Error
```

This provides visibility into the current state of each support request.

---

# 📧 Internal Human Review Notification

When the IF node determines that human review is required, Gmail sends an internal notification to the support team.

The notification includes information such as:

* Customer name
* Customer email
* Support category
* Confidence score
* Refund status
* Customer message
* AI-generated draft response
* Reason for review

The support agent can then review the request before sending a final response.

---

# 🛡️ Error Handling

A separate n8n error workflow provides basic error monitoring.

### Error workflow

```text
Error Trigger
      ↓
Airtable Error Log
      ↓
Gmail Error Alert
```

When an error occurs in the configured main workflow, the Error Trigger passes the error information to the error-handling workflow.

The system then:

1. Logs the error in Airtable.
2. Records the error message.
3. Records the error timestamp.
4. Sends an internal Gmail notification.

This makes workflow failures easier to identify and troubleshoot.

---

# 🧪 Testing

The workflow was tested using multiple scenarios.

## Test 1 — Normal Customer Question

**Input:**

> What are your customer support hours?

### Result

```text
Category: General Question
Confidence: 98
Refund Request: FALSE
Human Review Required: FALSE
Status: Completed
```

The request completed without triggering human review.

---

## Test 2 — Refund Request

**Input:**

> I was charged twice for my subscription and I want a refund.

### Expected behavior

```text
Category: Refund Request
Refund Request: TRUE
Human Review Required: TRUE
Status: Human Review
Internal Gmail Alert: Sent
```

The workflow correctly routes the request for human review.

---

## Test 3 — Low Confidence Request

**Input:**

> Something is wrong with my account and I need help with this issue.

### Result

```text
Confidence: 45
Refund Request: FALSE
Human Review Required: TRUE
Status: Human Review
```

Because the confidence score was below 80, the workflow routed the request to human review.

---

## Test 4 — Duplicate Message

The same Message ID was submitted again.

### Expected behavior

```text
Message ID already exists
        ↓
Duplicate = TRUE
        ↓
Stop processing
```

The existing Airtable record was found and the duplicate request was prevented from creating another ticket.

---

## Test 5 — Error Handling

A controlled workflow error was used to test the error-handling process.

### Result

```text
Workflow Error
      ↓
Error Trigger
      ↓
Airtable Error Log
      ↓
Gmail Error Alert
```

The error was successfully logged and an internal Gmail notification was sent.

---

# 🧰 Tools & Technologies

### Automation

**n8n**

Used to orchestrate the complete workflow, control business logic, parse AI output, route requests, and handle errors.

### AI

**Google Gemini**

Used for:

* Customer message classification
* Confidence scoring
* Refund detection
* Human-review recommendation
* Draft response generation

### Database / Logging

**Airtable**

Used for:

* Support ticket storage
* Customer message logging
* AI classification data
* Draft response storage
* Status tracking
* Human-review tracking
* Error logging

### Notifications

**Gmail**

Used for:

* Internal human-review alerts
* Workflow error notifications

### Input

**Webhook**

Used to receive customer support messages from an external application or system.

---

# 🧠 Automation Design Decisions

## AI + Deterministic Rules

AI is used where natural-language understanding is useful.

n8n handles critical business rules deterministically.

For example:

```text
Refund Request → Human Review
```

is enforced by the workflow even if the AI's `human_review_required` value is incorrect.

This creates a more controlled AI automation architecture.

---

## Structured AI Output

Gemini returns structured JSON for classification.

Example:

```json
{
  "category": "Billing",
  "confidence_score": 95,
  "human_review_required": false,
  "refund_request": false,
  "reason": "The customer is asking about a billing-related issue."
}
```

The structured output makes the AI result easier to process inside n8n.

---

## Human-in-the-Loop

The system does not automatically make sensitive decisions.

Instead, it identifies cases that require human attention and sends an internal notification.

This allows automation to handle repetitive work while keeping human oversight for important customer interactions.

---

# 📈 Business Outcome

This automation helps a support team reduce repetitive manual work involved in the initial handling of customer requests.

It provides:

* Faster initial request processing
* Automatic support categorization
* AI-assisted response drafting
* Consistent escalation rules
* Duplicate prevention
* Centralized ticket tracking
* Human oversight for sensitive cases
* Basic workflow error monitoring

The result is a structured support workflow where incoming requests can be automatically **received, understood, classified, logged, drafted, routed, and tracked**.

---

# 🔐 Security & Credentials

No API keys, passwords, or private credentials should be stored in this repository.

Credentials for:

* Google Gemini
* Airtable
* Gmail
* n8n

are configured separately inside the n8n environment.

Any exported n8n workflow should be reviewed before committing to GitHub to ensure credentials and sensitive information are not included.

---

# 📁 Suggested Project Structure

```text
ai-customer-support-automation/
│
├── README.md
│
├── workflow/
│   └── n8n-workflow.json
│
├── screenshots/
│   ├── workflow-overview.png
│   ├── airtable-tickets.png
│   └── error-handling.png
│
└── demo/
    └── test-results.md
```

---

# 💼 Portfolio Summary

**AI Customer Support Automation — n8n**

Designed and built an AI-powered customer support workflow using n8n, Google Gemini, Airtable, Gmail, and Webhooks. The system automatically classifies customer requests, generates draft responses, prevents duplicate processing, tracks ticket status, routes refunds and low-confidence cases for human review, and provides automated error logging and notifications.

### Key Skills Demonstrated

* n8n workflow automation
* AI workflow design
* Google Gemini integration
* Prompt engineering
* Structured JSON parsing
* Webhook automation
* Airtable database workflows
* Conditional logic
* Human-in-the-loop automation
* Duplicate prevention
* Status tracking
* Error handling
* Gmail notifications
* Business process automation

---

## 📌 Disclaimer

This is a fictional portfolio project created to demonstrate workflow automation, AI integration, business logic, and operational automation capabilities. Customer information and company details used in testing are sample data.
