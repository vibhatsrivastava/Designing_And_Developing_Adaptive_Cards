# 🔄 Power Automate Integration for Adaptive Cards

Complete your Adaptive Cards journey by learning how to build **end-to-end workflows** with Power Automate. Send cards to Teams, handle user interactions, process form submissions, and orchestrate business processes—all without writing custom backend code.

## 🎯 What You'll Learn

- **Send Adaptive Cards** using Power Automate flows
- **Handle form submissions** and user interactions
- **Build approval workflows** with automatic routing
- **Process card data** and integrate with other systems
- **Update cards dynamically** based on user actions
- **Schedule card delivery** for notifications and reminders
- **Connect cards to SharePoint, SQL, and other data sources**

## 🚀 Why Power Automate?

**The Challenge**: The existing [teams-integration](../teams-integration/) examples show how to *send* cards using PowerShell/Python, but what happens when users click buttons or submit forms? You need a backend to:
- Receive form submissions
- Process approval decisions
- Update card content
- Trigger business logic

**The Solution**: Power Automate provides a **no-code/low-code backend** that:
- ✅ Receives Adaptive Card actions via HTTP triggers
- ✅ Processes form data with built-in actions
- ✅ Integrates with 400+ connectors (SharePoint, SQL, Teams, Email, etc.)
- ✅ Updates cards in Teams channels
- ✅ Orchestrates multi-step approval workflows
- ✅ Requires no server infrastructure

## 📋 Prerequisites

Before starting, ensure you have:
- ✅ **Microsoft 365 account** with Power Automate access (included in most M365 plans)
- ✅ **Microsoft Teams** license and access
- ✅ **Teams channel** where you can post cards (or use personal chat for testing)
- ✅ Basic understanding of Adaptive Cards (see [QUICK_START.md](../QUICK_START.md))
- ✅ Familiarity with Teams webhooks (covered in [teams-integration](../teams-integration/))

**Optional for Premium Examples**:
- 💎 **Power Automate Premium** license (for SharePoint, SQL, custom connectors, card updates)
- 💎 **SharePoint Online** access (for some intermediate/advanced flows)

## 📚 Learning Path

### 🌟 Begin Here
Start with the **[5-Minute Quick Start](#quick-start)** below to send your first card via Power Automate.

### 📖 Complete Guide
Read the comprehensive **[Power Automate Guide](../POWER_AUTOMATE_GUIDE.md)** for deep-dive into concepts, architecture, troubleshooting, and best practices.

### 🎨 Example Flows by Difficulty

#### ⭐ Basic Flows (Free Tier)
Perfect for beginners learning Power Automate fundamentals.

1. **[Scheduled Card Sender](basic/)** - Send welcome/notification cards on a schedule
   - Triggers: Recurrence 
   - Actions: Post card to Teams channel
   - Use case: Daily standup reminders, weekly reports

2. **[Form Submission Handler](basic/)** - Receive and process feedback form submissions
   - Triggers: HTTP request (from Action.Submit)
   - Actions: Send email notification with form data
   - Use case: Customer feedback, bug reports, survey responses

3. **[Button Click Acknowledgment](basic/)** - Simple button handler with confirmation
   - Triggers: HTTP request
   - Actions: Post confirmation message to Teams
   - Use case: Task completion, acknowledgments, simple actions

#### ⭐⭐⭐ Intermediate Flows (Free + Premium)
Build interactive workflows with approvals and data storage.

4. **[Approval Workflow](intermediate/)** - Request approval and route responses
   - Triggers: Manual trigger or HTTP request
   - Actions: Send approval card, capture decision, notify requester
   - Use case: Expense approvals, leave requests, purchase orders
   - 💎 **Premium**: Log to SharePoint list

5. **[Dynamic Survey Card](intermediate/)** - Generate cards from data and collect responses
   - Triggers: Scheduled or manual
   - Actions: Read questions from Excel/SharePoint, build card dynamically, store results
   - Use case: Polls, surveys, feedback collection
   - 💎 **Premium**: SharePoint data source

6. **[Task Assignment with Updates](intermediate/)** - Send task cards and handle status updates
   - Triggers: HTTP request or SharePoint item creation
   - Actions: Send task card, receive updates, notify team
   - Use case: Issue tracking, task management, project updates
   - 💎 **Premium**: Update original card in Teams

#### ⭐⭐⭐⭐⭐ Advanced Flows (Premium Features)
Production-ready workflows for enterprise scenarios.

7. **[Multi-Level Approval](advanced/)** - Escalating approval workflow with routing
   - Triggers: HTTP request or form submission
   - Actions: Sequential approvals (manager → director → finance), notifications at each stage
   - Use case: Purchase orders, contract approvals, budget requests
   - 💎 **Premium**: SharePoint logging, card updates

8. **[Incident Management Lifecycle](advanced/)** - Full incident workflow from creation to resolution
   - Triggers: External webhook or manual
   - Actions: Create incident card, assign, update status, send resolution report
   - Use case: IT incidents, service desk, critical alerts
   - 💎 **Premium**: Update cards, SQL logging

9. **[Integration Workflow](advanced/)** - External system triggers card with dynamic data
   - Triggers: SharePoint, SQL, webhook from third-party
   - Actions: Fetch data, generate card, handle response, update source system
   - Use case: CRM updates, database alerts, cross-system workflows
   - 💎 **Premium**: SharePoint/SQL connectors, custom connectors

### 🔗 Connect to Existing Cards
These flows work with the interactive cards in the [examples](../examples/) directory:

- **[Feedback Form Flow](tutorials/feedback-form-flow.md)** - Backend for [01-feedback-form.json](../examples/intermediate/01-feedback-form.json)
- **[Task Assignment Flow](tutorials/task-assignment-flow.md)** - Backend for [02-task-assignment.json](../examples/intermediate/02-task-assignment.json)
- **[Poll Voting Flow](tutorials/poll-voting-flow.md)** - Backend for [03-poll-voting.json](../examples/intermediate/03-poll-voting.json)
- **[Approval Workflow Flow](tutorials/approval-workflow-flow.md)** - Backend for [01-approval-workflow.json](../examples/advanced/01-approval-workflow.json)

## ⚡ Quick Start

Send your first Adaptive Card via Power Automate in 5 minutes!

### Step 1: Get Your Teams Webhook URL
1. Open Microsoft Teams
2. Navigate to the channel where you want to post cards
3. Click **···** (More options) → **Connectors**
4. Search for **Incoming Webhook** → **Configure**
5. Name it (e.g., "Adaptive Card Test") → **Create**
6. **Copy the webhook URL** → **Done**

### Step 2: Create Your Flow
1. Go to [Power Automate](https://make.powerautomate.com)
2. Click **+ Create** → **Instant cloud flow**
3. Name it "My First Adaptive Card" → Choose **Manually trigger a flow** → **Create**

### Step 3: Add the HTTP Action
1. Click **+ New step**
2. Search for **HTTP** → Select **HTTP** (under Premium tab, but free to use)
3. Configure:
   - **Method**: POST
   - **URI**: Paste your webhook URL from Step 1
   - **Headers**: `Content-Type` = `application/json`
   - **Body**: Paste the JSON below

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
            "type": "TextBlock",
            "text": "Hello from Power Automate! 🎉",
            "size": "large",
            "weight": "bolder",
            "color": "accent"
          },
          {
            "type": "TextBlock",
            "text": "You just sent your first Adaptive Card using Power Automate!",
            "wrap": true
          },
          {
            "type": "FactSet",
            "facts": [
              {
                "title": "Sent by:",
                "value": "Power Automate Flow"
              },
              {
                "title": "Difficulty:",
                "value": "⭐ Basic"
              }
            ]
          }
        ],
        "actions": [
          {
            "type": "Action.OpenUrl",
            "title": "Learn More",
            "url": "https://adaptivecards.io"
          }
        ]
      }
    }
  ]
}
```

### Step 4: Test Your Flow
1. Click **Save** (top right)
2. Click **Test** → **Manually** → **Test**
3. Click **Run flow** → **Done**
4. Check your Teams channel—you should see the card! 🎉

### Next Steps
- ✅ **Customize the card** - Edit the JSON body to change text, colors, add images
- ✅ **Add a schedule** - Replace "Manual trigger" with "Recurrence" for daily cards
- ✅ **Handle form submissions** - See [Basic Flow #2](basic/) for HTTP triggers
- ✅ **Read the full guide** - Check out [POWER_AUTOMATE_GUIDE.md](../POWER_AUTOMATE_GUIDE.md)

## 📂 Directory Structure

```
power-automate/
├── README.md                    # This file - overview and navigation
├── basic/                       # Beginner flows (free tier)
│   └── README.md               # Basic flow examples with code
├── intermediate/                # Intermediate flows (free + premium)
│   └── README.md               # Intermediate flow examples
├── advanced/                    # Advanced flows (premium features)
│   └── README.md               # Advanced flow examples
├── tutorials/                   # Step-by-step guides with screenshots
│   ├── feedback-form-flow.md   # Tutorial for feedback form backend
│   ├── task-assignment-flow.md # Tutorial for task assignment backend
│   ├── poll-voting-flow.md     # Tutorial for poll backend
│   └── approval-workflow-flow.md # Tutorial for approval backend
└── flow-exports/                # Importable flow JSON files
    ├── basic-scheduled-card.json
    ├── basic-form-handler.json
    ├── intermediate-approval.json
    └── ... (more flows)
```

## 🔑 Key Concepts

### Power Automate + Adaptive Cards Architecture

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────┐
│  Power Automate │─────▶│  Teams Channel   │      │    User     │
│      Flow       │      │  (Adaptive Card) │◀─────│  Clicks     │
└─────────────────┘      └──────────────────┘      │   Button    │
        ▲                                           └──────┬──────┘
        │                                                  │
        │              ┌──────────────────┐               │
        └──────────────│  HTTP Trigger    │◀──────────────┘
                       │  (Receives Data) │
                       └──────────────────┘
```

**Two-Way Workflow**:
1. **Send Card**: Flow → HTTP action → Teams webhook → Card appears in channel
2. **Receive Action**: User clicks button → HTTP POST to flow's trigger URL → Flow processes data

### Common Triggers
- **Recurrence** - Schedule cards (daily reports, reminders) - ✅ Free
- **Manual** - Run flow on-demand - ✅ Free
- **HTTP Request** - Receive Adaptive Card Action.Submit data - ✅ Free
- **SharePoint** - Item created/modified triggers card - 💎 Premium
- **Email** - Incoming email triggers flow - ✅ Free
- **Teams** - Message posted triggers flow - 💎 Premium

### Common Actions
- **HTTP** - Send card to Teams webhook - ✅ Free
- **Post card to channel** - Direct Teams connector - 💎 Premium (recommended)
- **Send email** - Email notifications - ✅ Free
- **Condition** - Branch logic based on form data - ✅ Free
- **Compose** - Build JSON dynamically - ✅ Free
- **SharePoint** - Create/update list items - 💎 Premium
- **Approval** - Start approval process - ✅ Free
- **Update message** - Update existing card - 💎 Premium

### Handling Action.Submit
When a user clicks an Action.Submit button in your card, the button's data is sent as HTTP POST to a URL you specify. In Power Automate:

1. Create flow with **HTTP Request** trigger
2. Copy the trigger's **HTTP POST URL**
3. Add this URL to your Adaptive Card's submit action metadata (or handle via Teams messaging extension)
4. Parse the JSON body in the flow
5. Process the data (send email, update database, approve/reject, etc.)

**Note**: Direct Action.Submit handling requires additional Teams app configuration. See [POWER_AUTOMATE_GUIDE.md](../POWER_AUTOMATE_GUIDE.md) for details, or use the Teams "Post your own adaptive card as Flow bot" action for simpler integration.

## 💡 Best Practices

### ✅ Do
- **Test in Teams first** - Send cards to yourself before broadcasting
- **Validate JSON** - Use the [Adaptive Cards Designer](https://adaptivecards.io/designer) to verify card structure
- **Handle errors** - Add "Configure run after" for HTTP actions to catch failures
- **Use dynamic content** - Pull data from triggers/previous actions instead of hardcoding
- **Log to SharePoint/Excel** - Track form submissions for audit trail (Premium)
- **Set timeouts** - Configure HTTP action timeout (default 30s, max 120s)
- **Version cards** - Use version 1.5 for latest features (Teams supports 1.4+)
- **Mark Premium flows** - Use 💎 icon so users know what requires premium license

### ❌ Don't
- **Expose webhook URLs** - Keep URLs secret; anyone with URL can post to channel
- **Skip error handling** - Flows fail silently without notifications by default
- **Hardcode sensitive data** - Use environment variables or Key Vault
- **Create deep nesting** - HTTP body has JSON depth limits; keep cards flat where possible
- **Ignore rate limits** - Teams webhooks have rate limits (4 requests/second)
- **Use inline images** - Host images externally; inline data URIs bloat JSON
- **Create without testing** - Always test flows before deploying to production

## 🔗 Related Resources

### Internal Documentation
- **[Adaptive Cards Guide](../ADAPTIVE_CARDS_GUIDE.md)** - Complete guide to building cards
- **[Cheat Sheet](../CHEAT_SHEET.md)** - Quick reference for card elements
- **[Examples](../examples/)** - 9+ working card examples
- **[Teams Integration](../teams-integration/)** - PowerShell/Python integration code
- **[Power Automate Guide](../POWER_AUTOMATE_GUIDE.md)** - Comprehensive flow building guide

### External Resources
- **[Power Automate Documentation](https://learn.microsoft.com/power-automate/)** - Official Microsoft docs
- **[Adaptive Cards Designer](https://adaptivecards.io/designer)** - Visual card builder
- **[Adaptive Cards Schema](https://adaptivecards.io/explorer/)** - Schema explorer
- **[Teams Adaptive Cards](https://learn.microsoft.com/microsoftteams/platform/task-modules-and-cards/cards/cards-reference#adaptive-card)** - Teams-specific guidance
- **[Power Automate Community](https://powerusers.microsoft.com/t5/Power-Automate-Community/ct-p/MPACommunity)** - Forums and support

## ❓ Troubleshooting

### Card doesn't appear in Teams
- ✅ Verify webhook URL is correct (check for extra spaces, line breaks)
- ✅ Ensure JSON is valid (use [JSONLint](https://jsonlint.com/))
- ✅ Check flow run history for HTTP action errors (400/401/500 responses)
- ✅ Verify card is wrapped in Teams message envelope (`type: message`, `attachments` array)
- ✅ Confirm `contentType` is exactly `application/vnd.microsoft.card.adaptive`

### Flow doesn't trigger on button click
- ✅ Verify Action.Submit is configured correctly in card JSON
- ✅ Check if using Teams messaging extension (required for direct Action.Submit)
- ✅ Use "Post your own adaptive card as Flow bot" action for simpler integration
- ✅ Ensure HTTP trigger URL is correctly configured

### Permission errors
- ✅ Verify you have Owner/Member role in Teams channel (not Guest)
- ✅ Check Power Automate license (Premium features require paid license)
- ✅ Ensure connections are authenticated (SharePoint, SQL, etc.)

### Premium connectors not available
- ✅ Verify your M365 license includes Power Automate Premium
- ✅ Some features have trial periods - check subscription status
- ✅ Consider free alternatives (e.g., email instead of SharePoint for logging)

**For more troubleshooting**: See [POWER_AUTOMATE_GUIDE.md - Troubleshooting Section](../POWER_AUTOMATE_GUIDE.md#troubleshooting)

## 📈 What's Next?

After completing this Power Automate integration:

1. **Explore basic flows** - Start with [basic/](basic/) to understand fundamentals
2. **Build interactive forms** - Try [intermediate/](intermediate/) approval and survey flows
3. **Deploy to production** - Review [advanced/](advanced/) for enterprise scenarios
4. **Read the full guide** - Deep-dive into [POWER_AUTOMATE_GUIDE.md](../POWER_AUTOMATE_GUIDE.md)
5. **Combine with PowerShell/Python** - Use [teams-integration](../teams-integration/) for hybrid workflows
6. **Join the community** - Share your flows and learn from others

---

**💬 Questions or feedback?** Open an issue on this repository or contribute your own flow examples!

**🌟 Pro Tip**: Bookmark the [Cheat Sheet](../CHEAT_SHEET.md) and this README for quick reference while building flows.

*Last updated: April 2026*
