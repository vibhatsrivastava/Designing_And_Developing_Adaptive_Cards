# ⭐⭐⭐ Intermediate Power Automate Flows

Build interactive workflows with approvals, dynamic data, and enhanced Teams integration. These flows combine **free-tier** connectors with **💎 Premium** features (clearly marked).

## 📋 Prerequisites

- ✅ Completed [Basic Flows](../basic/) - Understanding of HTTP actions, triggers, JSON
- ✅ Microsoft 365 account with Power Automate access
- ✅ Microsoft Teams access
- 💎 **Optional**: Power Automate Premium license (for SharePoint, card updates
)
- 💎 **Optional**: SharePoint Online access (for some examples)

## 🎯 What You'll Learn

- Build approval workflows with routing logic
- Generate cards dynamically from data sources
- Update existing cards in Teams channels
- Store form submissions in SharePoint/Excel
- Use conditional logic for complex scenarios
- Handle multiple input types and validation
- Create interactive task management systems

## 📚 Flow Examples

---

### 1. Approval Workflow 👔

**Difficulty**: ⭐⭐⭐ Moderate  
**Triggers**: Manual or HTTP Request  
**Actions**: HTTP, Approval (built-in), Send email, Condition, SharePoint (Premium)  
**Use Cases**: Expense approvals, leave requests, purchase orders, document approvals

**What it does**: Sends an approval request card to Teams, captures the approve/reject decision, notifies the requester, and optionally logs the decision to SharePoint.

#### Flow Steps (Free Tier Version)

1. **Trigger**: Manually trigger a flow (with input parameters)
2. **Action**: HTTP - POST approval request card to Teams
3. **Action**: Start and wait for an approval
4. **Condition**: Check if approved or rejected
5. **Action** (if approved): Send approval email + post success card
6. **Action** (if rejected): Send rejection email + post rejection card

#### Complete Flow Configuration

**Step 1: Manual Trigger with Inputs**

Add these inputs:
- **Request Title** (Text) - e.g., "New Laptop Purchase"
- **Request Amount** (Text) - e.g., "$1,500"
- **Requester Email** (Text) - e.g., "john@company.com"
- **Requester Name** (Text) - e.g., "John Doe"
- **Description** (Text) - e.g., "MacBook Pro for development"

**Step 2: HTTP - Post Approval Request Card**

- Method: `POST`
- URI: `YOUR_TEAMS_WEBHOOK_URL`
- Headers: `Content-Type: application/json`
- Body:
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
            "style": "warning",
            "items": [
              {
                "type": "TextBlock",
                "text": "⏳ Approval Request",
                "size": "large",
                "weight": "bolder"
              }
            ]
          },
          {
            "type": "TextBlock",
            "text": "A new approval request requires your attention:",
            "wrap": true,
            "spacing": "medium"
          },
          {
            "type": "FactSet",
            "facts": [
              {
                "title": "Request:",
                "value": "@{triggerBody()?['text']}"
              },
              {
                "title": "Amount:",
                "value": "@{triggerBody()?['text_1']}"
              },
              {
                "title": "Requested by:",
                "value": "@{triggerBody()?['text_3']}"
              },
              {
                "title": "Date:",
                "value": "@{formatDateTime(utcNow(), 'yyyy-MM-dd HH:mm')}"
              }
            ]
          },
          {
            "type": "TextBlock",
            "text": "**Description:**",
            "weight": "bolder",
            "spacing": "medium"
          },
          {
            "type": "TextBlock",
            "text": "@{triggerBody()?['text_4']}",
            "wrap": true,
            "spacing": "small"
          },
          {
            "type": "TextBlock",
            "text": "Please review and approve/reject in Power Automate.",
            "isSubtle": true,
            "spacing": "medium",
            "size": "small"
          }
        ]
      }
    }
  ]
}
```

**Step 3: Start and wait for an approval**

- Approval type: `Approve/Reject - First to respond`
- Title: `@{triggerBody()?['text']} - @{triggerBody()?['text_1']}`
- Assigned to: Enter approver's email (or use dynamic content)
- Details:
  ```
  Request: @{triggerBody()?['text']}
  Amount: @{triggerBody()?['text_1']}
  Requested by: @{triggerBody()?['text_3']}
  Description: @{triggerBody()?['text_4']}
  
  Please review and approve or reject this request.
  ```

**Step 4: Condition - Check Approval Response**

- Condition: `@{outputs('Start_and_wait_for_an_approval')?['body/outcome']}` equals `Approve`

**Step 5a: If Approved - Send Email**

- To: `@{triggerBody()?['text_2']}` (requester email)
- Subject: `✅ Approved: @{triggerBody()?['text']}`
- Body:
  ```
  Good news! Your request has been approved.
  
  Request: @{triggerBody()?['text']}
  Amount: @{triggerBody()?['text_1']}
  Approved by: @{outputs('Start_and_wait_for_an_approval')?['body/responder/displayName']}
  Approved at: @{outputs('Start_and_wait_for_an_approval')?['body/responseTime']}
  Comments: @{outputs('Start_and_wait_for_an_approval')?['body/comments']}
  ```

**Step 5b: If Approved - Post Success Card to Teams**

- Method: `POST`
- URI: `YOUR_TEAMS_WEBHOOK_URL`
- Body: (see approval success card JSON below)

**Step 6a: If Rejected - Send Email**

- To: `@{triggerBody()?['text_2']}` (requester email)
- Subject: `❌ Rejected: @{triggerBody()?['text']}`
- Body:
  ```
  Your request has been rejected.
  
  Request: @{triggerBody()?['text']}
  Amount: @{triggerBody()?['text_1']}
  Rejected by: @{outputs('Start_and_wait_for_an_approval')?['body/responder/displayName']}
  Rejected at: @{outputs('Start_and_wait_for_an_approval')?['body/responseTime']}
  Reason: @{outputs('Start_and_wait_for_an_approval')?['body/comments']}
  ```

**Step 6b: If Rejected - Post Rejection Card to Teams**

- Method: `POST`
- URI: `YOUR_TEAMS_WEBHOOK_URL`
- Body: (see rejection card JSON below)

#### Approval Success Card JSON

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
            "style": "good",
            "items": [
              {
                "type": "ColumnSet",
                "columns": [
                  {
                    "type": "Column",
                    "width": "auto",
                    "items": [
                      {
                        "type": "TextBlock",
                        "text": "✅",
                        "size": "extraLarge"
                      }
                    ]
                  },
                  {
                    "type": "Column",
                    "width": "stretch",
                    "items": [
                      {
                        "type": "TextBlock",
                        "text": "Request Approved",
                        "size": "large",
                        "weight": "bolder"
                      }
                    ]
                  }
                ]
              }
            ]
          },
          {
            "type": "FactSet",
            "facts": [
              {
                "title": "Request:",
                "value": "@{triggerBody()?['text']}"
              },
              {
                "title": "Amount:",
                "value": "@{triggerBody()?['text_1']}"
              },
              {
                "title": "Approved by:",
                "value": "@{outputs('Start_and_wait_for_an_approval')?['body/responder/displayName']}"
              },
              {
                "title": "Approved at:",
                "value": "@{formatDateTime(outputs('Start_and_wait_for_an_approval')?['body/responseTime'], 'yyyy-MM-dd HH:mm')}"
              }
            ]
          },
          {
            "type": "TextBlock",
            "text": "**Comments:**",
            "weight": "bolder",
            "spacing": "medium"
          },
          {
            "type": "TextBlock",
            "text": "@{outputs('Start_and_wait_for_an_approval')?['body/comments']}",
            "wrap": true
          }
        ]
      }
    }
  ]
}
```

#### 💎 Premium Enhancement: Log to SharePoint

Add after the Condition action (runs for both approve/reject):

**Action**: Create item (SharePoint)
- Site Address: Your SharePoint site
- List Name: "Approval Requests" (create this list first)
- Fields:
  - Title: `@{triggerBody()?['text']}`
  - Amount: `@{triggerBody()?['text_1']}`
  - Requester: `@{triggerBody()?['text_3']}`
  - Status: `@{outputs('Start_and_wait_for_an_approval')?['body/outcome']}`
  - Approver: `@{outputs('Start_and_wait_for_an_approval')?['body/responder/displayName']}`
  - Comments: `@{outputs('Start_and_wait_for_an_approval')?['body/comments']}`
  - RequestDate: `@{utcNow()}`

---

### 2. Dynamic Survey Card 📊

**Difficulty**: ⭐⭐⭐ Moderate  
**Triggers**: Scheduled (Recurrence) or Manual  
**Actions**: Get items (SharePoint/Excel), Compose, HTTP, Parse JSON  
**Use Cases**: Polls, surveys, feedback collection, dynamic questionnaires

**What it does**: Pulls survey questions from SharePoint or Excel, dynamically generates a poll/survey card, posts it to Teams, and collects responses.

#### Flow Steps (Free Tier Version - Hardcoded Questions)

1. **Trigger**: Recurrence (weekly) or Manual
2. **Action**: Initialize variable - Questions array
3. **Action**: Compose - Build card JSON dynamically
4. **Action**: HTTP - POST survey card to Teams

#### Complete Flow Configuration

**Step 1: Trigger**

- **Recurrence**: Every Monday at 9 AM (or Manual for testing)

**Step 2: Initialize Variable - Questions**

- Name: `Questions`
- Type: `Array`
- Value:
```json
[
  {
    "id": "q1",
    "title": "How satisfied are you with our service?",
    "choices": ["Very Satisfied", "Satisfied", "Neutral", "Dissatisfied", "Very Dissatisfied"]
  },
  {
    "id": "q2",
    "title": "Would you recommend us to others?",
    "choices": ["Definitely", "Probably", "Maybe", "Probably Not", "Definitely Not"]
  },
  {
    "id": "q3",
    "title": "What can we improve?",
    "isText": true
  }
]
```

**Step 3: Compose - Build Survey Card**

Use this expression to build the card body dynamically:

```json
{
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
          "text": "📊 Weekly Team Survey",
          "size": "large",
          "weight": "bolder",
          "color": "accent"
        }
      ]
    },
    {
      "type": "TextBlock",
      "text": "Please take a moment to complete this week's survey. Your feedback helps us improve!",
      "wrap": true,
      "spacing": "medium"
    },
    {
      "type": "Input.ChoiceSet",
      "id": "q1",
      "label": "How satisfied are you with our service?",
      "style": "compact",
      "choices": [
        { "title": "Very Satisfied", "value": "5" },
        { "title": "Satisfied", "value": "4" },
        { "title": "Neutral", "value": "3" },
        { "title": "Dissatisfied", "value": "2" },
        { "title": "Very Dissatisfied", "value": "1" }
      ],
      "isRequired": true,
      "errorMessage": "Please select an option"
    },
    {
      "type": "Input.ChoiceSet",
      "id": "q2",
      "label": "Would you recommend us to others?",
      "style": "compact",
      "choices": [
        { "title": "Definitely", "value": "5" },
        { "title": "Probably", "value": "4" },
        { "title": "Maybe", "value": "3" },
        { "title": "Probably Not", "value": "2" },
        { "title": "Definitely Not", "value": "1" }
      ],
      "isRequired": true
    },
    {
      "type": "Input.Text",
      "id": "q3",
      "label": "What can we improve?",
      "isMultiline": true,
      "placeholder": "Your suggestions...",
      "maxLength": 500
    },
    {
      "type": "TextBlock",
      "text": "Survey closes: @{formatDateTime(addDays(utcNow(), 7), 'yyyy-MM-dd')}",
      "isSubtle": true,
      "size": "small",
      "spacing": "medium"
    }
  ],
  "actions": [
    {
      "type": "Action.Submit",
      "title": "Submit Survey",
      "data": {
        "action": "submitSurvey",
        "surveyId": "@{utcNow()}"
      }
    }
  ]
}
```

**Step 4: HTTP - POST Survey Card**

- Method: `POST`
- URI: `YOUR_TEAMS_WEBHOOK_URL`
- Headers: `Content-Type: application/json`
- Body:
```json
{
  "type": "message",
  "attachments": [
    {
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": @{outputs('Compose')}
    }
  ]
}
```

#### 💎 Premium Enhancement: Pull Questions from SharePoint

Replace "Initialize Variable" with:

**Action**: Get items (SharePoint)
- Site Address: Your SharePoint site
- List Name: "Survey Questions"
- Filter Query: `IsActive eq true`

Then use "Apply to each" to build choices dynamically:

```
Apply to each: @{body('Get_items')}
  Append to array variable: QuestionChoices
  Value: Build JSON object with question data
```

#### Companion Flow: Survey Response Handler

Create a second flow to handle responses (similar to [Basic Flow #2](../basic/README.md#2-form-submission-handler-)):

1. **Trigger**: When HTTP request is received
2. **Parse JSON**: Extract q1, q2, q3
3. **💎 Create item** (SharePoint): Log response
4. **Send email**: Notify about new response
5. **HTTP**: Post thank-you card to Teams

---

### 3. Task Assignment with Updates 📋

**Difficulty**: ⭐⭐⭐⭐ Moderate-Advanced  
**Triggers**: HTTP Request or SharePoint item created  
**Actions**: HTTP, Parse JSON, Update message (Premium), Condition  
**Use Cases**: Task management, issue tracking, project updates, status reporting

**What it does**: Sends a task assignment card to Teams When a new task is created (manually or from SharePoint). Users can update task status via card buttons. The card updates in-place to show current status.

#### Flow Steps (Free Tier Version)

1. **Trigger**: Manual with inputs (Task Title, Assignee, Due Date, Description)
2. **Action**: HTTP - POST task assignment card to Teams
3. **Action**: Store message ID for future updates (in variable or SharePoint)

#### Complete Flow Configuration

**Step 1: Manual Trigger with Inputs**

Add inputs:
- **Task Title** (Text) - e.g., "Update documentation"
- **Assignee** (Text) - e.g., "Jane Smith"
- **Due Date** (Text) - e.g., "2026-04-20"
- **Priority** (Choice: Low, Medium, High, Critical)
- **Description** (Text) - Task details

**Step 2: Compose - Build Task Card**

```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "Container",
      "style": "@{if(equals(triggerBody()?['choice'], 'Critical'), 'attention', if(equals(triggerBody()?['choice'], 'High'), 'warning', 'default'))}",
      "items": [
        {
          "type": "ColumnSet",
          "columns": [
            {
              "type": "Column",
              "width": "stretch",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "📋 New Task Assigned",
                  "size": "large",
                  "weight": "bolder"
                }
              ]
            },
            {
              "type": "Column",
              "width": "auto",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "@{triggerBody()?['choice']}",
                  "color": "@{if(equals(triggerBody()?['choice'], 'Critical'), 'attention', if(equals(triggerBody()?['choice'], 'High'), 'warning', 'accent'))}",
                  "weight": "bolder"
                }
              ]
            }
          ]
        }
      ]
    },
    {
      "type": "TextBlock",
      "text": "@{triggerBody()?['text']}",
      "size": "large",
      "weight": "bolder",
      "spacing": "medium"
    },
    {
      "type": "FactSet",
      "facts": [
        {
          "title": "Assigned to:",
          "value": "@{triggerBody()?['text_1']}"
        },
        {
          "title": "Due Date:",
          "value": "@{triggerBody()?['text_2']}"
        },
        {
          "title": "Priority:",
          "value": "@{triggerBody()?['choice']}"
        },
        {
          "title": "Status:",
          "value": "⏳ Not Started"
        },
        {
          "title": "Created:",
          "value": "@{formatDateTime(utcNow(), 'yyyy-MM-dd HH:mm')}"
        }
      ]
    },
    {
      "type": "TextBlock",
      "text": "**Description:**",
      "weight": "bolder",
      "spacing": "medium"
    },
    {
      "type": "TextBlock",
      "text": "@{triggerBody()?['text_3']}",
      "wrap": true
    }
  ],
  "actions": [
    {
      "type": "Action.Submit",
      "title": "Start Task",
      "data": {
        "action": "updateStatus",
        "status": "In Progress",
        "taskId": "@{guid()}"
      }
    },
    {
      "type": "Action.OpenUrl",
      "title": "View Details",
      "url": "https://your-task-system.com/tasks"
    }
  ]
}
```

**Step 3: HTTP - POST Task Card**

- Method: `POST`
- URI: `YOUR_TEAMS_WEBHOOK_URL`
- Headers: `Content-Type: application/json`
- Body:
```json
{
  "type": "message",
  "attachments": [
    {
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": @{outputs('Compose')}
    }
  ]
}
```

#### 💎 Premium Enhancement: Update Card on Status Change

This requires **Power Automate Premium** and using the native Teams connector (not webhook):

**Flow 1: Send Task Card**
1. Replace HTTP action with **Post adaptive card and wait for a response**
2. This action waits for Action.Submit and returns the data directly
3. Add condition based on returned status
4. Update card with new status using **Update card in a chat or channel**

**Flow 2: Status Update Handler**
1. Trigger when task status changes in SharePoint
2. Get original message ID from SharePoint
3. Use **Update card in a chat or channel** to refresh the card
4. Update status color/text dynamically

---

## 🎓 Learning Exercises

### Exercise 1: Multi-Stage Approval
Extend the approval workflow to require 2 approvers:
- Manager approval first
- If approved, send to director for final approval
- Only proceed if both approve

<details>
<summary>💡 Hint</summary>

- Add a second "Start and wait for an approval" action
- Use nested conditions: If (Manager Approves) → Then (Send to Director)
- Update cards after each stage
</details>

### Exercise 2: Survey Analytics
Enhance the survey flow to calculate results:
- Store responses in SharePoint/Excel
- Calculate average scores
- Post results card weekly with charts (use image URLs for visualization)

<details>
<summary>💡 Hint</summary>

- Create separate response handler flow
- Use SharePoint "Get items" to fetch all responses
- Use "Select" and "Compose" to calculate averages
- Include results in next week's card
</details>

### Exercise 3: Task Escalation
Add escalation logic to task assignment:
- If task not started in 24 hours, send reminder
- If task not completed by due date, escalate to manager
- Update card style to reflect urgency

<details>
<summary>💡 Hint</summary>

- Use "Delay until" action with due date
- Add parallel branch with timer
- Use "Send reminder" email action
- Change container style to "attention" for overdue
</details>

---

## 📦 Import Ready-Made Flows

Download and import these flows:

- **[intermediate-approval.zip](../flow-exports/intermediate-approval.zip)** - Approval Workflow (free tier version)
- **[intermediate-survey.zip](../flow-exports/intermediate-survey.zip)** - Dynamic Survey Card
- **[intermediate-task.zip](../flow-exports/intermediate-task.zip)** - Task Assignment

**Premium versions** (require Power Automate Premium):
- **[intermediate-approval-premium.zip](../flow-exports/intermediate-approval-premium.zip)** - With SharePoint logging
- **[intermediate-task-premium.zip](../flow-exports/intermediate-task-premium.zip)** - With card updates

---

## 🚀 Next Steps

Mastered intermediate flows? Move to:

1. **[Advanced Flows](../advanced/)** - Multi-level approvals, incident management, system integrations
2. **[Tutorials](../tutorials/)** - Detailed step-by-step guides with screenshots
3. **Connect to existing cards** - Link these flows to [examples/intermediate/](../../examples/intermediate/)

---

## 💡 Tips for Intermediate Flows

**Approvals**:
- Use "Approve/Reject - First to respond" for single approver
- Use "Approve/Reject - Everyone must approve" for committee decisions
- Include timeout for approvals (e.g., auto-reject after 3 days)

**Dynamic Content**:
- Use SharePoint lists for configuration (questions, options, templates)
- Store webhook URLs in environment variables
- Pull user lists from Azure AD for assignee dropdowns

**Error Handling**:
- Add "Configure run after" for critical actions
- Use "Scope" actions to group related steps
- Add "Send email" on failure with error details

**Performance**:
- Batch SharePoint operations when possible
- Use "Do until" loops cautiously (check limit: 60 iterations)
- Consider flow concurrency for high-volume scenarios

---

*Last updated: April 2026*
