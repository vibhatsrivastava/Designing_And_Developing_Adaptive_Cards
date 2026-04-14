# ⭐⭐⭐⭐⭐ Advanced Power Automate Flows

Production-ready enterprise workflows featuring multi-level approvals, dynamic card updates, external system integrations, and complex business logic.  **Most examples require 💎 Premium connectors.**

## 📋 Prerequisites

- ✅ Completed [Basic](../basic/) and [Intermediate](../intermediate/) flows
- ✅ Strong understanding of Power Automate expressions and JSON
- 💎 **Required**: Power Automate Premium license
- 💎 **Required**: SharePoint Online access (recommended for logging)
- 💎 **Optional**: SQL Server connector (for database integration examples)
- 💎 **Optional**: Custom connectors for third-party integrations

## 🎯 What You'll Learn

- Build multi-stage approval workflows with escalation
- Update cards dynamically based on status changes
- Integrate external systems (SQL, webhooks, APIs)
- Create full incident lifecycle management
- Handle complex conditional routing
- Implement error recovery and retry logic
- Design enterprise-scale automation

## ⚠️ Important Notes

These flows are designed for **production** environments:
- ✅ Include comprehensive error handling
- ✅ Log all actions for audit trails
- ✅ Use environment variables for configuration
- ✅ Implement security best practices
- ✅ Support concurrent executions
- ✅ Include monitoring and alerts

## 📚 Flow Examples

---

### 1. Multi-Level Approval Workflow 👔👔👔

**Difficulty**: ⭐⭐⭐⭐⭐ Advanced  
**Premium**: 💎 Yes (SharePoint, Update cards)  
**Triggers**: HTTP Request or SharePoint item created  
**Actions**: Multiple approvals, SharePoint logging, card updates, email notifications  
**Use Cases**: Purchase orders $10K+, contract approvals, capital expenditure requests, budget allocations

**What it does**: Routes approval requests through multiple levels (Requester → Manager → Director → Finance/Executive) with automatic escalation, parallel approvals, and comprehensive audit logging.

#### Architecture

```
Requester → Manager Approval → Director Approval → Finance Approval
    ↓            ↓                    ↓                   ↓
 SharePoint   Update Card        Update Card         Update Card
    Log       + Notify           + Notify            + Notify
                 ↓                    ↓                   ↓
              If Reject          If Reject           If Approve
              → Stop             → Stop              → Complete
```

#### Flow Steps

1. **Trigger**: When item created in SharePoint ("Approval Requests" list)
2. **Get requester details** from SharePoint/Azure AD
3. **POST initial card** to Teams with request details
4. **Level 1**: Manager approval (parallel: timeout after 48h)
5. **Condition**: If rejected → notify + stop; If approved → continue
6. **Update card** to show manager approval status
7. **Level 2**: Director approval (if amount > $5,000)
8. **Condition**: If rejected → notify + stop; If approved → continue
9. **Update card** to show director approval
10. **Level 3**: Finance/CFO approval (if amount > $25,000)
11. **Final update**: Mark as approved, update SharePoint, notify all stakeholders
12. **Error handler**: Catch failures, rollback transactions, notify admin

#### Complete Flow Configuration

**SharePoint List Setup**

Create "Approval Requests" list with columns:
- Title (Single line text)
- Amount (Currency)
- Description (Multiple lines)
- Requester (Person)
- Manager (Person)
- Director (Person)
- FinanceApprover (Person)
- Status (Choice: Pending, Manager Approved, Director Approved, Finance Approved, Rejected)
- ApprovalHistory (Multiple lines - JSON log)
- CardMessageId (Single line - for card updates)

**Step 1: Trigger - When item created**

- Site Address: Your SharePoint site
- List Name: "Approval Requests"

**Step 2: Get requester profile** (optional enhancement)

Use "Get user profile (V2)" to fetch manager details automatically:
- User (UPN): `@{triggerBody()?['Requester']?['Email']}`

**Step 3: Compose - Initial Approval Card**

```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "Container",
      "style": "warning",
      "items": [
        {
          "type": "ColumnSet",
          "columns": [
            {
              "type": "Column",
              "width": "auto",
              "items": [
                {
                  "type": "Image",
                  "url": "https://adaptivecards.io/content/pending.png",
                  "size": "small",
                  "style": "person"
                }
              ]
            },
            {
              "type": "Column",
              "width": "stretch",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "⏳ Approval Request - Level 1/3",
                  "size": "large",
                  "weight": "bolder"
                },
                {
                  "type": "TextBlock",
                  "text": "Awaiting Manager Approval",
                  "isSubtle": true,
                  "spacing": "none"
                }
              ]
            }
          ]
        }
      ]
    },
    {
      "type": "Container",
      "items": [
        {
          "type": "TextBlock",
          "text": "**Request Details**",
          "weight": "bolder",
          "size": "medium",
          "spacing": "medium"
        },
        {
          "type": "FactSet",
          "facts": [
            {
              "title": "Request ID:",
              "value": "REQ-@{triggerBody()?['ID']}"
            },
            {
              "title": "Title:",
              "value": "@{triggerBody()?['Title']}"
            },
            {
              "title": "Amount:",
              "value": "$@{formatNumber(triggerBody()?['Amount'], 'N2')}"
            },
            {
              "title": "Requested by:",
              "value": "@{triggerBody()?['Requester']?['DisplayName']}"
            },
            {
              "title": "Submitted:",
              "value": "@{formatDateTime(triggerBody()?['Created'], 'yyyy-MM-dd HH:mm')}"
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
          "text": "@{triggerBody()?['Description']}",
          "wrap": true
        }
      ]
    },
    {
      "type": "Container",
      "separator": true,
      "spacing": "medium",
      "items": [
        {
          "type": "TextBlock",
          "text": "**Approval Status**",
          "weight": "bolder"
        },
        {
          "type": "ColumnSet",
          "columns": [
            {
              "type": "Column",
              "width": "40px",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "⏳",
                  "size": "large"
                }
              ]
            },
            {
              "type": "Column",
              "width": "stretch",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "Manager: Pending",
                  "weight": "bolder"
                },
                {
                  "type": "TextBlock",
                  "text": "Director: Waiting",
                  "isSubtle": true
                },
                {
                  "type": "TextBlock",
                  "text": "Finance: Waiting",
                  "isSubtle": true
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

**Step 4: Post card using Teams connector** (Premium)

- Post as: Flow bot
- Post in: Channel
- Team: Select your team
- Channel: Select channel
- Adaptive Card: `@{outputs('Compose')}`

**Important**: Save the message ID for later updates:
- Add "Compose" action named "SaveMessageId"
- Value: `@{outputs('Post_adaptive_card_and_wait_for_a_response')?['messageId']}`

**Step 5: Update SharePoint with Message ID**

- Site: Your SharePoint site
- List: "Approval Requests"
- Id: `@{triggerBody()?['ID']}`
- CardMessageId: `@{outputs('SaveMessageId')}`

**Step 6: Parallel Branch - Timeout Handler**

Add parallel branch:
- **Delay until**: `@{addHours(utcNow(), 48)}`
- **Condition**: Check if still pending
- **If yes**: Send escalation email to manager + director

**Step 7: Start approval - Manager**

- Approval type: Approve/Reject - First to respond
- Title: `Purchase Request REQ-@{triggerBody()?['ID']} - $@{formatNumber(triggerBody()?['Amount'], 'N2')}`
- Assigned to: `@{triggerBody()?['Manager']?['Email']}`
- Details:
  ```
  **APPROVAL REQUEST**
  
  Request ID: REQ-@{triggerBody()?['ID']}
  Title: @{triggerBody()?['Title']}
  Amount: $@{formatNumber(triggerBody()?['Amount'], 'N2')}
  Requester: @{triggerBody()?['Requester']?['DisplayName']}
  
  Description:
  @{triggerBody()?['Description']}
  
  This request requires manager approval as Level 1 of multi-stage approval process.
  ```
- Item link: Link to SharePoint item
- Item link description: "View Request Details"

**Step 8: Condition - Manager Decision**

- Condition: `@{outputs('Start_approval_-_Manager')?['body/outcome']}` equals `Approve`

**Step 8a: If Rejected**

- Update SharePoint status: "Rejected"
- Append to ApprovalHistory:
  ```json
  {
    "level": "Manager",
    "approver": "@{outputs('Start_approval_-_Manager')?['body/responder/displayName']}",
    "decision": "Rejected",
    "timestamp": "@{outputs('Start_approval_-_Manager')?['body/responseTime']}",
    "comments": "@{outputs('Start_approval_-_Manager')?['body/comments']}"
  }
  ```
- Update Teams card to show rejection (use "Update a message" action)
- Send email to requester
- **Terminate** flow

**Step 8b: If Approved - Continue to Director**

- Update SharePoint status: "Manager Approved"
- Append to ApprovalHistory (same structure as above)
- Update card to show manager approval

**Step 9: Update Card - Manager Approved**

Use **Update a message** (Teams connector):
- Message: Use message ID from earlier
- Update card: Modified version of card JSON with updated status:
  - Change container style to "default"
  - Update status section:
    ```json
    {
      "type": "TextBlock",
      "text": "Manager: ✅ Approved by @{outputs('Start_approval_-_Manager')?['body/responder/displayName']}",
      "color": "good",
      "weight": "bolder"
    }
    ```
  - Change header to "⏳ Approval Request - Level 2/3"

**Step 10: Condition - Check Amount for Director Approval**

- Condition: `@{float(triggerBody()?['Amount'])}` is greater than `5000`

**Step 10a: If Amount > $5,000 - Director Approval Required**

Repeat approval process (similar to Steps 7-9):
- Start approval for Director
- Check outcome
- Update SharePoint
- Update card

**Step 10b: If Amount ≤ $5,000 - Skip Director, go to Finance check**

**Step 11: Condition - Check Amount for Finance Approval**

- Condition: `@{float(triggerBody()?['Amount'])}` is greater than `25000`

**Step 11a: If Amount > $25,000 - Finance/CFO Approval Required**

Final approval stage:
- Start approval for Finance
- Check outcome
- Update status

**Step 11b: If Amount ≤ $25,000 - Auto-approve**

**Step 12: Final Approval - Update Everything**

- Update SharePoint status: "Fully Approved"
- Update card to green "good" style with success message
- Send congratulatory email to requester
- notify all approvers
- Optionally: Trigger procurement system via HTTP/custom connector

**Step 13: Error Handling (Scope)**

Wrap entire flow in "Scope" action:
- Add second scope that runs on "has failed"
- Log error to SharePoint
- Send alert email to admin
- Optionally: Create incident ticket in IT system

#### Advanced Features

**1. Parallel Approvals** (e.g., both Finance AND Legal must approve):
```
Replace sequential approvals with parallel branches
Use "Condition" to check if BOTH approved
If either rejects, entire request is rejected
```

**2. Delegation** (if manager doesn't respond in 24h, escalate to their manager):
```
Add "Delay for 24 hours" in parallel branch
Check if approval still pending
If yes, reassign approval to manager's manager
```

**3. Dynamic Routing** (different approval chains based on category):
```
Add "Switch" action based on request category
Route to different approval chains
E.g., IT expenses → CIO; HR expenses → CHRO
```

---

### 2. Incident Management Lifecycle 🚨

**Difficulty**: ⭐⭐⭐⭐⭐ Advanced  
**Premium**: 💎 Yes (Update cards, SharePoint/SQL)  
**Triggers**: HTTP webhook, Email, SharePoint item  
**Actions**: Create incident card, update status dynamically, assignment routing, resolution tracking  
**Use Cases**: IT incidents, service desk tickets, critical alerts, P1/P2 issues

**What it does**: Complete incident lifecycle management from creation through investigation, assignment, resolution, and post-mortem—all tracked via a single dynamically-updating Adaptive Card.

#### Incident Lifecycle

```
1. Incident Created → Post card (Status: New)
2. Auto-Assign → Update card (Status: Assigned, show assignee)
3. Acknowledge → Update card (Status: In Progress, add notes)
4. Investigate → Update card (add investigation logs, timeline)
5. Resolve → Update card (Status: Resolved, add resolution)
6. Close → Final update (Status: Closed, add metrics)
```

#### Flow Architecture

**Flow 1: Create Incident**
- Trigger: HTTP request (from monitoring system/manual/email)
- Parse incident data
- Create SharePoint list item
- Post initial incident card to Teams
- Save message ID to SharePoint
- Auto-assign based on severity/category

**Flow 2: Update Incident Status**
- Trigger: When SharePoint item modified
- Get original card message ID
- Build updated card JSON with new status
- Update Teams message
- Send notifications based on status change
- Log timeline event

**Flow 3: Escalation Handler** (runs hourly)
- Get all "Open" incidents older than SLA
- For each: escalate priority, reassign, alert management
- Update cards with escalation notice

#### Detailed Implementation

**(Due to space constraints, key sections are provided)**

**SharePoint List: "Incidents"**

Columns:
- Title, Description, Severity (Critical/High/Medium/Low)
- Status (New/Assigned/In Progress/Resolved/Closed)
- AssignedTo (Person), ReportedBy (Person)
- Category, CreatedDate, ResolvedDate
- SLA (Hours), TimeToResolve (Calculated)
- CardMessageId, Timeline (Multiple lines - JSON array of events)

**Incident Card Template** (updates dynamically):

```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "Container",
      "style": "@{if(equals(body('Get_item')?['Severity'], 'Critical'), 'attention', if(equals(body('Get_item')?['Severity'], 'High'), 'warning', 'default'))}",
      "bleed": true,
      "items": [
        {
          "type": "ColumnSet",
          "columns": [
            {
              "type": "Column",
              "width": "auto",
              "verticalContentAlignment": "center",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "@{if(equals(body('Get_item')?['Status'], 'Closed'), '✅', if(equals(body('Get_item')?['Status'], 'Resolved'), '🎯', if(equals(body('Get_item')?['Status'], 'In Progress'), '⚙️', '🚨')))}",
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
                  "text": "Incident #@{body('Get_item')?['ID']} - @{body('Get_item')?['Severity']}",
                  "size": "large",
                  "weight": "bolder"
                },
                {
                  "type": "TextBlock",
                  "text": "@{body('Get_item')?['Title']}",
                  "size": "medium",
                  "spacing": "none"
                }
              ]
            },
            {
              "type": "Column",
              "width": "auto",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "@{body('Get_item')?['Status']}",
                  "weight": "bolder",
                  "color": "@{if(equals(body('Get_item')?['Status'], 'Closed'), 'good', if(equals(body('Get_item')?['Status'], 'Resolved'), 'good', if(equals(body('Get_item')?['Status'], 'In Progress'), 'warning', 'attention')))}",
                  "horizontalAlignment": "right"
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
          "title": "Reported by:",
          "value": "@{body('Get_item')?['ReportedBy']?['DisplayName']}"
        },
        {
          "title": "Assigned to:",
          "value": "@{if(empty(body('Get_item')?['AssignedTo']), 'Unassigned', body('Get_item')?['AssignedTo']?['DisplayName'])}"
        },
        {
          "title": "Category:",
          "value": "@{body('Get_item')?['Category']}"
        },
        {
          "title": "Created:",
          "value": "@{formatDateTime(body('Get_item')?['Created'], 'yyyy-MM-dd HH:mm')}"
        },
        {
          "title": "SLA:",
          "value": "@{body('Get_item')?['SLA']} hours"
        },
        {
          "title": "Time Elapsed:",
          "value": "@{div(sub(ticks(utcNow()), ticks(body('Get_item')?['Created'])), 36000000000)} hours"
        }
      ],
      "separator": true
    },
    {
      "type": "TextBlock",
      "text": "**Description:**",
      "weight": "bolder",
      "spacing": "medium"
    },
    {
      "type": "TextBlock",
      "text": "@{body('Get_item')?['Description']}",
      "wrap": true
    },
    {
      "type": "TextBlock",
      "text": "**Timeline:**",
      "weight": "bolder",
      "spacing": "medium"
    },
    "... (dynamically generate timeline from JSON array in SharePoint) ..."
  ],
  "actions": [
    {
      "type": "Action.OpenUrl",
      "title": "View in SharePoint",
      "url": "@{body('Get_item')?['{Link}']}"
    }
  ]
}
```

**Key Dynamic Elements**:
1. Color changes based on severity (attention/warning/default)
2. Icon changes based on status (🚨/⚙️/🎯/✅)
3. Facts update as incident progresses
4. Timeline grows with each status change
5. SLA countdown shows urgency

---

### 3. External System Integration Workflow 🔗

**Difficulty**: ⭐⭐⭐⭐⭐ Advanced  
**Premium**: 💎 Yes (SQL, Custom Connectors)  
**Triggers**: SQL table change, External webhook, SharePoint  
**Actions**: Fetch external data, transform to card, handle responses, update source system  
**Use Cases**: CRM updates, database alerts, third-party system notifications, cross-platform workflows

**What it does**: Monitors external systems (SQL database, CRM, custom API) for events, generates Adaptive Cards with relevant data, posts to Teams, captures user responses, and updates the source system.

#### Example Scenario: New CRM Lead Notification

```
1. New lead added to SQL database (CRM system)
2. Flow triggered by SQL insert
3. Fetch lead details + enrichment data
4. Post lead qualification card to Teams (sales channel)
5. Sales rep qualifies/disqualifies via card buttons
6. Flow updates SQL database with decision
7. If qualified: Create opportunity in CRM via API
8. Update card to show outcome
```

#### Flow Steps

1. **Trigger**: When a row is added/modified (SQL connector)
2. **Get additional data** from SQL JOINs or external APIs
3. **Compose**: Build Adaptive Card with lead details
4. **Post card** to Teams with qualification buttons
5. **Wait for response** (Action.Submit)
6. **Parse response** data
7. **Condition**: Check qualification decision
8. **Update SQL** with decision + timestamp
9. **If qualified**: Call CRM API to create opportunity
10. **Update card** to show final status
11. **Log to monitoring** system

#### SQL Connector Setup

**Connection**: 
- SQL Server: `your-server.database.windows.net`
- Database: `CRM_Database`
- Authentication: SQL Server auth or Azure AD

**Trigger**: When an item is created (V2)
- Table: `Leads`
- Interval: 1 minute (or real-time if supported)

**Sample Lead Table Schema**:
```sql
CREATE TABLE Leads (
    LeadID INT PRIMARY KEY IDENTITY,
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    Company NVARCHAR(100),
    Email NVARCHAR(100),
    Phone NVARCHAR(20),
    Source NVARCHAR(50),
    Budget DECIMAL(18,2),
    Industry NVARCHAR(50),
    CreatedDate DATETIME DEFAULT GETDATE(),
    Status NVARCHAR(20) DEFAULT 'New',
    QualifiedBy NVARCHAR(100),
    QualificationDate DATETIME,
    Notes NVARCHAR(MAX)
);
```

#### Lead Qualification Card

*(Displays company info, contact details, budget, source, with Qualify/Disqualify buttons)*

**Actions in card**:
```json
"actions": [
  {
    "type": "Action.Submit",
    "title": "✅ Qualify Lead",
    "style": "positive",
    "data": {
      "action": "qualify",
      "leadId": "@{triggerBody()?['LeadID']}",
      "leadName": "@{triggerBody()?['FirstName']} @{triggerBody()?['LastName']}"
    }
  },
  {
    "type": "Action.Submit",
    "title": "❌ Disqualify",
    "data": {
      "action": "disqualify",
      "leadId": "@{triggerBody()?['LeadID']}"
    }
  },
  {
    "type": "Action.ShowCard",
    "title": "Add Notes",
    "card": {
      "type": "AdaptiveCard",
      "body": [
        {
          "type": "Input.Text",
          "id": "notes",
          "placeholder": "Enter qualification notes...",
          "isMultiline": true
        }
      ],
      "actions": [
        {
          "type": "Action.Submit",
          "title": "Save Notes",
          "data": {
            "action": "saveNotes",
            "leadId": "@{triggerBody()?['LeadID']}"
          }
        }
      ]
    }
  }
]
```

#### Update SQL After Qualification

**Action**: Update a row (V2) - SQL connector
- Table: `Leads`
- Row ID: `@{triggerBody()?['LeadID']}`
- Fields:
  - Status: `@{if(equals(body('Parse_JSON')?['action'], 'qualify'), 'Qualified', 'Disqualified')}`
  - QualifiedBy: `@{outputs('Post_adaptive_card')?['responder/displayName']}`
  - QualificationDate: `@{utcNow()}`
  - Notes: `@{body('Parse_JSON')?['notes']}`

#### Call External CRM API

**Action**: HTTP (only if qualified)
- Method: `POST`
- URI: `https://your-crm-api.com/api/opportunities`
- Headers:
  ```
  Authorization: Bearer @{variables('CRM_API_Token')}
  Content-Type: application/json
  ```
- Body:
  ```json
  {
    "leadId": "@{triggerBody()?['LeadID']}",
    "accountName": "@{triggerBody()?['Company']}",
    "contactEmail": "@{triggerBody()?['Email']}",
    "estimatedValue": @{triggerBody()?['Budget']},
    "source": "@{triggerBody()?['Source']}",
    "industry": "@{triggerBody()?['Industry']}",
    "qualifiedBy": "@{outputs('Post_adaptive_card')?['responder/displayName']}",
    "qualificationDate": "@{utcNow()}"
  }
  ```

---

## 🎓 Advanced Exercises

### Exercise 1: Approval with Time-Based Escalation
Build a multi-level approval where:
- Manager has 24h to respond
- If no response, escalate to Director automatically
- If Director doesn't respond in 12h, escalate to VP
- Track response times in SharePoint

### Exercise 2: Incident SLA Monitoring
Enhance the incident flow:
- Calculate remaining SLA time dynamically
- Change card color as SLA approaches (green → yellow → red)
- Auto-escalate when SLA breached
- Generate weekly SLA compliance report

### Exercise 3: Bi-Directional CRM Sync
Create two-way sync between Teams and external CRM:
- CRM updates trigger Teams cards
- Teams card actions update CRM
- Handle conflicts (simultaneous updates)
- Implement retry logic for failed API calls

---

## 💡 Enterprise Best Practices

### Security
- ✅ Store API keys/secrets in Azure Key Vault
- ✅ Use managed identities for SQL connections
- ✅ Implement OAuth for third-party APIs
- ✅ Validate all user inputs (prevent injection attacks)
- ✅ Log all actions for audit compliance (SOX, GDPR, HIPAA)

### Performance
- ✅ Batch SharePoint operations (update multiple items at once)
- ✅ Use "Do until" loops carefully (max 60 iterations)
- ✅ Implement pagination for large datasets
- ✅ Cache frequently-accessed data (environment variables)
- ✅ Set appropriate timeouts for external APIs (30-120s)

### Reliability
- ✅ Wrap critical sections in "Scope" for error handling
- ✅ Implement retry logic for transient failures (use "Configure run after")
- ✅ Send admin alerts on flow failures
- ✅ Use "Terminate" action to gracefully exit on errors
- ✅ Log errors to monitoring system (Application Insights)

### Maintainability
- ✅ Use descriptive action names ("Send Manager Approval Email" not "Send Email 3")
- ✅ Add comments to complex expressions
- ✅ Create child flows for reusable logic (modular design)
- ✅ Version control: export flows regularly via Solutions
- ✅ Document flow purpose, triggers, dependencies in SharePoint/Wiki

---

## 📦 Import Ready-Made Flows

💎 **Premium Required** for all advanced flows:

- **[advanced-multilevel-approval.zip](../flow-exports/advanced-multilevel-approval.zip)** - Multi-Level Approval
- **[advanced-incident-lifecycle.zip](../flow-exports/advanced-incident-lifecycle.zip)** - Incident Management
- **[advanced-crm-integration.zip](../flow-exports/advanced-crm-integration.zip)** - SQL/CRM Integration

**Import Note**: These flows include environment variables and connections that must be reconfigured:
1. Import solution package
2. Update environment variables (webhook URLs, API endpoints)
3. Configure connections (SQL, SharePoint, Teams)
4. Create required SharePoint lists/SQL tables
5. Test thoroughly before production deployment

---

## 🔧 Troubleshooting Advanced Flows

**Card Update doesn't work**
- Ensure using native Teams connector (not HTTP webhook)
- Verify message ID stored correctly
- Check permissions (Flow bot must have access to channel)
-  Confirm card hasn't been deleted manually

**SQL Connector timeouts**
- Optimize SQL queries (add indexes, use WHERE clauses)
- Reduce data retrieved (SELECT specific columns, not *)
- Increase timeout in HTTP settings (up to 120s)
- Consider async patterns for long operations

**Approval doesn't route correctly**
- Verify email addresses in SharePoint match Azure AD
- Check conditional logic (use "Test" mode to debug)
- Ensure approver has permissions to respond
- Review approval action timeout settings

**External API calls fail**
- Verify API endpoint URL and authentication
- Check request/response formats (JSON schema)
- Review API rate limits (implement throttling if needed)
- Use "Configure run after" for retry logic

---

## 🚀 Production Deployment Checklist

Before deploying advanced flows to production:

- [ ] Tested with representative data volumes
- [ ] Error handling implemented for all critical actions
- [ ] Logging configured for audit trails
- [ ] Environment variables used for configuration
- [ ] Connections use service accounts (not personal)
- [ ] SLA/performance benchmarks validated
- [ ] Documentation created (runbooks, architecture diagrams)
- [ ] Stakeholders trained on usage
- [ ] Monitoring/alerting configured
- [ ] Rollback plan documented
- [ ] Security review completed
- [ ] Change management approval obtained

---

*Last updated: April 2026*
