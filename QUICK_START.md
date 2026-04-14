# Quick Start Guide - Adaptive Cards

Get started with Adaptive Cards in 5 minutes!

## 🎯 Goal
Send your first Adaptive Card to Microsoft Teams

## 📋 What You'll Need
- Microsoft Teams account
- 5 minutes of your time
- Web browser

## 🚀 Step-by-Step

### Step 1: Create an Incoming Webhook (2 minutes)

1. Open **Microsoft Teams**
2. Navigate to the channel where you want to send cards
3. Click the **"..."** (More options) next to the channel name
4. Select **"Connectors"** or **"Workflows"** (depending on your Teams version)
5. Search for **"Incoming Webhook"**
6. Click **"Add"** or **"Configure"**
7. Give it a name: `Adaptive Cards Test`
8. Optionally upload an icon
9. Click **"Create"**
10. **Copy the webhook URL** - you'll need this!

### Step 2: Test with Adaptive Cards Designer (2 minutes)

1. Go to https://adaptivecards.microsoft.com/designer
2. You'll see a sample card already loaded
3. Click **"Select host app"** dropdown at the top
4. Choose **"Microsoft Teams - Dark"** or **"Microsoft Teams - Light"**
5. See how your card looks!

### Step 3: Send Your First Card (1 minute)

#### Option A: Using PowerShell (Windows)

1. Open PowerShell
2. Copy and paste this script:

```powershell
$webhookUrl = "YOUR_WEBHOOK_URL_HERE"  # Paste your webhook URL

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
                        text = "🎉 My First Adaptive Card!"
                        size = "Large"
                        weight = "Bolder"
                    }
                    @{
                        type = "TextBlock"
                        text = "I just sent my first adaptive card to Teams!"
                        wrap = $true
                    }
                )
                actions = @(
                    @{
                        type = "Action.OpenUrl"
                        title = "Learn More"
                        url = "https://adaptivecards.io"
                    }
                )
            }
        }
    )
} | ConvertTo-Json -Depth 20

Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $card -ContentType "application/json"
Write-Host "✅ Card sent! Check your Teams channel!"
```

3. Press Enter
4. Check your Teams channel!

#### Option B: Using Python

1. Install requests: `pip install requests`
2. Create a file `send_card.py`:

```python
import requests
import json

WEBHOOK_URL = "YOUR_WEBHOOK_URL_HERE"  # Paste your webhook URL

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
                        "text": "🎉 My First Adaptive Card!",
                        "size": "Large",
                        "weight": "Bolder"
                    },
                    {
                        "type": "TextBlock",
                        "text": "I just sent my first adaptive card to Teams!",
                        "wrap": True
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

response = requests.post(WEBHOOK_URL, json=card)
print("✅ Card sent! Check your Teams channel!")
```

3. Run: `python send_card.py`
4. Check your Teams channel!

#### Option C: Using cURL (Any OS)

```bash
curl -X POST "YOUR_WEBHOOK_URL_HERE" \
  -H "Content-Type: application/json" \
  -d '{
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
              "text": "🎉 My First Adaptive Card!",
              "size": "Large",
              "weight": "Bolder"
            }
          ]
        }
      }
    ]
  }'
```

## ✅ Success!

If you see the card in Teams, congratulations! You've just:
- ✅ Created a Teams webhook
- ✅ Understood the basic card structure
- ✅ Sent your first adaptive card

---

## 🤖 Quick Start: Power Automate (5 minutes)

Automate card delivery without writing code!

### 🎯 Goal
Create a scheduled flow that sends a card to Teams every weekday morning

### 📋 What You'll Need
- Microsoft 365 account with Power Automate access
- Teams webhook from Step 1 above
- 5 minutes

### 🚀 Step-by-Step

#### Step 1: Create a Scheduled Flow (2 minutes)

1. Go to https://make.powerautomate.com
2. Sign in with your M365 account
3. Click **"+ Create"** in the left sidebar
4. Select **"Scheduled cloud flow"**
5. Configure:
   - **Flow name**: `Daily Team Reminder`
   - **Starting**: Tomorrow's date
   - **Repeat every**: `1` **Day**
   - **At these hours**: `9` (9 AM)
6. Click **"Create"**

#### Step 2: Add HTTP Action (2 minutes)

1. Click **"+ New step"**
2. Search for `HTTP`
3. Select **"HTTP"** action (under Standard connectors)
4. Fill in:
   - **Method**: `POST`
   - **URI**: Paste your Teams webhook URL from earlier
   - **Headers**: Click "Add new parameter" → Select "Headers"
     - Key: `Content-Type`
     - Value: `application/json`
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
            "type": "Container",
            "style": "emphasis",
            "items": [
              {
                "type": "TextBlock",
                "text": "☀️ Good Morning Team!",
                "size": "large",
                "weight": "bolder"
              }
            ]
          },
          {
            "type": "TextBlock",
            "text": "Today is @{formatDateTime(utcNow(), 'dddd, MMMM dd, yyyy')}",
            "wrap": true
          },
          {
            "type": "TextBlock",
            "text": "Have a productive day! 🚀",
            "wrap": true,
            "spacing": "medium"
          }
        ]
      }
    }
  ]
}
```

**Note**: The expression `@{formatDateTime(utcNow(), 'dddd, MMMM dd, yyyy')}` will automatically insert today's date!

#### Step 3: Test Your Flow (1 minute)

1. Click **"Save"** (top right)
2. Click **"Test"** button
3. Select **"Manually"**
4. Click **"Test"** → **"Run flow"** → **"Done"**
5. **Check your Teams channel!** You should see the card
6. Wait a few seconds, then check if the flow succeeded (green checkmark)

## ✅ Power Automate Success!

Your flow will now run automatically every weekday at 9 AM! 🎉

**What you just learned:**
- ✅ Created a scheduled Power Automate flow
- ✅ Used dynamic expressions to insert today's date
- ✅ Automated card delivery without writing code
- ✅ Tested and verified your flow works

### Disable/Edit Your Flow

**To pause**: 
- Go to https://make.powerautomate.com → "My flows"
- Find "Daily Team Reminder"
- Toggle to "Off"

**To change the schedule**:
- Open the flow
- Click on the "Recurrence" trigger
- Change time/frequency
- Click "Save"

---

## 🎓 What Next?

### Learn More
1. Read the [Complete Guide](ADAPTIVE_CARDS_GUIDE.md)
2. Read the [Power Automate Guide](POWER_AUTOMATE_GUIDE.md) - Full automation guide
3. Try [Example Cards](examples/)
4. Explore [Teams Integration](teams-integration/)
5. Try [Power Automate Flows](power-automate/) - Pre-built automation examples

### Build Something Cool
- Daily standup reports
- Build notifications
- Approval workflows (try [Power Automate](power-automate/))
- Status dashboards
- Alert systems
- Scheduled reminders (perfect for Power Automate!)
- Form handling with automated responses

### Experiment
1. Go to https://adaptivecards.microsoft.com/designer
2. Modify the sample cards
3. Copy the JSON
4. Send it to Teams using the methods above!

## 🐛 Troubleshooting

### Card Issues

**"Card not showing in Teams"**
- Check that webhook URL is correct
- Ensure JSON structure has the Teams wrapper (`type: "message"`, `attachments`)
- Verify card version is 1.4 or 1.5 (Teams supports up to 1.5)

**"Error 400 Bad Request"**
- JSON might be malformed
- Check quotation marks and commas
- Validate JSON at https://jsonlint.com

**"Webhook not found"**
- Webhook might have been deleted
- Create a new webhook and update the URL

### Power Automate Issues

**"Flow not running on schedule"**
- Check that flow is turned "On" (toggle at top)
- Verify recurrence settings (time zone, frequency)
- Check run history for errors

**"HTTP action failed"**
- Verify webhook URL is correct (copy from Teams webhook settings)
- Check that `Content-Type` header is set to `application/json`
- Ensure JSON body is properly formatted

**"Expression error"**
- Make sure expression syntax is correct: `@{formatDateTime(utcNow(), 'format')}`
- Test expressions in a "Compose" action first
- Check for matching braces `{ }`

## 💡 Pro Tips

1. **Save Your Webhook**: Store it securely, you'll use it often
2. **Use the Designer**: Design visually, then copy the JSON
3. **Start Simple**: Begin with basic cards, add complexity gradually
4. **Test Variations**: Try different colors, sizes, and layouts
5. **Automate with Power Automate**: Use flows for scheduled cards, approvals, and form handling
6. **Use Dynamic Content**: Power Automate expressions like `@{formatDateTime()}` make cards dynamic
7. **Store Webhook in Variables**: In Power Automate, use environment variables for webhook URLs

## 📚 Resources

### Adaptive Cards
- **Designer**: https://adaptivecards.microsoft.com/designer
- **Samples**: https://adaptivecards.io/samples/
- **Documentation**: https://learn.microsoft.com/en-us/adaptive-cards/

### Power Automate
- **Portal**: https://make.powerautomate.com
- **Documentation**: https://learn.microsoft.com/power-automate/
- **Community**: https://powerusers.microsoft.com/

### This Repository
- **Complete Adaptive Cards Guide**: [ADAPTIVE_CARDS_GUIDE.md](ADAPTIVE_CARDS_GUIDE.md)
- **Complete Power Automate Guide**: [POWER_AUTOMATE_GUIDE.md](POWER_AUTOMATE_GUIDE.md)
- **Card Examples**: [examples/](examples/)
- **Power Automate Flows**: [power-automate/](power-automate/)
- **Cheat Sheet**: [CHEAT_SHEET.md](CHEAT_SHEET.md)

---

**Time to celebrate!** 🎉 You're now sending Adaptive Cards AND automating them with Power Automate!

**Next Steps:**
- [Explore the Full Adaptive Cards Guide](ADAPTIVE_CARDS_GUIDE.md)
- [Explore the Full Power Automate Guide](POWER_AUTOMATE_GUIDE.md)
- [Try Pre-Built Flows](power-automate/)
