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

## 🎓 What Next?

### Learn More
1. Read the [Complete Guide](ADAPTIVE_CARDS_GUIDE.md)
2. Try [Example Cards](examples/)
3. Explore [Teams Integration](teams-integration/)

### Build Something Cool
- Daily standup reports
- Build notifications
- Approval workflows
- Status dashboards
- Alert systems

### Experiment
1. Go to https://adaptivecards.microsoft.com/designer
2. Modify the sample cards
3. Copy the JSON
4. Send it to Teams using the methods above!

## 🐛 Troubleshooting

### "Card not showing in Teams"
- Check that webhook URL is correct
- Ensure JSON structure has the Teams wrapper (`type: "message"`, `attachments`)
- Verify card version is 1.4 or 1.5 (Teams supports up to 1.5)

### "Error 400 Bad Request"
- JSON might be malformed
- Check quotation marks and commas
- Validate JSON at https://jsonlint.com

### "Webhook not found"
- Webhook might have been deleted
- Create a new webhook and update the URL

## 💡 Pro Tips

1. **Save Your Webhook**: Store it securely, you'll use it often
2. **Use the Designer**: Design visually, then copy the JSON
3. **Start Simple**: Begin with basic cards, add complexity gradually
4. **Test Variations**: Try different colors, sizes, and layouts

## 📚 Resources

- **Designer**: https://adaptivecards.microsoft.com/designer
- **Samples**: https://adaptivecards.io/samples/
- **Documentation**: https://learn.microsoft.com/en-us/adaptive-cards/

---

**Time to celebrate!** 🎉 You're now an Adaptive Cards user!

Next: [Explore the Full Guide](ADAPTIVE_CARDS_GUIDE.md)
