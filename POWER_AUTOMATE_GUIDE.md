# Power Automate + Adaptive Cards - Complete Integration Guide

Welcome to the comprehensive guide for building end-to-end workflows with Power Automate and Adaptive Cards! This guide will take you from understanding the basics to deploying production-ready automation workflows in Microsoft Teams.

## 📚 Table of Contents

1. [What is Power Automate?](#what-is-power-automate)
2. [Why Power Automate for Adaptive Cards?](#why-power-automate-for-adaptive-cards)
3. [Architecture & Key Concepts](#architecture--key-concepts)
4. [Prerequisites and Setup](#prerequisites-and-setup)
5. [Your First Power Automate Flow](#your-first-power-automate-flow)
6. [Understanding Triggers](#understanding-triggers)
7. [Understanding Actions](#understanding-actions)
8. [Working with Dynamic Content](#working-with-dynamic-content)
9. [Form Handling & Data Processing](#form-handling--data-processing)
10. [Approval Workflows](#approval-workflows)
11. [Advanced Patterns](#advanced-patterns)
12. [Error Handling & Reliability](#error-handling--reliability)
13. [Free vs Premium Features](#free-vs-premium-features)
14. [Best Practices](#best-practices)
15. [Troubleshooting](#troubleshooting)
16. [Production Deployment](#production-deployment)
17. [Additional Resources](#additional-resources)

---

## What is Power Automate?

**Power Automate** (formerly Microsoft Flow) is a cloud-based service that enables you to create automated workflows between applications and services without writing code. It's part of the Microsoft Power Platform.

### Key Characteristics

- **Low-Code/No-Code**: Build workflows through visual designer
- **Cloud-Based**: Runs on Microsoft's infrastructure, no server management
- **400+ Connectors**: Integrate with Microsoft services and third-party apps
- **Event-Driven**: Flows trigger automatically based on events
- **Scalable**: Handles from simple tasks to enterprise-scale automation

### Visual Example

Think of Power Automate as the "backend brain" for your Adaptive Cards:

```
┌─────────────────────┐
│   Adaptive Card     │ ← What user sees
│   (Display Layer)   │
└──────────┬──────────┘
           │ User clicks button
           ↓
┌─────────────────────┐
│  Power Automate     │ ← What processes the action
│  (Business Logic)   │
└──────────┬──────────┘
           │ Workflow executes
           ↓
┌─────────────────────┐
│  Data/Systems       │ ← Where data is stored/updated
│  (SharePoint, SQL)  │
└─────────────────────┘
```

📖 **Official Documentation**: [Power Automate Overview](https://learn.microsoft.com/power-automate/)

---

## Why Power Automate for Adaptive Cards?

### The Problem: One-Way Communication

Traditional Adaptive Card integrations (PowerShell/Python scripts) can **send** cards to Teams, but cannot:
- ❌ Receive form submissions
- ❌ Process button clicks
- ❌ Update cards dynamically
- ❌ Orchestrate multi-step workflows
- ❌ Integrate with business systems

**You need a backend service** to handle these interactions.

### The Solution: Power Automate as Your Backend

Power Automate provides:

✅ **Two-Way Communication**
- Send cards to Teams
- Receive and process user interactions
- Update cards in real-time

✅ **No Infrastructure Required**
- No servers to manage
- No deployment pipelines
- Microsoft handles hosting, scaling, security

✅ **Built-In Connectors**
- SharePoint, SQL, Teams (400+ services)
- No API authentication code needed
- Visual configuration

✅ **Business Process Automation**
- Approval workflows
- Data validation and transformation
- Multi-system integration
- Scheduled tasks

✅ **Enterprise Features**
- Audit logging
- Error handling
- Retry mechanisms
- Security & compliance

### Comparison: Script-Based vs Power Automate

| Feature | PowerShell/Python | Power Automate |
|---------|-------------------|----------------|
| **Send Cards** | ✅ Easy | ✅ Easy |
| **Receive Form Data** | ⚠️ Requires custom server | ✅ Built-in HTTP trigger |
| **Approvals** | ❌ Manual implementation | ✅ Built-in action |
| **Update Cards** | ❌ Not supported | ✅ Premium connector |
| **SharePoint Integration** | ⚠️ Requires auth code | ✅ Visual connector |
| **Error Handling** | ⚠️ Manual try-catch | ✅ Built-in retry logic |
| **Scheduling** | ⚠️ Requires task scheduler | ✅ Built-in recurrence |
| **Audit Trail** | ❌ Manual logging | ✅ Automatic run history |
| **Deployment** | ⚠️ Manual script distribution | ✅ Export/import flows |

---

## Architecture & Key Concepts

### Flow Architecture

A Power Automate flow consists of:

```
┌─────────────────────────────────────────────────────┐
│                    FLOW                              │
│                                                      │
│  ┌──────────────┐      ┌──────────────┐            │
│  │   TRIGGER    │──────▶│   ACTIONS    │            │
│  │              │      │              │            │
│  │ When this    │      │ Then do      │            │
│  │ happens...   │      │ these...     │            │
│  └──────────────┘      └──────────────┘            │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### Triggers (Flow Starts)

**Triggers** determine when a flow runs:

| Trigger Type | Description | Example Use Case |
|--------------|-------------|------------------|
| **Recurrence** | Run on schedule | Daily standup reminder card |
| **Manual** | Run on-demand | Test flows, admin tools |
| **HTTP Request** | External webhook | Receive Action.Submit data |
| **SharePoint** | Item created/modified | New request → send card |
| **Email** | Email received | Parse email → create card |
| **Teams** | Message posted | React to mentions |

### Actions (Flow Steps)

**Actions** are the steps your flow executes:

| Action Type | Description | Example Use Case |
|-------------|-------------|------------------|
| **HTTP** | Send HTTP request | POST card to Teams webhook |
| **Teams** (Premium) | Native Teams operations | Post card, update message |
| **Approval** | Start approval process | Request manager approval |
| **Condition** | Branch logic | If approved → then notify |
| **SharePoint** (Premium) | List operations | Store form data |
| **Send Email** | Email notification | Alert on submission |
| **Compose** | Build JSON | Construct card dynamically |
| **Parse JSON** | Extract data | Get form field values |

### Dynamic Content & Expressions

Power Automate uses **expressions** to work with data:

```
@{expression}       ← Expression syntax
@{utcNow()}        ← Current date/time
@{triggerBody()}   ← Data from trigger
@{outputs('Action_Name')?['field']}  ← Reference action output
```

### Connectors: Free vs Premium

| Connector | Tier | Use With Adaptive Cards |
|-----------|------|-------------------------|
| HTTP | ✅ Free | Send cards via webhook |
| Manual Trigger | ✅ Free | Test flows |
| Recurrence | ✅ Free | Scheduled cards |
| Approval | ✅ Free | Simple approvals |
| Send Email | ✅ Free | Notifications |
| **Teams Connector** | 💎 Premium | Post/update cards natively |
| **SharePoint** | 💎 Premium | Store form data |
| **SQL** | 💎 Premium | Database integration |
| **Custom Connectors** | 💎 Premium | Third-party APIs |

---

## Prerequisites and Setup

### Required Access

✅ **Microsoft 365 Account**
- Power Automate is included with most M365 licenses
- Check your license: https://admin.microsoft.com

✅ **Microsoft Teams**
- Access to a team/channel for testing
- Ability to add connectors (may require admin approval)

✅ **Web Browser**
- Edge, Chrome, Firefox, Safari
- Enable cookies and JavaScript

### Optional (For Premium Features)

💎 **Power Automate Premium License**
- Required for: Teams native connector, SharePoint, SQL, custom connectors
- Trial available: 90-day free trial at https://powerautomate.microsoft.com

💎 **SharePoint Online**
- For data storage examples
- Included with most M365 licenses

### Getting Started Checklist

- [ ] Verify Power Automate access: https://make.powerautomate.com
- [ ] Create Teams webhook (see [Quick Start](QUICK_START.md))
- [ ] Bookmark Adaptive Cards Designer: https://adaptivecards.io/designer
- [ ] (Optional) Start Power Automate Premium trial
- [ ] (Optional) Create SharePoint list for practice

---

## Your First Power Automate Flow

Let's build a simple flow that sends an Adaptive Card daily at 9 AM.

### Step 1: Create a New Flow

1. Go to https://make.powerautomate.com
2. Click **+ Create** in left sidebar
3. Select **Scheduled cloud flow**
4. Configure:
   - **Flow name**: "Daily Standup Reminder"
   - **Starting**: Tomorrow's date, 9:00 AM
   - **Repeat every**: 1 Day
5. Click **Create**

### Step 2: Add HTTP Action

1. Click **+ New step**
2. Search for "HTTP"
3. Select **HTTP** (under standard connectors)
4. Configure:
   - **Method**: `POST`
   - **URI**: `YOUR_TEAMS_WEBHOOK_URL` (from Teams connector setup)
   - **Headers**: Click "Add new parameter" → Headers
     - Key: `Content-Type`
     - Value: `application/json`
   - **Body**: Paste the JSON below

### Step 3: Card JSON with Dynamic Date

```json
{
  "type": "message",
  "attachments": [
    {
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": {
        "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
        "type": "AdaptiveCard",
        "version": "1.5",
        "body": [
          {
            "type": "Container",
            "style": "emphasis",
            "items": [
              {
                "type": "TextBlock",
                "text": "☀️ Good Morning Team!",
                "size": "large",
                "weight": "bolder",
                "color": "accent"
              }
            ]
          },
          {
            "type": "TextBlock",
            "text": "Time for our daily standup!",
            "wrap": true,
            "spacing": "medium"
          },
          {
            "type": "FactSet",
            "facts": [
              {
                "title": "📅 Date:",
                "value": "@{formatDateTime(utcNow(), 'dddd, MMMM dd, yyyy')}"
              },
              {
                "title": "🕐 Time:",
                "value": "9:00 AM"
              }
            ]
          },
          {
            "type": "TextBlock",
            "text": "**Discussion Points:**",
            "weight": "bolder",
            "spacing": "medium"
          },
          {
            "type": "TextBlock",
            "text": "• What did you accomplish yesterday?\n• What are you working on today?\n• Any blockers?",
            "wrap": true
          }
        ],
        "actions": [
          {
            "type": "Action.OpenUrl",
            "title": "Join Meeting",
            "url": "https://teams.microsoft.com/l/meetup-join/..."
          }
        ]
      }
    }
  ]
}
```

**Note**: The expression `@{formatDateTime(utcNow(), 'dddd, MMMM dd, yyyy')}` will dynamically insert today's date in the card!

### Step 4: Save and Test

1. Click **Save** (top right)
2. Click **Test** → **Manually** → **Test**
3. Click **Run flow** → **Done**
4. Check your Teams channel—you should see the card!
5. Click **✓** when flow succeeds

🎉 **Congratulations!** You've created your first Power Automate flow with Adaptive Cards!

### Step 5: Verify Schedule

- The flow will now run automatically every day at 9 AM
- Check run history: Click flow name → "28-day run history"
- Disable: Toggle "Off" at top of flow editor
- Edit: Change recurrence time in trigger settings

---

## Understanding Triggers

Triggers are the "when" of your flow. Let's explore the most common triggers for Adaptive Card workflows.

### 1. Recurrence Trigger (Scheduled)

**Use Cases**: Daily reports, weekly reminders, hourly status updates

**Configuration**:
```
Interval: 1
Frequency: Day, Week, Month, Hour, Minute
Time zone: (UTC+00:00) Dublin, Edinburgh, Lisbon, London
At these hours: 9
At these minutes: 0, 30
On these days: Monday, Wednesday, Friday
```

**Examples**:
- Every Monday at 9 AM: `Frequency: Week, On these days: Monday, At 9:00`
- Every 2 hours: `Frequency: Hour, Interval: 2`
- Last day of month: Use advanced options → "Month" → "day -1"

**Dynamic Content Available**:
- `utcNow()` - Current timestamp
- `formatDateTime(utcNow(), 'yyyy-MM-dd')` - Formatted date

### 2. Manual Trigger (Button)

**Use Cases**: Testing, admin tools, on-demand reports

**Configuration**:
```
Add inputs:
- Text: "Report Title"
- Number: "Days to Include"
- Yes/No: "Include Charts"
- Email: "Send to"
- File: "Attach Document"
```

**Access Dynamic Content**:
```
@{triggerBody()?['text']}          ← First text input
@{triggerBody()?['text_1']}        ← Second text input
@{triggerBody()?['number']}        ← Number input
@{triggerBody()?['boolean']}       ← Yes/No input (true/false)
```

**Running Manually**:
1. From flow editor: Click "Test" → "Manually"
2. From Power Automate app (mobile)
3. From Teams (add Power Automate app → My flows → Run)

### 3. HTTP Request Trigger (Webhook)

**Use Cases**: Receive Action.Submit data, external webhooks, API integrations

**Configuration**:
```
Request Body JSON Schema:
{
  "type": "object",
  "properties": {
    "feedbackType": { "type": "string" },
    "rating": { "type": "string" },
    "comments": { "type": "string" }
  }
}
```

**Important**: After saving, copy the **HTTP POST URL** from the trigger. This is your webhook endpoint.

**Access Data**:
```
@{triggerBody()?['feedbackType']}  ← Field from schema
@{triggerBody()?['rating']}        ← Another field
```

**Example Action.Submit Button** (in your Adaptive Card):
```json
{
  "type": "Action.Submit",
  "title": "Submit Feedback",
  "data": {
    "feedbackType": "Product Feedback",
    "rating": "5",
    "comments": "Great product!"
  }
}
```

**⚠️ Important**: Direct Action.Submit requires Teams app configuration. For simpler approach, use "Post your own adaptive card as Flow bot" action (Premium).

### 4. SharePoint Triggers (Premium)

**Use Cases**: New request submitted, item approval needed, document uploaded

**Common Triggers**:
- **When an item is created** - New list item added
- **When an item is created or modified** - Any change to item
- **For a selected item** - Manual run from SharePoint

**Configuration**:
```
Site Address: https://contoso.sharepoint.com/sites/YourSite
List Name: "Approval Requests"
Folder: (optional - filter to specific folder)
```

**Access Item Data**:
```
@{triggerBody()?['Title']}         ← List item Title
@{triggerBody()?['ID']}            ← Item ID
@{triggerBody()?['Author']?['DisplayName']}  ← Created by
```

### 5. Email Triggers

**Use Cases**: Parse email content → create card, email-to-Teams bridge

**Triggers**:
- **When a new email arrives (V3)** - Any new email
- **When a new email arrives in a shared mailbox** - Shared inbox

**Configuration**:
```
Folder: Inbox
To: specific email or leave blank for any
Subject Filter: "URGENT", "Incident"
Include Attachments: Yes/No
```

**Access Email Data**:
```
@{triggerBody()?['subject']}       ← Email subject
@{triggerBody()?['body']}          ← Email body (HTML)
@{triggerBody()?['from']}          ← Sender email
```

### 6. Teams Message Trigger (Premium)

**Use Cases**: React to @mentions, bot commands, channel messages

**Triggers**:
- **When a new channel message is added**
- **When keywords are mentioned**
- **For a selected message** (right-click in Teams)

**Configuration**:
```
Team: Select team
Channel: Select channel
Keywords: @FlowBot, #incident, urgent
```

---

## Understanding Actions

Actions are the "do this" steps of your flow. Let's explore the key actions for Adaptive Card workflows.

### 1. HTTP Action (Send Card to Webhook)

**Use**: Send cards via incoming webhook (free tier method)

**Configuration**:
```
Method: POST
URI: https://outlook.office.com/webhook/...
Headers:
  Content-Type: application/json
Body: { card JSON wrapped in Teams message envelope }
```

**Complete Example**:
```json
{
  "type": "message",
  "attachments": [{
    "contentType": "application/vnd.microsoft.card.adaptive",
    "content": {
      "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
      "type": "AdaptiveCard",
      "version": "1.5",
      "body": [
        {
          "type": "TextBlock",
          "text": "Status: @{variables('Status')}",
          "color": "@{if(equals(variables('Status'), 'Complete'), 'good', 'warning')}"
        }
      ]
    }
  }]
}
```

**Advanced Settings**:
- **Timeout**: 30 seconds (default), max 120 seconds
- **Retry Policy**: Configure exponential backoff
- **Authentication**: Usually none for webhooks, OAuth for APIs

### 2. Teams Connector Actions (Premium)

**Recommended for production** - Native Teams integration with advanced features.

#### Post Adaptive Card (as Flow bot)

**Use**: Send card to channel/chat with ability to get message ID for updates

**Configuration**:
```
Post as: Flow bot
Post in: Channel / Chat / Group chat
Team: Select team
Channel: Select channel
Adaptive Card: { card JSON - NO wrapper needed }
```

**Output** (save this!):
```
@{outputs('Post_adaptive_card_and_wait_for_a_response')?['messageId']}
```

**⭐ Key Difference from HTTP**: Returns message ID, enabling card updates!

#### Update a Message

**Use**: Dynamically update existing card (change status, add notes, etc.)

**Configuration**:
```
Message: @{variables('MessageId')}  ← From Post action
Update card: { updated card JSON }
```

**Example Flow**:
```
1. Post card (save message ID)
2. Wait for approval
3. Update card to show "Approved" status
```

#### Post Your Own Adaptive Card as Flow Bot

**Use**: Send card and wait for Action.Submit response

**Configuration**:
```
Post as: Flow bot
Post in: Channel
Adaptive Card: { card with Action.Submit buttons }
Update message: (optional) Success/failure messages
Should update card: Yes
```

**Output**:
```
@{outputs('Post_card')?['submitActionId']}  ← Which button clicked
@{outputs('Post_card')?['data']}            ← Form data submitted
```

**⭐ This is the easiest way to handle Action.Submit!**

### 3. Approval Actions

**Use**: Built-in approval process with email notifications

**Approval Types**:
- **Approve/Reject - First to respond** - First approver to respond decides
- **Approve/Reject - Everyone must approve** - All approvers must approve
- **Custom Responses** - Define your own response options

**Configuration**:
```
Approval type: Approve/Reject - First to respond
Title: "Expense Request - $@{variables('Amount')}"
Assigned to: manager@company.com
Details: Full request details
Item link: Link to SharePoint item
Item link description: "View Request"
```

**Output**:
```
@{outputs('Start_approval')?['body/outcome']}              ← "Approve" or "Reject"
@{outputs('Start_approval')?['body/responder/displayName']} ← Who responded
@{outputs('Start_approval')?['body/responseTime']}         ← When
@{outputs('Start_approval')?['body/comments']}             ← Approver comments
```

**Use with Condition**:
```
Condition: @{outputs('Start_approval')?['body/outcome']}
Equals: Approve
Then: [Send success email, update SharePoint]
Else: [Send rejection email]
```

### 4. Condition Action (If/Else Logic)

**Use**: Branch execution based on criteria

**Simple Condition**:
```
Choose a value: @{outputs('Approval')?['body/outcome']}
is equal to
Value: Approve
```

**Advanced Conditions** (click "..." → "Edit in advanced mode"):
```
@{and(
  equals(triggerBody()?['status'], 'Urgent'),
  greater(float(triggerBody()?['amount']), 1000)
)}
```

**Operators**:
- `equals(a, b)` - Equality
- `greater(a, b)` - Numeric comparison
- `less(a, b)` - Less than
- `contains(text, substring)` - String contains
- `and(condition1, condition2)` - Both true
- `or(condition1, condition2)` - Either true
- `not(condition)` - Negate

### 5. Compose Action (Build JSON)

**Use**: Build complex JSON structures, format data

**Example 1: Build Card Dynamically**
```json
{
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": @{variables('CardElements')}
}
```

**Example 2: Format Data**
```
Name: @{toUpper(triggerBody()?['name'])}
Date: @{formatDateTime(utcNow(), 'yyyy-MM-dd')}
Amount: $@{formatNumber(triggerBody()?['amount'], 'N2')}
```

**Access Output**:
```
@{outputs('Compose')}  ← Entire output
```

### 6. Parse JSON Action

**Use**: Extract fields from JSON data

**Configuration**:
```
Content: @{triggerBody()}
Schema: {
  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "age": { "type": "integer" }
  }
}
```

**Generate Schema**: Click "Use sample payload" → Paste JSON example

**Access Fields**:
```
@{body('Parse_JSON')?['name']}
@{body('Parse_JSON')?['age']}
```

### 7. Send Email Action

**Use**: Email notifications, alerts, confirmations

**Configuration**:
```
To: @{triggerBody()?['email']}
Subject: Your request has been approved
Body: HTML or plain text with dynamic content
Attachments: (optional)
```

**HTML Email Body**:
```html
<h2>Request Approved</h2>
<p>Your request for <strong>@{triggerBody()?['title']}</strong> has been approved.</p>
<p>Approved by: @{outputs('Approval')?['body/responder/displayName']}</p>
```

### 8. SharePoint Actions (Premium)

**Common Actions**:
- **Create item** - Add new list item
- **Update item** - Modify existing item
- **Get item** - Retrieve item details
- **Get items** - Query multiple items

**Create Item Example**:
```
Site Address: https://contoso.sharepoint.com/sites/Requests
List Name: "Approval Requests"
Title: @{triggerBody()?['title']}
Status: Pending
Amount: @{triggerBody()?['amount']}
Requester: @{triggerBody()?['email']}
CreatedDate: @{utcNow()}
```

**Query Items (Get Items)**:
```
Filter Query: Status eq 'Pending' and Amount gt 1000
Order By: Created desc
Top Count: 10
```

### 9. Variables

**Use**: Store values for later use in flow

**Types**: String, Integer, Float, Boolean, Array, Object

**Actions**:
- **Initialize variable** - Create at start of flow
- **Set variable** - Update value
- **Append to string** - Concat to string variable
- **Append to array** - Add item to array

**Example**:
```
Initialize variable:
  Name: MessageId
  Type: String
  Value: (leave empty)

Later: Set variable
  Name: MessageId
  Value: @{outputs('Post_card')?['messageId']}
```

---

## Working with Dynamic Content

Dynamic content and expressions are the "glue" that makes flows intelligent.

### Expression Syntax

All expressions use `@{expression}` syntax:

```
@{utcNow()}                    ← Function call
@{triggerBody()?['field']}    ← Safe navigation with ?
@{variables('MyVar')}          ← Variable reference
@{outputs('ActionName')}       ← Action output
```

### Common Functions

#### Date/Time Functions

```
@{utcNow()}
→ 2026-04-14T15:30:00.0000000Z

@{formatDateTime(utcNow(), 'yyyy-MM-dd')}
→ 2026-04-14

@{formatDateTime(utcNow(), 'dddd, MMMM dd, yyyy')}
→ Monday, April 14, 2026

@{formatDateTime(utcNow(), 'hh:mm tt')}
→ 03:30 PM

@{addDays(utcNow(), 7)}
→ Date 7 days from now

@{addHours(utcNow(), -5)}
→ 5 hours ago

@{dayOfWeek(utcNow())}
→ 1 (Monday=1, Sunday=0)

@{startOfMonth(utcNow())}
→ First day of current month
```

**Format Strings**:
- `yyyy` - 4-digit year
- `MM` - 2-digit month
- `dd` - 2-digit day
- `HH` - 24-hour format
- `hh` - 12-hour format
- `mm` - Minutes
- `ss` - Seconds
- `tt` - AM/PM

#### String Functions

```
@{toUpper('hello')}
→ HELLO

@{toLower('HELLO')}
→ hello

@{concat('Hello', ' ', 'World')}
→ Hello World

@{substring('Hello World', 0, 5)}
→ Hello

@{replace('Hello World', 'World', 'Teams')}
→ Hello Teams

@{length('Hello')}
→ 5

@{trim('  spaces  ')}
→ spaces

@{split('a,b,c', ',')}
→ ["a", "b", "c"]

@{join(split('a,b,c', ','), '; ')}
→ a; b; c
```

#### Number Functions

```
@{add(5, 3)}
→ 8

@{sub(10, 3)}
→ 7

@{mul(4, 5)}
→ 20

@{div(10, 2)}
→ 5

@{mod(10, 3)}
→ 1 (remainder)

@{formatNumber(1234.5, 'N2')}
→ 1,234.50

@{formatNumber(1234.5, 'C2')}
→ $1,234.50

@{float('123.45')}
→ 123.45 (convert string to number)

@{int('123')}
→ 123 (convert to integer)
```

#### Logic Functions

```
@{if(equals(5, 5), 'yes', 'no')}
→ yes

@{equals('a', 'a')}
→ true

@{greater(10, 5)}
→ true

@{less(5, 10)}
→ true

@{and(true, true)}
→ true

@{or(true, false)}
→ true

@{not(false)}
→ true

@{empty('')}
→ true

@{coalesce(null, null, 'default')}
→ default
```

#### Array Functions

```
@{length(split('a,b,c', ','))}
→ 3

@{first(split('a,b,c', ','))}
→ a

@{last(split('a,b,c', ','))}
→ c

@{contains(split('a,b,c', ','), 'b')}
→ true

@{union(array1, array2)}
→ Combined array

@{intersection(array1, array2)}
→ Common elements
```

### Accessing Trigger Data

Different triggers provide different data:

**Manual Trigger**:
```
@{triggerBody()?['text']}      ← First text input
@{triggerBody()?['text_1']}    ← Second text input
@{triggerBody()?['number']}    ← Number input
```

**HTTP Request**:
```
@{triggerBody()?['fieldName']}  ← From JSON schema
@{triggerBody()}                ← Entire request body
```

**SharePoint**:
```
@{triggerBody()?['Title']}                    ← List item title
@{triggerBody()?['ID']}                       ← Item ID
@{triggerBody()?['Author']?['DisplayName']}   ← Created by
@{triggerBody()?['Modified']}                 ← Last modified date
@{triggerBody()?['{Link}']}                   ← Link to item
```

**Recurrence**:
```
@{utcNow()}                    ← Current time when flow runs
```

### Accessing Action Outputs

Reference previous action results:

```
@{outputs('ActionName')}                    ← Entire output
@{outputs('ActionName')?['body']}          ← Body only
@{outputs('ActionName')?['body/field']}    ← Specific field
@{body('ActionName')?['field']}            ← Shorthand for body/field
```

**Example: Get HTTP Response**:
```
@{outputs('HTTP')?['statusCode']}          ← 200, 404, etc.
@{outputs('HTTP')?['body']}                ← Response body
```

**Example: Get Approval Outcome**:
```
@{outputs('Start_approval')?['body/outcome']}
@{outputs('Start_approval')?['body/responder/email']}
```

### Building Dynamic Cards

Combine expressions in JSON:

```json
{
  "type": "TextBlock",
  "text": "Status: @{variables('Status')}",
  "color": "@{if(equals(variables('Status'), 'Complete'), 'good', if(equals(variables('Status'), 'In Progress'), 'warning', 'attention'))}",
  "weight": "bolder"
}
```

```json
{
  "type": "FactSet",
  "facts": [
    {
      "title": "Submitted:",
      "value": "@{formatDateTime(triggerBody()?['Created'], 'yyyy-MM-dd HH:mm')}"
    },
    {
      "title": "Amount:",
      "value": "$@{formatNumber(float(triggerBody()?['Amount']), 'N2')}"
    },
    {
      "title": "Status:",
      "value": "@{if(equals(body('Get_item')?['Status'], 'Approved'), '✅ Approved', '⏳ Pending')}"
    }
  ]
}
```

### Tips for Expressions

1. **Use the Expression Editor**: Click in a field → "Expression" tab → Browse functions
2. **Test with Compose**: Use Compose action to test expressions before using in cards
3. **Safe Navigation (`?`)**: Always use `?` for optional fields: `@{body('Parse')?['field']}`
4. **Escape Quotes**: In JSON, escape `"` as `\"`
5. **Check Run History**: Failed runs show which expression caused the error

---

## Form Handling & Data Processing

Learn how to receive, validate, and process form submissions from Adaptive Cards.

### Action.Submit Pattern

When a user clicks an Action.Submit button, the card sends all Input field values.

**Card with Inputs**:
```json
{
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "Input.Text",
      "id": "name",
      "label": "Your Name",
      "isRequired": true
    },
    {
      "type": "Input.ChoiceSet",
      "id": "priority",
      "label": "Priority",
      "choices": [
        {"title": "Low", "value": "low"},
        {"title": "High", "value": "high"}
      ]
    },
    {
      "type": "Input.Toggle",
      "id": "notify",
      "title": "Send me updates",
      "value": "false"
    }
  ],
  "actions": [
    {
      "type": "Action.Submit",
      "title": "Submit",
      "data": {
        "action": "submitForm",
        "formId": "feedback-001"
      }
    }
  ]
}
```

**Submitted Data Structure**:
```json
{
  "action": "submitForm",
  "formId": "feedback-001",
  "name": "John Doe",
  "priority": "high",
  "notify": "true"
}
```

### Receiving Submissions (HTTP Trigger)

**Flow Setup**:

1. **Trigger**: When a HTTP request is received
2. **Request Body JSON Schema**:
```json
{
  "type": "object",
  "properties": {
    "action": {"type": "string"},
    "formId": {"type": "string"},
    "name": {"type": "string"},
    "priority": {"type": "string"},
    "notify": {"type": "string"}
  }
}
```

3. **Access Data**:
```
@{triggerBody()?['name']}
@{triggerBody()?['priority']}
@{triggerBody()?['notify']}
```

### Validation

Add validation logic after receiving data:

**Condition: Check Required Fields**:
```
@{and(
  not(empty(triggerBody()?['name'])),
  not(empty(triggerBody()?['priority']))
)}
```

**If Invalid**:
- Send error response
- Post error card to Teams
- Log validation failure

**If Valid**:
- Continue processing

### Data Transformation

**Convert String to Boolean**:
```
Initialize variable: isNotifyEnabled (Boolean)
Value: @{equals(triggerBody()?['notify'], 'true')}
```

**Convert String to Number**:
```
Initialize variable: amountNum (Float)
Value: @{float(triggerBody()?['amount'])}
```

**Parse Date**:
```
@{formatDateTime(triggerBody()?['dueDate'], 'yyyy-MM-dd')}
```

### Storing Form Data

**Option 1: SharePoint List** (Premium)

```
Action: Create item (SharePoint)
Site: https://contoso.sharepoint.com/sites/FormSubmissions
List: "Feedback Forms"
Title: @{triggerBody()?['name']}
Priority: @{triggerBody()?['priority']}
Comments: @{triggerBody()?['comments']}
SubmittedDate: @{utcNow()}
```

**Option 2: Excel Online** (Premium)

```
Action: Add a row into a table
Location: OneDrive
Document Library: Documents
File: Feedback.xlsx
Table: Table1
Columns: {
  "Name": "@{triggerBody()?['name']}",
  "Priority": "@{triggerBody()?['priority']}",
  "Date": "@{formatDateTime(utcNow(), 'yyyy-MM-dd')}"
}
```

**Option 3: Email Notification** (Free)

```
Action: Send an email (V2)
To: admin@company.com
Subject: New Form Submission from @{triggerBody()?['name']}
Body:
  Name: @{triggerBody()?['name']}
  Priority: @{triggerBody()?['priority']}
  Comments: @{triggerBody()?['comments']}
  Submitted: @{formatDateTime(utcNow(), 'yyyy-MM-dd HH:mm')}
```

**Option 4: SQL Database** (Premium)

```
Action: Insert row (V2)
Server: yourserver.database.windows.net
Database: FormData
Table: Submissions
Columns: {
  "Name": "@{triggerBody()?['name']}",
  "Priority": "@{triggerBody()?['priority']}",
  "SubmittedDate": "@{utcNow()}"
}
```

### Sending Confirmation

After processing, send confirmation back to Teams:

```json
{
  "type": "message",
  "attachments": [{
    "contentType": "application/vnd.microsoft.card.adaptive",
    "content": {
      "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
      "type": "AdaptiveCard",
      "version": "1.5",
      "body": [
        {
          "type": "Container",
          "style": "good",
          "items": [{
            "type": "TextBlock",
            "text": "✅ Submission Received",
            "size": "large",
            "weight": "bolder"
          }]
        },
        {
          "type": "TextBlock",
          "text": "Thank you, @{triggerBody()?['name']}! Your feedback has been recorded.",
          "wrap": true
        },
        {
          "type": "FactSet",
          "facts": [
            {"title": "Priority", "value": "@{triggerBody()?['priority']}"},
            {"title": "Received", "value": "@{formatDateTime(utcNow(), 'yyyy-MM-dd HH:mm')}"},
            {"title": "Reference #", "value": "@{guid()}"}
          ]
        }
      ]
    }
  }]
}
```

---

## Approval Workflows

Build sophisticated approval processes with Power Automate's built-in Approval actions.

### Single Approver Pattern

**Flow**:
1. Trigger: HTTP request or SharePoint item created
2. Post approval request card to Teams
3. Start and wait for approval
4. Condition: Approved or Rejected
5. Update requestor via email/Teams
6. Log decision to SharePoint

**Approval Action Configuration**:
```
Approval type: Approve/Reject - First to respond
Title: Expense Request - $@{triggerBody()?['amount']}
Assigned to: manager@company.com
Details:
  Requested by: @{triggerBody()?['name']}
  Amount: $@{triggerBody()?['amount']}
  Description: @{triggerBody()?['description']}
Item link: @{triggerBody()?['{Link}']}
Item link description: View Request Details
```

**Conditional Logic**:
```
If @{outputs('Start_approval')?['body/outcome']} equals "Approve":
  - Send approval email
  - Update SharePoint status to "Approved"
  - Post success card
Else:
  - Send rejection email
  - Update SharePoint status to "Rejected"
  - Post rejection card with reason
```

### Sequential Approver Pattern (Multi-Level)

**Use Case**: Manager → Director → Finance approval chain

**Flow**:
```
1. Post request card
2. Manager approval
   └─ If Reject → Stop
   └─ If Approve → Continue
3. Update card: "Manager Approved ✅"
4. Director approval (if amount > $5,000)
   └─ If Reject → Stop
   └─ If Approve → Continue
5. Update card: "Director Approved ✅"
6. Finance approval (if amount > $25,000)
   └─ If Reject → Stop
   └─ If Approve → Final Approval
7. Update card: "Fully Approved ✅"
8. Notify all stakeholders
```

**Implementation**:
- Use nested Conditions
- Update card after each stage
- Store approval history in SharePoint
- Set timeouts for each level

### Parallel Approver Pattern (Committee)

**Use Case**: Both Legal AND Finance must approve

**Approval Configuration**:
```
Approval type: Approve/Reject - Everyone must approve
Title: Contract Review
Assigned to: legal@company.com; finance@company.com
```

**Result**:
- Flow waits until BOTH approve
- If either rejects, entire request is rejected
- Access individual responses:
  ```
  @{outputs('Start_approval')?['body/responses']}
  ```

### Escalation Pattern

**Use Case**: If manager doesn't respond in 24 hours, escalate to director

**Implementation**:

```
┌─ Start Approval (Manager) ──────────────────┐
│                                              │
│  Parallel Branch 1: Wait for Approval       │
│  Parallel Branch 2: Delay 24 hours          │
└──────────────────────────────────────────────┘
                    │
                    ↓
            Check if Still Pending
                    │
        ┌───────────┴──────────┐
        ↓                      ↓
   Still Pending          Already Responded
        │                      │
   Escalate to            Do Nothing
   Director
```

**Actions**:
1. Start approval (Manager)
2. Add parallel branch:
   - Branch A: Waits for approval response
   - Branch B: Delay for 24 hours → Check status → Escalate if pending
3. Use variable to track if responded

### Dynamic Approver Routing

**Use Case**: Route to different approvers based on category

**Implementation**:

```
Switch action: @{triggerBody()?['category']}
  Case "IT": Send to IT Director
  Case "HR": Send to HR Manager
  Case "Finance": Send to CFO
  Default: Send to General Manager
```

---

## Advanced Patterns

### Card Update Pattern (Status Changes)

**Scenario**: Task card that updates as status changes

**Architecture**:
```
Flow 1: Create Task
  1. Post initial card
  2. Save message ID to SharePoint

Flow 2: Status Change (triggered by SharePoint)
  1. Get message ID from SharePoint
  2. Get updated task details
  3. Build updated card JSON
  4. Update Teams message
```

**Key Action (Premium)**:
```
Action: Update a message
Message: @{body('Get_item')?['CardMessageId']}
Update card: {
  ... updated card JSON with new status ...
}
```

### Scheduled Digest Pattern

**Scenario**: Daily summary of pending items

**Flow**:
```
1. Trigger: Recurrence (Daily at 8 AM)
2. Get items from SharePoint
   Filter: Status eq 'Pending'
   Order by: Created desc
3. Initialize variable: CardElements (Array)
4. Apply to each item:
   5. Append to CardElements:
      {
        "type": "FactSet",
        "facts": [
          {"title": "Request", "value": "@{items('Apply_to_each')?['Title']}"},
          {"title": "Amount", "value": "$@{items('Apply_to_each')?['Amount']}"}
        ],
        "separator": true
      }
6. Compose final card:
   body: @{variables('CardElements')}
7. POST card to Teams
```

### Child Flow Pattern (Reusable Logic)

**Scenario**: Multiple flows need to send the same type of card

**Solution**: Create child flow, call from parent flows

**Child Flow ("Send Status Card")**:
```
1. Trigger: When HTTP request received
   Schema: {"title": "string", "status": "string", "webhookUrl": "string"}
2. Compose card JSON with @{triggerBody()?['title']} and status
3. HTTP POST to @{triggerBody()?['webhookUrl']}
4. Respond to HTTP with status
```

**Parent Flow**:
```
Action: HTTP
Method: POST
URI: [Child flow HTTP trigger URL]
Body: {
  "title": "Task Complete",
  "status": "success",
  "webhookUrl": "@{variables('WebhookURL')}"
}
```

### Error Recovery with Retry

**Pattern**: Retry failed HTTP calls with exponential backoff

**Implementation**:
```
1. Initialize variable: RetryCount = 0
2. Do until @{or(equals(variables('Success'), true), greater(variables('RetryCount'), 3))}
   3. HTTP action
   4. Condition: Status code = 200
      If Yes: Set Success = true
      If No: 
        - Increment RetryCount
        - Delay: @{mul(variables('RetryCount'), 5)} seconds
```

**OR use built-in retry**:
```
HTTP Action → ... → Settings → Retry Policy
  Count: 4
  Interval: PT5S (5 seconds)
  Type: Exponential
```

### Batch Processing Pattern

**Scenario**: Process 100 SharePoint items, send summary card

**Flow**:
```
1. Get items (Top count: 100)
2. Initialize variable: ProcessedCount = 0, FailedCount = 0
3. Apply to each item:
   4. Try: Process item
   5. If success: Increment ProcessedCount
   6. If fail: Increment FailedCount
7. After loop: Send summary card
   "Processed: @{variables('ProcessedCount')}"
   "Failed: @{variables('FailedCount')}"
```

**Optimization**: Use "Degree of Parallelism" in Apply to each (up to 50 concurrent)

---

## Error Handling & Reliability

Building robust flows that handle failures gracefully.

### Configure Run After

Every action can be configured to run only under certain conditions:

**Access**: Click "..." on action → "Configure run after"

**Options**:
- ☑ **is successful** (default)
- ☐ **has failed**
- ☐ **is skipped**
- ☐ **has timed out**

**Example: Error Handler**:
```
Scope: Main Logic
  ├─ Action 1
  ├─ Action 2
  └─ Action 3

Scope: Error Handler (configure to run if Main Logic "has failed")
  ├─ Compose: Error details
  ├─ Send email to admin
  └─ Post error card to Teams
```

### Scope Actions

Group related actions for better error handling:

```
Scope: "Try"
  ├─ Get SharePoint item
  ├─ Process data
  └─ Update item

Scope: "Catch" (runs if Try fails)
  ├─ Log error
  └─ Send alert

Scope: "Finally" (always runs)
  └─ Send completion notification
```

### Retry Policies

Configure automatic retries for transient failures:

**HTTP Action**:
```
Settings → Retry Policy:
  Type: Exponential (recommended) or Fixed
  Count: 4 (will try 5 times total)
  Interval: PT10S (10 seconds)
  Minimum Interval: PT5S
  Maximum Interval: PT1H
```

**Exponential Backoff**:
- Try 1: Immediate
- Try 2: 10 seconds
- Try 3: 20 seconds
- Try 4: 40 seconds
- Try 5: 80 seconds (capped at max interval)

### Error Information

Access error details in "Catch" scope:

```
@{result('Scope_Try')[0]['error']['message']}
@{result('Scope_Try')[0]['error']['code']}
@{result('Scope_Try')[0]['outputs']}
```

**Send Error Alert**:
```
Action: Send email
To: admin@company.com
Subject: Flow Error: @{workflow()?['name']}
Body:
  Flow: @{workflow()?['name']}
  Run ID: @{workflow()?['run']?['name']}
  Error: @{result('Scope_Try')[0]['error']['message']}
  Time: @{utcNow()}
```

### Timeout Configuration

Set maximum execution time:

**Action Settings**:
```
Timeout: PT2M (2 minutes)
```

**Format**: ISO 8601 duration
- PT30S = 30 seconds
- PT5M = 5 minutes
- PT1H = 1 hour

### Monitoring & Alerts

**Built-In Monitoring**:
1. Flow run history (28 days retention)
2. Analytics dashboard
3. Email on failure (configure in flow settings)

**Custom Alerts**:
```
Create separate flow:
  Trigger: Recurrence (every hour)
  Get failed runs in last hour
  If any failed: Send alert email with details
```

---

## Free vs Premium Features

Understanding what's included in your license vs what requires Power Automate Premium.

### Free Tier (Included with M365)

✅ **Triggers**:
- Recurrence
- Manual
- HTTP Request
- Email

✅ **Actions**:
- HTTP
- Manual trigger
- Send email (V2)
- Compose
- Parse JSON
- Condition
- Scope
- Variables (Initialize, Set, Append)
- Apply to each
- Do until
- Approval (built-in action)

✅ **Limits**:
- 750 runs per day per user
- 5-minute flow run timeout
- 100 MB per run

### Premium Features (Requires Premium License)

💎 **Premium Connectors**:
- Microsoft Teams (native connector)
- SharePoint
- SQL Server
- Dataverse
- Azure services
- Custom connectors
- On-premises data gateway

💎 **Advanced Features**:
- RPA (Robotic Process Automation)
- AI Builder
- Process Advisor
- Longer run times (24+ hours)
- Higher limits (40,000+ runs/month)

💎 **Specific Actions**:
- Post adaptive card in a chat or channel (Teams)
- Update a message (Teams)
- Get items (SharePoint)
- Create item (SharePoint)
- Execute SQL query
- Call custom connector

### Feature Comparison for Adaptive Cards

| Task | Free Tier | Premium |
|------|-----------|---------|
| Send card to  Teams | ✅ HTTP to webhook | ✅ Teams connector (better) |
| Schedule card delivery | ✅ Recurrence | ✅ Recurrence |
| Receive form data | ✅ HTTP trigger | ✅ HTTP trigger |
| Built-in approvals | ✅ Yes | ✅ Yes |
| Email notifications | ✅ Yes | ✅ Yes |
| **Update existing card** | ❌ Not possible | ✅ Update message action |
| **Store in SharePoint** | ❌ No | ✅ SharePoint connector |
| **Store in SQL** | ❌ No | ✅ SQL connector |
| **Get SharePoint data** | ❌ No | ✅ Get items action |
| Post as Flow bot | ❌ No | ✅ Teams connector |

### Free Tier Workarounds

**Challenge**: Can't update cards  
**Workaround**: Post new card with updated status, include reference ID

**Challenge**: Can't use SharePoint  
**Workaround**: Store data in Excel on OneDrive (limited free use) or send to email

**Challenge**: Can't use Teams native connector  
**Workaround**: Use HTTP action with webhook URL (works great for sending)

### Licensing Options

1. **Power Automate per user plan**: $15/user/month
   - Unlimited premium flows
   - 40,000 daily API calls
   - All premium connectors

2. **Power Automate per flow plan**: $100/flow/month
   - For automation serving many users
   - Unlimited runs for that flow

3. **Microsoft 365 (E3/E5)**: Includes free tier Power Automate

4. **Power Automate Premium trial**: 90 days free
   - Full premium features
   - Great for learning/testing

---

## Best Practices

### Design Principles

**1. Single Responsibility**
- Each flow should do one thing well
- Complex workflows: Break into child flows
- Easier to test, debug, maintain

**2. Fail Fast**
- Validate inputs at the start
- Check required fields before processing
- Return clear error messages

**3. Idempotency**
- Flow should produce same result if run multiple times
- Use unique IDs to prevent duplicates
- Check if already processed before taking action

**4. Observability**
- Log important steps
- Include flow run ID in notifications
- Store audit trail in SharePoint

### Naming Conventions

**Flow Names**:
```
[Category] - [Action] - [Target]
"HR - Send Welcome Card - New Employees"
"Finance - Process Approval - Expense Requests"
```

**Action Names**:
```
Use verbs: "Get user details", "Send notification", "Update status"
NOT: "HTTP", "Compose 1", "Action 2"
```

**Variables**:
```
PascalCase or camelCase
"MessageId", "ApprovalStatus", "cardElements"
Include type in name if helpful: "teamsList", "isApproved", "retryCount"
```

### Performance Optimization

**1. Minimize HTTP Calls**
- Cache data in variables
- Batch operations where possible
- Use "Select" to filter before "Apply to each"

**2. Use Parallel Branches Wisely**
- Run independent actions in parallel
- Don't exceed 5-10 parallel branches
- Monitor for throttling

**3. Limit Loop Iterations**
- Add safeguards: "Do until" with iteration limit
- Process in batches if > 100 items
- Use SharePoint views to pre-filter data

**4. Optimize Apply to Each**
- Use "Select" action to transform data before looping
- Enable "Concurrency Control" for parallel processing (up to 50)
- Avoid nested loops when possible

### Security Best Practices

**1. Webhook URLs**
```
❌ Don't: Hardcode in flow
✅ Do: Store in environment variables
✅ Do: Rotate periodically
```

**2. Sensitive Data**
```
❌ Don't: Store passwords/keys in variables
✅ Do: Use Azure Key Vault
✅ Do: Use secure inputs/outputs
```

**3. Connections**
```
❌ Don't: Share connection credentials
✅ Do: Each user creates own connections
✅ Do: Use service accounts for shared flows
```

**4. Permissions**
```
❌ Don't: Give flow owner access to everything
✅ Do: Principle of least privilege
✅ Do: Audit flow owners regularly
```

### Testing Strategy

**Development**:
```
1. Create test Teams channel
2. Use manual trigger for testing
3. Test with sample data first
4. Verify all branches (success, fail, edge cases)
```

**User Acceptance Testing**:
```
1. Deploy to test environment
2. Real users test with real scenarios
3. Collect feedback
4. Iterate
```

**Production**:
```
1. Export as solution
2. Import to production environment
3. Update environment variables (webhook URLs)
4. Monitor first runs closely
5. Have rollback plan ready
```

### Documentation

**Flow Description**:
```
Purpose: Send daily standup reminder card to team
Trigger: Recurrence (weekdays at 9 AM)
Actions: HTTP POST to Teams webhook
Owner: IT Team
Last Modified: 2026-04-14
```

**Comments in Flow**:
- Use "Add a note" on complex actions
- Document regex patterns
- Explain business logic in conditions

**External Docs**:
- SharePoint page with flow catalog
- Include: Purpose, owner, schedule, dependencies
- Link to related lists/systems

---

## Troubleshooting

Common issues and solutions.

### Card Doesn't Appear in Teams

**Issue**: HTTP action succeeds but no card in channel

**Diagnostics**:
```
1. Check HTTP status code: 200 = success
2. Verify webhook URL is correct
3. Check JSON is valid (use JSONLint.com)
4. Ensure proper Teams message envelope
5. Check webhook hasn't been deleted in Teams
```

**Solution**:
```
✅ Verify contentType: "application/vnd.microsoft.card.adaptive"
✅ Ensure card has valid schema: "$schema", "type", "version"
✅ Test card in Designer first
✅ Check Teams webhook settings (might be disabled)
```

### Expression Errors

**Issue**: `InvalidTemplate` error in flow

**Common Causes**:
```
❌ Missing closing brace: @{utcNow(
❌ Wrong quotes: @{triggerBody()['field']} (should use ?)
❌ Typo in function name: @{UtcNow()} (case sensitive)
❌ Invalid format string: @{formatDateTime(utcNow(), 'wrong')}
```

**Solution**:
```
✅ Use Expression editor (validates syntax)
✅ Test in Compose action first
✅ Check function documentation
✅ Use ? for optional fields: @{body('Action')?['field']}
```

### Approval Not Routing

**Issue**: Approval email not sent to approver

**Diagnostics**:
```
1. Check "Assigned to" email is valid Azure AD email
2. Verify approver has M365 license
3. Check spam folder
4. Review flow run history for errors
```

**Solution**:
```
✅ Use email from Azure AD (not alias)
✅ Ensure approver can access Power Automate
✅ Check Approvals app in Teams (backup access)
✅ Set timeout: 7-30 days (default: 30 days)
```

### HTTP Trigger Not Receiving Data

**Issue**: Flow doesn't trigger when Action.Submit clicked

**Reason**: Action.Submit requires Teams app configuration (advanced)

**Simpler Alternative**:
```
💎 Use "Post your own adaptive card as the Flow bot" (Premium)
  - Built-in Action.Submit handling
  - No webhook configuration needed
  - Returns submitted data directly
```

**Free Tier Workaround**:
```
1. Don't use Action.Submit for forms
2. Use Action.OpenUrl to link to Microsoft Forms
3. Forms submission triggers flow via Forms connector
```

### SharePoint Trigger Not Firing

**Issue**: Flow doesn't run when item created

**Diagnostics**:
```
1. Check trigger settings (correct site/list)
2. Verify flow is turned on
3. Check if exceeded run quota
4. Look for errors in run history
```

**Solution**:
```
✅ Re-save trigger configuration
✅ Check SharePoint permissions
✅ Verify list name hasn't changed
✅ Try "Get items" action to test connectivity
```

### Card Update Fails

**Issue**: Update a message action fails with 404

**Cause**: Message ID invalid or card deleted

**Solution**:
```
✅ Verify message ID saved correctly
✅ Check card hasn't been manually deleted
✅ Ensure using same channel as original post
✅ Use try-catch: If update fails, post new card instead
```

### Timeout Errors

**Issue**: Flow times out after 5 minutes

**Free Tier**: 5-minute limit per run

**Solutions**:
```
✅ Optimize loops (reduce iterations)
✅ Move long processes to child flows
✅ Process in batches (trigger child flow per batch)
💎 Upgrade to Premium (24+ hour limit)
```

### Throttling Errors

**Issue**: `TooManyRequests` error from connector

**Causes**:
```
SharePoint: 600 requests per minute per connection
Teams: 4 messages per second per webhook
SQL: Varies by plan
```

**Solutions**:
```
✅ Add "Delay" between iterations (1-2 seconds)
✅ Batch operations
✅ Use "Concurrency Control" to limit parallel calls
✅ Implement exponential backoff retry
```

---

## Production Deployment

Best practices for deploying flows to production.

### Environment Strategy

**Recommended Setup**:
```
1. Development Environment
   - Individual testing
   - Rapid iteration
   - Use test webhook URLs

2. UAT Environment
   - User acceptance testing
   - Real-world scenarios
   - Test data only

3. Production Environment
   - Live workflows
   - Production webhook URLs
   - Change control required
```

### Solution Packaging

**Export Flow as Solution**:
```
1. Create solution in Power Automate
2. Add flow to solution
3. Add environment variables for webhooks
4. Export as .zip file
5. Store in version control (Git)
```

**Import to Production**:
```
1. Import solution .zip
2. Configure environment variables
3. Fix connection references
4. Test with manual trigger
5. Enable automatic triggers
```

### Environment Variables

**Create Variables**:
```
Name: TeamsWebhookURL_Production
Type: Text
Current Value: https://outlook.office.com/webhook/...
```

**Use in Flow**:
```
@{parameters('TeamsWebhookURL_Production')}
```

**Benefits**:
- Single place to update URLs
- Different values per environment
- No need to edit flow when promoting

### Connection Management

**Service Accounts**:
```
✅ Create service account: flowbot@company.com
✅ Grant minimum permissions needed
✅ Use for shared flows
✅ Document in flow description
```

**Personal Connections**:
```
❌ Don't use for production
⚠️ Flow breaks when user leaves
⚠️ Auditing shows wrong user
```

### Monitoring

**Set Up Alerts**:
```
Create monitor flow:
  Trigger: Recurrence (every 15 minutes)
  Get failed flow runs (last 15 min)
  If any: Send alert to Teams channel
```

**Analytics Dashboard**:
```
https://make.powerautomate.com
→ Solutions → Your solution
→ Analytics
View: Run success rate, avg duration, errors
```

**Log Important Events**:
```
Action: Create item (SharePoint "Flow Logs")
  Timestamp: @{utcNow()}
  FlowName: @{workflow()?['name']}
  RunId: @{workflow()?['run']?['name']}
  Status: Success
  Details: Processed @{variables('ItemCount')} items
```

### Change Management

**Before Deployment**:
- [ ] Test thoroughly in UAT
- [ ] Document changes
- [ ] Get stakeholder approval
- [ ] Schedule deployment window
- [ ] Prepare rollback plan
- [ ] Notify users

**During Deployment**:
- [ ] Import solution
- [ ] Update environment variables
- [ ] Test with manual trigger
- [ ] Monitor first automatic runs
- [ ] Verify connections
- [ ] Check error logs

**After Deployment**:
- [ ] Verify flows running successfully
- [ ] Monitor for 24-48 hours
- [ ] Collect user feedback
- [ ] Update documentation
- [ ] Archive previous version

### Backup & Disaster Recovery

**Regular Backups**:
```
Weekly: Export all flows as solutions
Store: Version control (Git) + SharePoint
Retention: Last 6 months minimum
```

**Recovery Plan**:
```
If flow deleted/corrupted:
  1. Import last known good version
  2. Reconfigure connections
  3. Update environment variables
  4. Test
  5. Re-enable
```

---

## Additional Resources

### Official Microsoft Documentation

- **Power Automate Docs**: https://learn.microsoft.com/power-automate/
- **Adaptive Cards Docs**: https://learn.microsoft.com/adaptive-cards/
- **Teams Platform**: https://learn.microsoft.com/microsoftteams/platform/
- **Expression Reference**: https://learn.microsoft.com/power-automate/use-expressions-in-conditions
- **Connector Reference**: https://learn.microsoft.com/connectors/

### Tools & Designers

- **Adaptive Cards Designer**: https://adaptivecards.io/designer
- **Power Automate Portal**: https://make.powerautomate.com
- **Schema Explorer**: https://adaptivecards.io/explorer/
- **Samples**: https://adaptivecards.io/samples/

### Community & Support

- **Power Automate Community**: https://powerusers.microsoft.com/t5/Power-Automate-Community/ct-p/MPACommunity
- **Adaptive Cards GitHub**: https://github.com/microsoft/AdaptiveCards
- **Microsoft Tech Community**: https://techcommunity.microsoft.com/
- **Stack Overflow**: Tag `powerautomate`, `adaptive-cards`

### Learning Paths

**Microsoft Learn**:
- Power Automate fundamentals
- Build approval workflows
- Create flows for Teams
- Advanced expression techniques

**YouTube Channels**:
- Microsoft Power Platform
- April Dunnam (Power Automate expert)
- Shane Young (SharePoint + Power Automate)

### Repository Examples

**This Repository**:
- [Power Automate README](power-automate/README.md) - Overview and quick start
- [Basic Flows](power-automate/basic/) - 3 beginner flows (free tier)
- [Intermediate Flows](power-automate/intermediate/) - Approvals, surveys, tasks
- [Advanced Flows](power-automate/advanced/) - Enterprise patterns
- [Cheat Sheet](CHEAT_SHEET.md) - Quick reference

### Next Steps

1. **Complete Basic Flows**: Start with [power-automate/basic/](power-automate/basic/)
2. **Build Your First Approval**: Try [intermediate approval workflow](power-automate/intermediate/)
3. **Explore Premium Features**: Start 90-day trial if not already licensed
4. **Join Community**: Share your flows, get help from experts
5. **Read Best Practices**: Review this guide's best practices section
6. **Deploy to Production**: Follow production deployment checklist

---

## Conclusion

You now have a comprehensive understanding of Power Automate + Adaptive Cards integration! 

**Key Takeaways**:

✅ Power Automate provides **no-code backend** for Adaptive Cards  
✅ **Free tier** enables scheduled cards, HTTP triggers, approvals, email  
✅ **Premium tier** adds Teams native connector, SharePoint, SQL, card updates  
✅ **Expressions** make cards dynamic with dates, conditionals, formatting  
✅ **Error handling** ensures reliable production workflows  
✅ **Best practices** lead to maintainable, scalable automation  

**Your Journey**:
1. Started understanding "why Power Automate"
2. Learned architecture, triggers, actions
3. Built your first flow
4. Mastered dynamic content and expressions
5. Handled forms, approvals, advanced patterns
6. Prepared for production deployment

**Keep Learning**:
- Experiment with the [example flows](power-automate/)
- Join the community and share your workflows
- Start small, iterate, and scale
- Document and share your learnings

🚀 **Now go build amazing automated workflows!**

---

*Last updated: April 14, 2026*  
*Questions or feedback? Open an issue on this repository!*
