# ⭐ Basic Power Automate Flows

Beginner-friendly Power Automate flows for sending and receiving Adaptive Cards. All examples use **free-tier connectors** and are perfect for learning fundamentals.

## 📋 Prerequisites

- ✅ Microsoft 365 account with Power Automate access
- ✅ Microsoft Teams access
- ✅ Teams webhook URL (see [main README](../README.md#quick-start) for setup)

## 🎯 What You'll Learn

- Send cards on a schedule (recurrence trigger)
- Send cards manually (button trigger)
- Receive form submissions via HTTP trigger
- Parse JSON data from Adaptive Cards
- Send email notifications with form data
- Basic error handling in flows

## 📚 Flow Examples

### 1. Scheduled Card Sender ⏰

**Difficulty**: ⭐ Very Easy  
**Triggers**: Recurrence  
**Actions**: HTTP (POST to webhook)  
**Use Cases**: Daily standup reminders, weekly reports, scheduled notifications

**What it does**: Automatically sends a welcome or notification card to Teams on a schedule (daily, weekly, hourly, etc.).

#### Flow Steps

1. **Trigger**: Recurrence - Set to run daily at 9:00 AM
2. **Action**: HTTP - POST card to Teams webhook

#### Complete Flow Configuration

**Trigger Settings**:
- Interval: `1`
- Frequency: `Day`
- Time zone: `(Your timezone)`
- At these hours: `9`
- At these minutes: `0`

**HTTP Action Settings**:
- Method: `POST`
- URI: `YOUR_TEAMS_WEBHOOK_URL`
- Headers:
  ```
  Content-Type: application/json
  ```
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
              "text": "Daily standup reminder - please share your updates!",
              "wrap": true,
              "spacing": "medium"
            },
            {
              "type": "FactSet",
              "facts": [
                {
                  "title": "📅 Date:",
                  "value": "@{formatDateTime(utcNow(), 'yyyy-MM-dd')}"
                },
                {
                  "title": "🕐 Time:",
                  "value": "9:00 AM"
                },
                {
                  "title": "📍 Location:",
                  "value": "Zoom Meeting Room"
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
              "text": "• What did you complete yesterday?\n• What are you working on today?\n• Any blockers or challenges?",
              "wrap": true
            }
          ],
          "actions": [
            {
              "type": "Action.OpenUrl",
              "title": "Join Standup",
              "url": "https://zoom.us/j/your-meeting-id"
            }
          ]
        }
      }
    ]
  }
  ```

**💡 Dynamic Content**:  
Notice `@{formatDateTime(utcNow(), 'yyyy-MM-dd')}` in the FactSet - this is Power Automate expression syntax that generates the current date dynamically. You can use expressions like:
- `@{utcNow()}` - Current UTC time
- `@{formatDateTime(utcNow(), 'dddd, MMMM dd, yyyy')}` - Friendly date format
- `@{addDays(utcNow(), 7)}` - One week from now

#### Variations

**Weekly Report (Fridays at 5 PM)**:
- Change frequency to `Week`
- Set "On these days": `Friday`
- Set "At these hours": `17`

**Hourly Reminder**:
- Change frequency to `Hour`
- Set interval to desired hours (e.g., `2` for every 2 hours)

**Custom Schedule**:
- Use advanced options for specific dates/times
- Add conditions to skip holidays

---

### 2. Form Submission Handler 📝

**Difficulty**: ⭐⭐ Easy  
**Triggers**: HTTP Request (When a HTTP request is received)  
**Actions**: Parse JSON, Send email, Post to Teams  
**Use Cases**: Feedback forms, bug reports, survey responses, contact forms

**What it does**: Receives form data from an Adaptive Card Action.Submit button, parses the data, and sends email notification with the submission details.

#### Flow Steps

1. **Trigger**: When a HTTP request is received
2. **Action**: Parse JSON - Extract form fields
3. **Action**: Send email - Notify about submission
4. **Action**: HTTP - Post confirmation to Teams

#### Complete Flow Configuration

**Step 1: HTTP Request Trigger**

Click "When a HTTP request is received" → Copy the URL after saving (you'll use this later)

Request Body JSON Schema:
```json
{
  "type": "object",
  "properties": {
    "feedbackType": {
      "type": "string"
    },
    "rating": {
      "type": "string"
    },
    "comments": {
      "type": "string"
    },
    "email": {
      "type": "string"
    },
    "followUp": {
      "type": "string"
    }
  }
}
```

**Step 2: Parse JSON** (optional if schema above is configured)

If you didn't configure schema, add Parse JSON action:
- Content: `@{triggerBody()}`
- Schema: Same as above

**Step 3: Send Email (V2)**

- To: `your-email@company.com` (or use dynamic content from form)
- Subject: `New Feedback Received - @{body('Parse_JSON')?['feedbackType']}`
- Body:
  ```
  A new feedback form was submitted:
  
  Feedback Type: @{body('Parse_JSON')?['feedbackType']}
  Rating: @{body('Parse_JSON')?['rating']}
  Comments: @{body('Parse_JSON')?['comments']}
  Email: @{body('Parse_JSON')?['email']}
  Follow-up Requested: @{body('Parse_JSON')?['followUp']}
  
  Submitted at: @{utcNow()}
  ```

**Step 4: HTTP - Post Confirmation to Teams**

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
              "style": "good",
              "items": [
                {
                  "type": "TextBlock",
                  "text": "✅ Feedback Received",
                  "size": "large",
                  "weight": "bolder"
                }
              ]
            },
            {
              "type": "TextBlock",
              "text": "Thank you for your feedback! We've received your @{body('Parse_JSON')?['feedbackType']} submission.",
              "wrap": true
            },
            {
              "type": "FactSet",
              "facts": [
                {
                  "title": "Rating:",
                  "value": "@{body('Parse_JSON')?['rating']}"
                },
                {
                  "title": "Submitted by:",
                  "value": "@{body('Parse_JSON')?['email']}"
                },
                {
                  "title": "Received at:",
                  "value": "@{formatDateTime(utcNow(), 'yyyy-MM-dd HH:mm')}"
                }
              ]
            }
          ]
        }
      }
    ]
  }
  ```

#### How to Use This Flow

This flow is designed to work with the [Feedback Form card](../../examples/intermediate/01-feedback-form.json). However, that card doesn't directly trigger flows - you need to connect it. Here are two approaches:

**Approach A: Use Teams Messaging Extension** (Recommended)
1. Replace the HTTP trigger with "For a selected message" trigger (Teams connector)
2. Use "Post your own adaptive card as the Flow bot to a channel" action to send the card
3. This approach automatically handles Action.Submit responses

**Approach B: Custom Teams App** (Advanced)
1. Create a Teams app with messaging extension
2. Configure the app to call this flow's HTTP endpoint when Action.Submit is clicked
3. See [POWER_AUTOMATE_GUIDE.md](../../POWER_AUTOMATE_GUIDE.md) for detailed steps

**Approach C: Simplified Testing**
For learning purposes, test this flow by:
1. Use a tool like Postman to POST JSON directly to the trigger URL
2. Example request body:
   ```json
   {
     "feedbackType": "Product Feedback",
     "rating": "5 - Excellent",
     "comments": "Great product! Very easy to use.",
     "email": "user@example.com",
     "followUp": "true"
   }
   ```

---

### 3. Button Click Acknowledgment ✅

**Difficulty**: ⭐ Very Easy  
**Triggers**: Manual (button in Teams/email/other app)  
**Actions**: HTTP (POST to webhook)  
**Use Cases**: Task completion, simple acknowledgments, quick notifications

**What it does**: When you click a button (manually run flow), it sends a confirmation card to Teams acknowledging the action.

#### Flow Steps

1. **Trigger**: Manually trigger a flow
2. **Action**: HTTP - POST acknowledgment card to Teams

#### Complete Flow Configuration

**Trigger**: Manually trigger a flow
- No configuration needed - this creates a button you can click in Power Automate or Teams

**HTTP Action**:
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
                          "text": "Task Completed",
                          "size": "large",
                          "weight": "bolder"
                        },
                        {
                          "type": "TextBlock",
                          "text": "The task was marked as complete.",
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
              "type": "FactSet",
              "facts": [
                {
                  "title": "Completed by:",
                  "value": "Flow User"
                },
                {
                  "title": "Completed at:",
                  "value": "@{formatDateTime(utcNow(), 'yyyy-MM-dd HH:mm:ss')}"
                },
                {
                  "title": "Status:",
                  "value": "✅ Done"
                }
              ]
            },
            {
              "type": "TextBlock",
              "text": "Great job! Keep up the good work! 🎉",
              "wrap": true,
              "spacing": "medium",
              "color": "good"
            }
          ],
          "actions": [
            {
              "type": "Action.OpenUrl",
              "title": "View Details",
              "url": "https://your-task-system.com/tasks"
            }
          ]
        }
      }
    ]
  }
  ```

#### Adding Input Parameters

You can enhance this flow by adding input fields to the manual trigger:

1. Click on the manual trigger
2. Click "+ Add an input"
3. Add inputs like:
   - **Text**: Task Name
   - **Text**: Completed By
   - **Yes/No**: Notify Team

Then reference these in the card body using dynamic content:
```json
{
  "title": "Task:",
  "value": "@{triggerBody()?['text']}"
}
```

---

## 🎓 Learning Exercises

Test your understanding with these hands-on challenges:

### Exercise 1: Customize the Schedule
Modify the **Scheduled Card Sender** to:
- Send reminders every Monday and Wednesday at 2 PM
- Include the day of the week in the card
- Add a link to your calendar

<details>
<summary>💡 Hint</summary>

- Use "On these days" in recurrence trigger
- Use `@{formatDateTime(utcNow(), 'dddd')}` for day name
- Add Action.OpenUrl with your calendar link
</details>

### Exercise 2: Multi-Field Form
Extend the **Form Submission Handler** to:
- Accept additional fields (department, priority, category)
- Add conditional logic - only send email if priority is "High"
- Format email body with nice HTML styling

<details>
<summary>💡 Hint</summary>

- Add fields to JSON schema
- Use "Condition" action after Parse JSON
- Use Send Email (V2) with HTML body
</details>

### Exercise 3: Dynamic Button
Enhance the **Button Click Acknowledgment** to:
- Ask for task name and assignee via input parameters
- Include these in the confirmation card
- Send card to different channels based on task type

<details>
<summary>💡 Hint</summary>

- Add input parameters to manual trigger
- Use dynamic content in card JSON
- Add "Condition" to check task type and switch webhook URLs
</details>

---

## 🔧 Common Patterns

### Error Handling

Add error handling to HTTP actions:

1. Click **...** (three dots) on the HTTP action
2. Select **Configure run after**
3. Check ☑ **has failed**
4. Add a **Send email** action to notify on failures

### Environment Variables for Webhook URL

Instead of hardcoding webhook URLs:

1. Go to [Power Automate](https://make.powerautomate.com) → Solutions
2. Create environment variable: `TeamsWebhookURL`
3. Reference in flows: `@{parameters('TeamsWebhookURL')}`

**Benefits**: Change webhook once, updates all flows

### Compose for Complex JSON

Use **Compose** action to build JSON before HTTP:

1. Add **Compose** action before HTTP
2. Build your card JSON in Compose
3. Reference in HTTP body: `@{outputs('Compose')}`

**Benefits**: Better readability, easier debugging

---

## 📦 Import Ready-Made Flows

Download and import these flows directly into Power Automate:

- **[basic-scheduled-card.zip](../flow-exports/basic-scheduled-card.zip)** - Scheduled Card Sender
- **[basic-form-handler.zip](../flow-exports/basic-form-handler.zip)** - Form Submission Handler
- **[basic-button-acknowledgment.zip](../flow-exports/basic-button-acknowledgment.zip)** - Button Click Acknowledgment

**How to import**:
1. Download the .zip file
2. Go to [Power Automate](https://make.powerautomate.com) → **My flows**
3. Click **Import** → **Import Package (Legacy)**
4. Upload the .zip file → **Import**
5. Configure connections and update webhook URLs

---

## 🚀 Next Steps

Completed the basic flows? Level up with:

1. **[Intermediate Flows](../intermediate/)** - Approvals, dynamic cards, task updates
2. **[Advanced Flows](../advanced/)** - Multi-level approvals, incident management, integrations
3. **[Tutorials](../tutorials/)** - Step-by-step guides with screenshots
4. **[Power Automate Guide](../../POWER_AUTOMATE_GUIDE.md)** - Comprehensive deep-dive

---

## 💡 Tips & Tricks

**Testing Flows**:
- Use "Test" button in Flow designer for quick iterations
- Check "Flow checker" (top right) for warnings/errors
- View run history to debug failed flows

**Performance**:
- Keep HTTP timeout reasonable (30-60 seconds)
- Avoid nested loops with large datasets
- Use "Do until" sparingly (can hit iteration limits)

**Best Practices**:
- Name actions descriptively ("Send Welcome Card" not "HTTP")
- Add comments using "Add a note" on actions
- Version control: Export flows regularly as backup

---

**Questions? Issues?** Check the [main README troubleshooting](../README.md#troubleshooting) or open an issue!

*Last updated: April 2026*
