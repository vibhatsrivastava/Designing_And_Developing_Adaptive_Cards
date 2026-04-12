# Microsoft Adaptive Cards - A Practical Learning Guide

Welcome to this comprehensive, hands-on guide to learning Microsoft Adaptive Cards! This guide will take you from understanding the basics to building and deploying adaptive cards in Microsoft Teams and other platforms.

## 📚 Table of Contents

1. [What are Adaptive Cards?](#what-are-adaptive-cards)
2. [Why Use Adaptive Cards?](#why-use-adaptive-cards)
3. [Features and Functionalities](#features-and-functionalities)
4. [When to Use Adaptive Cards](#when-to-use-adaptive-cards)
5. [Benefits and Value](#benefits-and-value)
6. [Prerequisites and Environment Setup](#prerequisites-and-environment-setup)
7. [Your First Adaptive Card](#your-first-adaptive-card)
8. [Integration with Microsoft Teams](#integration-with-microsoft-teams)
9. [Sending and Receiving Notifications](#sending-and-receiving-notifications)
10. [Advanced Examples](#advanced-examples)
11. [Best Practices](#best-practices)
12. [Additional Resources](#additional-resources)

---

## What are Adaptive Cards?

Adaptive Cards are platform-agnostic UI snippets authored in JSON that apps and services can exchange. When delivered to a specific app, the JSON is transformed into native UI that automatically adapts to its host.

### Key Characteristics:

- **Platform Agnostic**: Write once, display everywhere
- **JSON-based**: Simple, declarative format
- **Lightweight**: Small payload size
- **Schema-driven**: Strongly typed with validation
- **Responsive**: Automatically adapts to host environment

### Visual Example:
Think of Adaptive Cards as "UI building blocks" that can be used across:
- Microsoft Teams
- Outlook
- Cortana
- Windows Timeline
- Web Chat
- And many more platforms

📖 **Official Documentation**: [What are Adaptive Cards?](https://learn.microsoft.com/en-us/adaptive-cards/)

---

## Why Use Adaptive Cards?

### 1. **Consistency Across Platforms**
Write your card once and it renders appropriately on any supported platform, maintaining brand consistency while respecting each platform's native look and feel.

### 2. **Rich User Experience**
Go beyond plain text messages with:
- Images and media
- Interactive buttons and actions
- Form inputs
- Structured layouts
- Rich formatting

### 3. **Developer Efficiency**
- No need to learn platform-specific UI frameworks
- Single JSON schema for all platforms
- Easy to generate programmatically
- Extensive tooling and designer support

### 4. **Actionable Content**
Users can take actions directly within the card:
- Submit forms
- Open URLs
- Trigger workflows
- Update data

### 5. **Future-Proof**
Microsoft's recommended approach for rich content in Teams, Outlook, and other Microsoft 365 applications.

---

## Features and Functionalities

### Core Features

#### 1. **Elements** (Display Components)
- **TextBlock**: Display text with formatting
- **Image**: Show images
- **Media**: Video and audio
- **RichTextBlock**: Advanced text formatting
- **Container**: Group elements
- **ColumnSet**: Multi-column layouts
- **FactSet**: Display name-value pairs
- **ImageSet**: Gallery of images

#### 2. **Actions** (Interactive Components)
- **Action.OpenUrl**: Open a webpage
- **Action.Submit**: Submit data to your service
- **Action.ShowCard**: Display a child card
- **Action.Execute**: Execute a command (Universal Actions)
- **Action.ToggleVisibility**: Show/hide elements

#### 3. **Inputs** (Form Controls)
- **Input.Text**: Single-line text input
- **Input.Number**: Numeric input
- **Input.Date**: Date picker
- **Input.Time**: Time picker
- **Input.Toggle**: Checkbox/switch
- **Input.ChoiceSet**: Dropdown or radio buttons

#### 4. **Containers and Layout**
- **Container**: Basic grouping
- **ColumnSet & Column**: Grid layouts
- **ActionSet**: Group actions
- **FactSet**: Key-value display
- **ImageSet**: Image galleries
- **Table**: Tabular data (v1.5+)

#### 5. **Styling and Theming**
- Color schemes (default, emphasis, good, warning, attention, accent)
- Font sizes and weights
- Spacing and separator
- Background images
- Adaptive theming (light/dark mode)

### Advanced Features

- **Schema Versioning**: Target specific card versions
- **Fallback Support**: Graceful degradation
- **Refresh Actions**: Auto-update cards
- **Universal Actions**: Modern action model
- **Authentication**: Secure user actions
- **Data Binding**: Template-based cards
- **Accessibility**: Screen reader support

📖 **Feature Documentation**: [Adaptive Cards Features](https://learn.microsoft.com/en-us/adaptive-cards/authoring-cards/getting-started)

---

## When to Use Adaptive Cards

### ✅ Ideal Use Cases

1. **Notifications and Alerts**
   - System notifications
   - Status updates
   - Alert messages with actions

2. **Forms and Data Collection**
   - Surveys and feedback
   - Approval requests
   - Registration forms
   - Quick data entry

3. **Bot Interactions**
   - Welcome messages
   - Help menus
   - Conversation flows
   - Rich responses

4. **Dashboard and Reports**
   - Summary cards
   - KPI displays
   - Quick reports
   - Status dashboards

5. **Workflow Automation**
   - Approval workflows
   - Task assignments
   - Status updates
   - Process automation

6. **Integration Scenarios**
   - Service notifications
   - External system updates
   - Cross-platform messaging
   - Webhook responses

### ❌ When NOT to Use Adaptive Cards

- Complex, multi-page applications
- Highly customized UI requirements
- Real-time, streaming data visualization
- Native platform-specific features required
- Complex navigation flows

---

## Benefits and Value

### For End Users

✨ **Seamless Experience**
- Consistent UI across platforms
- No app switching required
- In-context actions
- Mobile-friendly

🚀 **Increased Productivity**
- Quick actions without context switching
- Forms within messages
- Instant feedback
- Reduced clicks

### For Developers

⚡ **Faster Development**
- JSON-based (easy to generate)
- Designer tool available
- No platform-specific code
- Reusable templates

🔧 **Easy Maintenance**
- Single source of truth
- Version control friendly
- Schema validation
- Extensive documentation

### For Organizations

💼 **Business Value**
- Reduced development costs
- Faster time to market
- Better user adoption
- Improved engagement metrics
- Platform flexibility

📊 **Measurable Impact**
- Higher response rates
- Faster decision-making
- Reduced email volume
- Improved workflow efficiency

---

## Prerequisites and Environment Setup

### Prerequisites

#### Knowledge Requirements
- ✅ Basic understanding of JSON
- ✅ Familiarity with REST APIs (for Teams integration)
- ✅ Basic understanding of webhooks (optional)
- ✅ Microsoft Teams account (for Teams integration)

#### No Programming Experience Required!
While coding helps, you can create adaptive cards using visual designers without writing code.

### Required Software

#### 1. **For Basic Card Design**
- Web browser (Chrome, Edge, Firefox)
- Text editor (VS Code recommended)

#### 2. **For Teams Integration**
- Microsoft Teams (Desktop or Web)
- Microsoft 365 account

#### 3. **For Development (Optional)**
- Node.js (v14 or later)
- Visual Studio Code
- Postman or similar API testing tool
- Git (for version control)

### Setting Up Your Development Environment

#### Step 1: Install Visual Studio Code
```bash
# Download from https://code.visualstudio.com/
# Or use winget on Windows:
winget install -e --id Microsoft.VisualStudioCode
```

#### Step 2: Install VS Code Extensions (Recommended)
- **Adaptive Card Previewer**: Real-time card preview
- **JSON Tools**: Format and validate JSON
- **REST Client**: Test APIs

#### Step 3: Bookmark Essential Tools

1. **Adaptive Cards Designer**
   - URL: https://adaptivecards.microsoft.com/designer
   - Purpose: Visual card design and testing

2. **Schema Explorer**
   - URL: https://adaptivecards.io/explorer/
   - Purpose: Browse all available elements

3. **Sample Cards**
   - URL: https://adaptivecards.io/samples/
   - Purpose: Learn from examples

#### Step 4: Create Your Project Structure
```
adaptive-cards-learning/
├── examples/
│   ├── basic/
│   ├── intermediate/
│   └── advanced/
├── templates/
├── teams-integration/
└── README.md
```

#### Step 5: Install Node.js (For Teams Bot Development)
```bash
# Download from https://nodejs.org/
# Or use winget:
winget install -e --id OpenJS.NodeJS

# Verify installation:
node --version
npm --version
```

### Quick Environment Test

Create a simple test file to ensure everything works:

**test-card.json**
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "TextBlock",
      "text": "Hello, Adaptive Cards! 🎉",
      "size": "Large",
      "weight": "Bolder"
    }
  ]
}
```

Test it at: https://adaptivecards.microsoft.com/designer

---

## Your First Adaptive Card

Let's build your first adaptive card step by step!

### Example 1: Simple Welcome Card

**welcome-card.json**
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "TextBlock",
      "text": "Welcome to Adaptive Cards!",
      "size": "Large",
      "weight": "Bolder",
      "color": "Accent"
    },
    {
      "type": "TextBlock",
      "text": "This is your first adaptive card. Pretty cool, right?",
      "wrap": true,
      "spacing": "Medium"
    },
    {
      "type": "Image",
      "url": "https://adaptivecards.io/content/samples/aclogo.png",
      "size": "Medium",
      "horizontalAlignment": "Center"
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
```

**What's happening here?**
- `$schema`: Defines the card schema version
- `type`: Must be "AdaptiveCard"
- `version`: Adaptive Card version (1.0 to 1.6)
- `body`: Array of elements to display
- `actions`: Array of interactive buttons

**Try it yourself:**
1. Copy the JSON above
2. Go to https://adaptivecards.microsoft.com/designer
3. Paste it in the "Card Payload Editor"
4. See the live preview!

### Example 2: Information Card with Formatting

**status-card.json**
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "ColumnSet",
      "columns": [
        {
          "type": "Column",
          "width": "auto",
          "items": [
            {
              "type": "Image",
              "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Microsoft_logo.svg/512px-Microsoft_logo.svg.png",
              "size": "Small",
              "style": "Person"
            }
          ]
        },
        {
          "type": "Column",
          "width": "stretch",
          "items": [
            {
              "type": "TextBlock",
              "text": "System Status Update",
              "weight": "Bolder",
              "size": "Medium"
            },
            {
              "type": "TextBlock",
              "text": "All systems operational",
              "spacing": "None",
              "color": "Good",
              "isSubtle": true
            }
          ]
        }
      ]
    },
    {
      "type": "FactSet",
      "spacing": "Medium",
      "facts": [
        {
          "title": "Status:",
          "value": "✅ Online"
        },
        {
          "title": "Uptime:",
          "value": "99.9%"
        },
        {
          "title": "Last Check:",
          "value": "2 minutes ago"
        }
      ]
    }
  ]
}
```

**Key Concepts:**
- **ColumnSet**: Creates multi-column layouts
- **FactSet**: Displays name-value pairs
- **Color properties**: Default, Good, Warning, Attention, Accent
- **Size properties**: Small, Medium, Large, ExtraLarge

### Example 3: Interactive Form Card

**feedback-form.json**
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "TextBlock",
      "text": "Feedback Form",
      "size": "Large",
      "weight": "Bolder"
    },
    {
      "type": "TextBlock",
      "text": "We'd love to hear your thoughts!",
      "wrap": true,
      "spacing": "Small"
    },
    {
      "type": "Input.Text",
      "id": "name",
      "placeholder": "Your name",
      "label": "Name"
    },
    {
      "type": "Input.Text",
      "id": "email",
      "placeholder": "your.email@company.com",
      "label": "Email",
      "style": "Email"
    },
    {
      "type": "Input.ChoiceSet",
      "id": "rating",
      "label": "How satisfied are you?",
      "style": "compact",
      "choices": [
        {
          "title": "⭐⭐⭐⭐⭐ Excellent",
          "value": "5"
        },
        {
          "title": "⭐⭐⭐⭐ Good",
          "value": "4"
        },
        {
          "title": "⭐⭐⭐ Average",
          "value": "3"
        },
        {
          "title": "⭐⭐ Poor",
          "value": "2"
        },
        {
          "title": "⭐ Very Poor",
          "value": "1"
        }
      ],
      "value": "5"
    },
    {
      "type": "Input.Text",
      "id": "comments",
      "placeholder": "Share your thoughts...",
      "label": "Comments",
      "isMultiline": true
    }
  ],
  "actions": [
    {
      "type": "Action.Submit",
      "title": "Submit Feedback",
      "style": "positive"
    }
  ]
}
```

**Input Elements Explained:**
- **Input.Text**: Single or multi-line text input
- **Input.ChoiceSet**: Dropdown or radio button selection
- **Action.Submit**: Sends form data to your backend

**Important**: The `id` property is crucial - it's used to identify the data when submitted.

---

## Integration with Microsoft Teams

Now let's integrate adaptive cards into Microsoft Teams!

### Understanding Teams Card Delivery Methods

#### Method 1: Incoming Webhooks (Easiest)
- No coding required
- Simple HTTP POST
- Great for notifications
- Limited interactivity

#### Method 2: Bot Framework
- Full interactivity
- Two-way communication
- User actions handling
- More setup required

#### Method 3: Messaging Extensions
- Search-based cards
- Action-based cards
- User-triggered

### Getting Started: Incoming Webhooks

#### Step 1: Create an Incoming Webhook in Teams

1. Open Microsoft Teams
2. Navigate to the channel where you want to send cards
3. Click the "..." (More options) next to the channel name
4. Select "Connectors" or "Workflows" (depending on Teams version)
5. Search for "Incoming Webhook"
6. Click "Add" or "Configure"
7. Give your webhook a name (e.g., "Adaptive Card Notifications")
8. Optionally upload an icon
9. Click "Create"
10. **Copy the webhook URL** - you'll need this!

#### Step 2: Send Your First Card to Teams

Create a file named `send-to-teams.ps1`:

```powershell
# Your webhook URL from Step 1
$webhookUrl = "YOUR_WEBHOOK_URL_HERE"

# Simple Adaptive Card
$card = @{
    type = "message"
    attachments = @(
        @{
            contentType = "application/vnd.microsoft.card.adaptive"
            content = @{
                type = "AdaptiveCard"
                version = "1.4"
                body = @(
                    @{
                        type = "TextBlock"
                        text = "Hello from Adaptive Cards!"
                        size = "Large"
                        weight = "Bolder"
                    }
                    @{
                        type = "TextBlock"
                        text = "This card was sent via PowerShell to Microsoft Teams!"
                        wrap = $true
                    }
                )
                actions = @(
                    @{
                        type = "Action.OpenUrl"
                        title = "View Documentation"
                        url = "https://learn.microsoft.com/en-us/adaptive-cards/"
                    }
                )
            }
        }
    )
} | ConvertTo-Json -Depth 20

# Send to Teams
Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $card -ContentType "application/json"

Write-Host "Card sent successfully!"
```

**Run it:**
```powershell
.\send-to-teams.ps1
```

#### Alternative: Using PowerShell with JSON File

**teams-notification.json**
```json
{
  "type": "message",
  "attachments": [
    {
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": {
        "type": "AdaptiveCard",
        "version": "1.4",
        "body": [
          {
            "type": "Container",
            "items": [
              {
                "type": "TextBlock",
                "text": "📢 Important Notification",
                "size": "Large",
                "weight": "Bolder",
                "color": "Attention"
              },
              {
                "type": "TextBlock",
                "text": "This is an important system update.",
                "wrap": true,
                "spacing": "Small"
              }
            ]
          },
          {
            "type": "FactSet",
            "spacing": "Medium",
            "facts": [
              {
                "title": "Priority:",
                "value": "High"
              },
              {
                "title": "Sent:",
                "value": "{{DATE(2024-01-15T09:00:00Z, SHORT)}}"
              },
              {
                "title": "From:",
                "value": "System Administrator"
              }
            ]
          }
        ],
        "actions": [
          {
            "type": "Action.OpenUrl",
            "title": "View Details",
            "url": "https://example.com/details"
          },
          {
            "type": "Action.OpenUrl",
            "title": "Acknowledge",
            "url": "https://example.com/acknowledge"
          }
        ]
      }
    }
  ]
}
```

**Send with PowerShell:**
```powershell
$webhookUrl = "YOUR_WEBHOOK_URL_HERE"
$cardJson = Get-Content -Path "teams-notification.json" -Raw
Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $cardJson -ContentType "application/json"
```

#### Using Python

**send_card_teams.py**
```python
import requests
import json

webhook_url = "YOUR_WEBHOOK_URL_HERE"

card = {
    "type": "message",
    "attachments": [
        {
            "contentType": "application/vnd.microsoft.card.adaptive",
            "content": {
                "type": "AdaptiveCard",
                "version": "1.4",
                "body": [
                    {
                        "type": "TextBlock",
                        "text": "Daily Report 📊",
                        "size": "Large",
                        "weight": "Bolder"
                    },
                    {
                        "type": "FactSet",
                        "facts": [
                            {"title": "Tasks Completed:", "value": "15"},
                            {"title": "In Progress:", "value": "8"},
                            {"title": "Pending:", "value": "3"}
                        ]
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "View Dashboard",
                        "url": "https://example.com/dashboard"
                    }
                ]
            }
        }
    ]
}

response = requests.post(webhook_url, json=card)
print(f"Status Code: {response.status_code}")
```

#### Using Node.js

**send-card-teams.js**
```javascript
const axios = require('axios');

const webhookUrl = 'YOUR_WEBHOOK_URL_HERE';

const card = {
  type: 'message',
  attachments: [
    {
      contentType: 'application/vnd.microsoft.card.adaptive',
      content: {
        type: 'AdaptiveCard',
        version: '1.4',
        body: [
          {
            type: 'TextBlock',
            text: 'Build Status Update',
            size: 'Large',
            weight: 'Bolder',
            color: 'Good'
          },
          {
            type: 'TextBlock',
            text: '✅ Build completed successfully!',
            wrap: true
          },
          {
            type: 'FactSet',
            facts: [
              { title: 'Build #:', value: '1234' },
              { title: 'Duration:', value: '3m 42s' },
              { title: 'Branch:', value: 'main' }
            ]
          }
        ],
        actions: [
          {
            type: 'Action.OpenUrl',
            title: 'View Build',
            url: 'https://example.com/build/1234'
          }
        ]
      }
    }
  ]
};

axios.post(webhookUrl, card)
  .then(response => {
    console.log('Card sent successfully!');
  })
  .catch(error => {
    console.error('Error sending card:', error);
  });
```

### Important Notes for Teams Integration

⚠️ **Version Compatibility**: Microsoft Teams supports Adaptive Cards up to version 1.5 (as of 2024)

⚠️ **Webhook Wrapper**: When sending to Teams via webhook, you must wrap the adaptive card in the Teams message format:
```json
{
  "type": "message",
  "attachments": [
    {
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": { /* Your Adaptive Card JSON */ }
    }
  ]
}
```

⚠️ **Action Limitations**: With incoming webhooks, `Action.Submit` doesn't work. Use `Action.OpenUrl` for interactivity.

---

## Sending and Receiving Notifications

### Setting Up Two-Way Communication with Teams Bot

For full interactivity (including form submissions), you need a Teams Bot.

#### Prerequisites for Bot Development

1. **Microsoft Azure Account**: [Sign up here](https://azure.microsoft.com/free/)
2. **Bot Framework SDK**: We'll use JavaScript/TypeScript
3. **Teams Toolkit** (Optional but recommended)

#### Quick Start: Create a Teams Bot with Adaptive Cards

#### Step 1: Install Teams Toolkit for VS Code

1. Open VS Code
2. Go to Extensions (Ctrl+Shift+X)
3. Search for "Teams Toolkit"
4. Install "Teams Toolkit" by Microsoft

#### Step 2: Create a New Bot Project

1. Click the Teams Toolkit icon in VS Code sidebar
2. Click "Create a New App"
3. Select "Bot"
4. Choose "Basic Bot"
5. Select JavaScript or TypeScript
6. Choose your workspace folder
7. Enter an application name

#### Step 3: Customize Bot to Send Adaptive Cards

**Create adaptiveCards/welcomeCard.json:**
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.4",
  "body": [
    {
      "type": "TextBlock",
      "text": "Welcome to Adaptive Cards Bot! 🤖",
      "size": "Large",
      "weight": "Bolder"
    },
    {
      "type": "TextBlock",
      "text": "I can send you interactive cards and process your responses.",
      "wrap": true
    },
    {
      "type": "Input.Text",
      "id": "userName",
      "placeholder": "What's your name?",
      "label": "Name"
    },
    {
      "type": "Input.ChoiceSet",
      "id": "department",
      "label": "Department",
      "choices": [
        { "title": "Engineering", "value": "eng" },
        { "title": "Sales", "value": "sales" },
        { "title": "Marketing", "value": "marketing" },
        { "title": "HR", "value": "hr" }
      ]
    }
  ],
  "actions": [
    {
      "type": "Action.Submit",
      "title": "Submit",
      "data": {
        "actionType": "userInfo"
      }
    }
  ]
}
```

**Update bot.js (JavaScript example):**
```javascript
const { TeamsActivityHandler, CardFactory } = require('botbuilder');
const welcomeCard = require('./adaptiveCards/welcomeCard.json');

class AdaptiveCardBot extends TeamsActivityHandler {
  constructor() {
    super();

    // Send welcome card when user joins
    this.onMembersAdded(async (context, next) => {
      const membersAdded = context.activity.membersAdded;
      for (let member of membersAdded) {
        if (member.id !== context.activity.recipient.id) {
          await context.sendActivity({
            attachments: [CardFactory.adaptiveCard(welcomeCard)]
          });
        }
      }
      await next();
    });

    // Handle messages
    this.onMessage(async (context, next) => {
      const text = context.activity.text?.toLowerCase();
      
      if (text === 'card' || text === 'show card') {
        await context.sendActivity({
          attachments: [CardFactory.adaptiveCard(welcomeCard)]
        });
      } else {
        await context.sendActivity(`You said: ${context.activity.text}`);
      }
      
      await next();
    });

    // Handle adaptive card submit actions
    this.onAdaptiveCardInvoke(async (context, invokeValue) => {
      console.log('Adaptive Card data received:', invokeValue.action.data);
      
      const { userName, department, actionType } = invokeValue.action.data;
      
      if (actionType === 'userInfo') {
        const responseCard = {
          type: 'AdaptiveCard',
          version: '1.4',
          body: [
            {
              type: 'TextBlock',
              text: `Thanks, ${userName}! 🎉`,
              size: 'Large',
              weight: 'Bolder'
            },
            {
              type: 'TextBlock',
              text: `Department: ${department}`,
              wrap: true
            }
          ]
        };
        
        return {
          statusCode: 200,
          type: 'application/vnd.microsoft.card.adaptive',
          value: responseCard
        };
      }
      
      return { statusCode: 200 };
    });
  }
}

module.exports.AdaptiveCardBot = AdaptiveCardBot;
```

#### Handling User Actions

When a user submits an adaptive card form, the data comes through the `onAdaptiveCardInvoke` handler:

```javascript
// Data structure received
{
  action: {
    type: 'Action.Submit',
    data: {
      userName: 'John Doe',
      department: 'eng',
      actionType: 'userInfo'
    }
  }
}
```

**Best Practice**: Always include an `actionType` field in your action data to identify which action was triggered.

---

## Advanced Examples

### Example 1: Approval Workflow Card

**approval-request.json**
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
          "type": "ColumnSet",
          "columns": [
            {
              "type": "Column",
              "width": "auto",
              "items": [
                {
                  "type": "Image",
                  "url": "https://via.placeholder.com/50",
                  "size": "Small",
                  "style": "Person"
                }
              ]
            },
            {
              "type": "Column",
              "width": "stretch",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "Approval Required",
                  "weight": "Bolder",
                  "size": "Medium"
                },
                {
                  "type": "TextBlock",
                  "text": "Purchase Order #PO-12345",
                  "spacing": "None",
                  "isSubtle": true
                }
              ]
            }
          ]
        }
      ],
      "bleed": true
    },
    {
      "type": "Container",
      "spacing": "Medium",
      "items": [
        {
          "type": "FactSet",
          "facts": [
            {
              "title": "Requested by:",
              "value": "John Smith"
            },
            {
              "title": "Department:",
              "value": "Engineering"
            },
            {
              "title": "Amount:",
              "value": "$2,450.00"
            },
            {
              "title": "Date:",
              "value": "{{DATE(2024-01-15T09:00:00Z, SHORT)}}"
            }
          ]
        },
        {
          "type": "TextBlock",
          "text": "Description",
          "weight": "Bolder",
          "spacing": "Medium"
        },
        {
          "type": "TextBlock",
          "text": "Laptop computers for new team members (Qty: 3)",
          "wrap": true,
          "spacing": "Small"
        },
        {
          "type": "Input.Text",
          "id": "approverComments",
          "label": "Comments (optional)",
          "placeholder": "Add your comments here...",
          "isMultiline": true
        }
      ]
    }
  ],
  "actions": [
    {
      "type": "Action.Submit",
      "title": "✅ Approve",
      "style": "positive",
      "data": {
        "actionType": "approval",
        "decision": "approved",
        "poNumber": "PO-12345"
      }
    },
    {
      "type": "Action.Submit",
      "title": "❌ Reject",
      "style": "destructive",
      "data": {
        "actionType": "approval",
        "decision": "rejected",
        "poNumber": "PO-12345"
      }
    },
    {
      "type": "Action.ShowCard",
      "title": "📋 More Details",
      "card": {
        "type": "AdaptiveCard",
        "body": [
          {
            "type": "TextBlock",
            "text": "Additional Information",
            "weight": "Bolder"
          },
          {
            "type": "FactSet",
            "facts": [
              {
                "title": "Vendor:",
                "value": "TechSupply Inc."
              },
              {
                "title": "Delivery:",
                "value": "2-3 business days"
              },
              {
                "title": "Budget Code:",
                "value": "ENG-2024-Q1"
              }
            ]
          }
        ]
      }
    }
  ]
}
```

### Example 2: Dashboard Card with Progress

**project-dashboard.json**
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "TextBlock",
      "text": "Project Dashboard",
      "size": "ExtraLarge",
      "weight": "Bolder"
    },
    {
      "type": "TextBlock",
      "text": "Q1 2024 Progress Report",
      "spacing": "None",
      "isSubtle": true
    },
    {
      "type": "ColumnSet",
      "columns": [
        {
          "type": "Column",
          "width": "stretch",
          "items": [
            {
              "type": "TextBlock",
              "text": "Overall Progress",
              "weight": "Bolder"
            },
            {
              "type": "TextBlock",
              "text": "75% Complete",
              "size": "Large",
              "color": "Good",
              "spacing": "None"
            }
          ]
        },
        {
          "type": "Column",
          "width": "auto",
          "items": [
            {
              "type": "Image",
              "url": "https://via.placeholder.com/80x80/4CAF50/FFFFFF?text=75%",
              "size": "Medium"
            }
          ]
        }
      ],
      "spacing": "Medium"
    },
    {
      "type": "Container",
      "spacing": "Medium",
      "separator": true,
      "items": [
        {
          "type": "TextBlock",
          "text": "Task Breakdown",
          "weight": "Bolder",
          "spacing": "Small"
        },
        {
          "type": "FactSet",
          "facts": [
            {
              "title": "✅ Completed:",
              "value": "45 tasks"
            },
            {
              "title": "🔄 In Progress:",
              "value": "12 tasks"
            },
            {
              "title": "⏸️ Pending:",
              "value": "3 tasks"
            }
          ]
        }
      ]
    },
    {
      "type": "Container",
      "spacing": "Medium",
      "separator": true,
      "items": [
        {
          "type": "TextBlock",
          "text": "Key Milestones",
          "weight": "Bolder"
        },
        {
          "type": "FactSet",
          "spacing": "Small",
          "facts": [
            {
              "title": "Design Phase:",
              "value": "✅ Complete"
            },
            {
              "title": "Development:",
              "value": "🔄 75% Complete"
            },
            {
              "title": "Testing:",
              "value": "📅 Scheduled"
            },
            {
              "title": "Deployment:",
              "value": "⏸️ Not Started"
            }
          ]
        }
      ]
    }
  ],
  "actions": [
    {
      "type": "Action.OpenUrl",
      "title": "View Full Report",
      "url": "https://example.com/project-report"
    },
    {
      "type": "Action.OpenUrl",
      "title": "Team Dashboard",
      "url": "https://example.com/dashboard"
    }
  ]
}
```

### Example 3: Multi-Step Survey Card

**survey-card.json**
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "TextBlock",
      "text": "Employee Satisfaction Survey",
      "size": "Large",
      "weight": "Bolder",
      "color": "Accent"
    },
    {
      "type": "TextBlock",
      "text": "Help us improve your workplace experience",
      "wrap": true,
      "spacing": "None",
      "isSubtle": true
    },
    {
      "type": "Container",
      "spacing": "Medium",
      "separator": true,
      "items": [
        {
          "type": "TextBlock",
          "text": "1. How satisfied are you with your current role?",
          "wrap": true,
          "weight": "Bolder"
        },
        {
          "type": "Input.ChoiceSet",
          "id": "roleSatisfaction",
          "style": "expanded",
          "choices": [
            {
              "title": "😊 Very Satisfied",
              "value": "5"
            },
            {
              "title": "🙂 Satisfied",
              "value": "4"
            },
            {
              "title": "😐 Neutral",
              "value": "3"
            },
            {
              "title": "🙁 Dissatisfied",
              "value": "2"
            },
            {
              "title": "😞 Very Dissatisfied",
              "value": "1"
            }
          ],
          "value": "3"
        }
      ]
    },
    {
      "type": "Container",
      "spacing": "Medium",
      "separator": true,
      "items": [
        {
          "type": "TextBlock",
          "text": "2. How would you rate work-life balance?",
          "wrap": true,
          "weight": "Bolder"
        },
        {
          "type": "Input.ChoiceSet",
          "id": "workLifeBalance",
          "style": "compact",
          "choices": [
            {
              "title": "Excellent",
              "value": "5"
            },
            {
              "title": "Good",
              "value": "4"
            },
            {
              "title": "Fair",
              "value": "3"
            },
            {
              "title": "Poor",
              "value": "2"
            },
            {
              "title": "Very Poor",
              "value": "1"
            }
          ]
        }
      ]
    },
    {
      "type": "Container",
      "spacing": "Medium",
      "separator": true,
      "items": [
        {
          "type": "TextBlock",
          "text": "3. What areas would you like to see improved? (Select all that apply)",
          "wrap": true,
          "weight": "Bolder"
        },
        {
          "type": "Input.ChoiceSet",
          "id": "improvements",
          "isMultiSelect": true,
          "style": "expanded",
          "choices": [
            {
              "title": "Communication",
              "value": "communication"
            },
            {
              "title": "Tools & Technology",
              "value": "tools"
            },
            {
              "title": "Training & Development",
              "value": "training"
            },
            {
              "title": "Team Collaboration",
              "value": "collaboration"
            },
            {
              "title": "Management Support",
              "value": "management"
            }
          ]
        }
      ]
    },
    {
      "type": "Input.Text",
      "id": "additionalComments",
      "label": "Additional Comments",
      "placeholder": "Share any additional thoughts...",
      "isMultiline": true,
      "spacing": "Medium"
    },
    {
      "type": "Input.Toggle",
      "id": "anonymous",
      "title": "Submit anonymously",
      "value": "true",
      "spacing": "Small"
    }
  ],
  "actions": [
    {
      "type": "Action.Submit",
      "title": "Submit Survey",
      "style": "positive",
      "data": {
        "actionType": "survey",
        "surveyId": "employee-satisfaction-2024-q1"
      }
    }
  ]
}
```

---

## Best Practices

### Design Best Practices

#### 1. **Keep It Simple**
- Use clear, concise text
- Avoid cluttered layouts
- Limit the number of actions (3-5 max)
- Use white space effectively

#### 2. **Accessibility**
- Always include `altText` for images
- Use appropriate color contrast
- Provide descriptive labels for inputs
- Test with screen readers

#### 3. **Responsive Design**
- Test on mobile devices
- Use `wrap: true` for text blocks
- Avoid fixed widths when possible
- Consider column collapsing

#### 4. **Visual Hierarchy**
- Use heading sizes appropriately
- Leverage color for emphasis (sparingly)
- Group related information
- Use separators wisely

### Development Best Practices

#### 1. **Schema Versioning**
```json
{
  "type": "AdaptiveCard",
  "version": "1.4",
  "fallbackText": "This card requires a newer version to display properly."
}
```
- Use the lowest version that meets your needs
- Always include `fallbackText`
- Test across different versions

#### 2. **Input Validation**
- Use appropriate input types (`Input.Number`, `Input.Date`)
- Set `isRequired: true` for mandatory fields
- Provide clear placeholder text
- Validate on both client and server

#### 3. **Action Data Structure**
```json
{
  "type": "Action.Submit",
  "title": "Submit",
  "data": {
    "actionType": "formSubmit",
    "formId": "feedback-2024",
    "timestamp": "{{DATE()}}"
  }
}
```
- Always include an `actionType` identifier
- Include metadata for tracking
- Keep data structure consistent

#### 4. **Error Handling**
- Provide graceful fallbacks
- Handle missing data elegantly
- Show user-friendly error messages
- Log errors for debugging

#### 5. **Performance**
- Minimize card payload size
- Optimize images (use appropriate sizes)
- Avoid unnecessary nesting
- Cache frequently used cards

### Security Best Practices

#### 1. **Input Sanitization**
- Never trust user input
- Sanitize all submitted data
- Validate against expected formats
- Protect against injection attacks

#### 2. **URL Validation**
- Whitelist allowed domains
- Use HTTPS for all URLs
- Validate URLs before opening
- Avoid user-generated URLs in actions

#### 3. **Sensitive Data**
- Don't include secrets in cards
- Don't expose internal IDs unnecessarily
- Use authentication for sensitive actions
- Implement proper authorization

### Testing Best Practices

#### 1. **Cross-Platform Testing**
- Test on Teams (web, desktop, mobile)
- Test on Outlook
- Use the Designer for quick testing
- Verify on different screen sizes

#### 2. **Version Testing**
- Test with different schema versions
- Verify fallback behavior
- Check feature availability

#### 3. **Data Testing**
- Test with empty data
- Test with maximum data
- Test special characters
- Test internationalization

---

## Additional Resources

### Official Documentation
- 📘 [Adaptive Cards Documentation](https://learn.microsoft.com/en-us/adaptive-cards/)
- 🎨 [Adaptive Cards Designer](https://adaptivecards.microsoft.com/designer)
- 📚 [Schema Reference](https://adaptivecards.io/explorer/)
- 💡 [Sample Cards](https://adaptivecards.io/samples/)

### Tools
- **Designer**: https://adaptivecards.microsoft.com/designer
- **Schema Explorer**: https://adaptivecards.io/explorer/
- **Visualizer**: https://adaptivecards.io/visualizer/
- **Template Service**: https://docs.microsoft.com/adaptive-cards/templating/

### SDKs and Libraries
- **.NET SDK**: https://docs.microsoft.com/adaptive-cards/sdk/rendering-cards/net/getting-started
- **JavaScript SDK**: https://docs.microsoft.com/adaptive-cards/sdk/rendering-cards/javascript/getting-started
- **iOS SDK**: https://docs.microsoft.com/adaptive-cards/sdk/rendering-cards/ios/getting-started
- **Android SDK**: https://docs.microsoft.com/adaptive-cards/sdk/rendering-cards/android/getting-started

### Community
- **GitHub Repository**: https://github.com/microsoft/AdaptiveCards
- **Stack Overflow**: Tag `adaptive-cards`
- **Microsoft Tech Community**: https://techcommunity.microsoft.com/

### Video Tutorials
- [Adaptive Cards Overview (Microsoft)](https://www.youtube.com/results?search_query=microsoft+adaptive+cards)
- [Teams Bot Development](https://learn.microsoft.com/en-us/microsoftteams/platform/bots/what-are-bots)

### Templates and Examples
- **Template Gallery**: https://adaptivecards.io/samples/
- **Card Repository**: https://github.com/pnp/AdaptiveCards-Templates

---

## Next Steps

### Beginner Path
1. ✅ Complete "Your First Adaptive Card" examples
2. ✅ Experiment in the Designer
3. ✅ Set up an incoming webhook in Teams
4. ✅ Send your first card to Teams
5. ✅ Customize cards for your use case

### Intermediate Path
1. Create a Teams bot with Teams Toolkit
2. Implement form handling with Action.Submit
3. Build a multi-step workflow
4. Integrate with external APIs
5. Implement card refresh

### Advanced Path
1. Implement Universal Actions
2. Create template-based cards
3. Build a card management system
4. Implement authentication flows
5. Create custom renderers

---

## Practice Exercises

### Exercise 1: Build a Welcome Card
Create a welcome card for new team members with:
- Company logo
- Welcome message
- Key links (HR portal, Team directory, etc.)
- A contact button

### Exercise 2: Daily Standup Card
Build a card for daily standup reports with:
- What did you do yesterday?
- What will you do today?
- Any blockers?
- Submit button

### Exercise 3: Incident Report Card
Create an incident reporting card with:
- Severity level (Critical, High, Medium, Low)
- Description
- Affected systems
- Submit and escalate actions

### Exercise 4: Poll Card
Build a voting/poll card with:
- Question
- Multiple choice options
- Submit vote action
- View results button

---

## Troubleshooting

### Common Issues

**Issue**: Card not displaying in Teams
- **Solution**: Check wrapper format, ensure version compatibility

**Issue**: Action.Submit not working
- **Solution**: Incoming webhooks don't support Submit actions, use a bot instead

**Issue**: Images not loading
- **Solution**: Use HTTPS URLs, ensure images are publicly accessible

**Issue**: Card looks different on mobile
- **Solution**: Use `wrap: true`, avoid fixed widths, test responsive design

**Issue**: Validation errors
- **Solution**: Use the Designer to validate JSON, check schema version

---

## Congratulations! 🎉

You've completed the Adaptive Cards practical learning guide! You now have the knowledge to:

✅ Understand what Adaptive Cards are and when to use them  
✅ Design beautiful, functional cards  
✅ Integrate cards with Microsoft Teams  
✅ Handle user interactions  
✅ Follow best practices  

**Keep Learning**: The best way to master Adaptive Cards is to build them! Start with simple cards and gradually add complexity.

Happy card building! 🚀

---

*Last Updated: April 2026*  
*Guide Version: 1.0*
