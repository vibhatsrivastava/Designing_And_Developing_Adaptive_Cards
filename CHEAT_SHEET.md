 # Adaptive Cards - Quick Reference Cheat Sheet

## 📐 Basic Card Structure

```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [ /* elements */ ],
  "actions": [ /* actions */ ]
}
```

## 📝 Common Elements

### TextBlock
```json
{
  "type": "TextBlock",
  "text": "Your text here",
  "size": "Medium",              // Small, Default, Medium, Large, ExtraLarge
  "weight": "Bolder",            // Lighter, Default, Bolder
  "color": "Accent",             // Default, Dark, Light, Accent, Good, Warning, Attention
  "wrap": true,
  "isSubtle": false,
  "horizontalAlignment": "Left"  // Left, Center, Right
}
```

### Image
```json
{
  "type": "Image",
  "url": "https://example.com/image.png",
  "size": "Medium",              // Auto, Stretch, Small, Medium, Large
  "style": "Default",            // Default, Person
  "horizontalAlignment": "Center",
  "altText": "Description"
}
```

### FactSet (Key-Value Pairs)
```json
{
  "type": "FactSet",
  "facts": [
    { "title": "Key:", "value": "Value" },
    { "title": "Name:", "value": "John Doe" }
  ]
}
```

## 🎨 Layout Elements

### Container
```json
{
  "type": "Container",
  "style": "emphasis",           // default, emphasis, good, attention, warning, accent
  "items": [ /* elements */ ],
  "separator": true,
  "spacing": "Medium"            // None, Small, Default, Medium, Large, ExtraLarge, Padding
}
```

### ColumnSet
```json
{
  "type": "ColumnSet",
  "columns": [
    {
      "type": "Column",
      "width": "auto",           // auto, stretch, or number (e.g., "50", "100px")
      "items": [ /* elements */ ]
    },
    {
      "type": "Column",
      "width": "stretch",
      "items": [ /* elements */ ]
    }
  ]
}
```

## 📥 Input Elements

### Input.Text
```json
{
  "type": "Input.Text",
  "id": "uniqueId",
  "label": "Name",
  "placeholder": "Enter your name",
  "isMultiline": false,
  "maxLength": 100,
  "isRequired": true,
  "errorMessage": "This field is required",
  "style": "Text"                // Text, Tel, Url, Email
}
```

### Input.Number
```json
{
  "type": "Input.Number",
  "id": "age",
  "label": "Age",
  "placeholder": "Enter age",
  "min": 0,
  "max": 120,
  "value": 25
}
```

### Input.Date
```json
{
  "type": "Input.Date",
  "id": "startDate",
  "label": "Start Date",
  "value": "2024-01-15"
}
```

### Input.Time
```json
{
  "type": "Input.Time",
  "id": "meetingTime",
  "label": "Meeting Time",
  "value": "14:30"
}
```

### Input.Toggle
```json
{
  "type": "Input.Toggle",
  "id": "subscribe",
  "title": "Subscribe to newsletter",
  "value": "true",
  "valueOn": "true",
  "valueOff": "false"
}
```

### Input.ChoiceSet
```json
{
  "type": "Input.ChoiceSet",
  "id": "department",
  "label": "Department",
  "style": "compact",            // compact or expanded
  "isMultiSelect": false,
  "choices": [
    { "title": "Engineering", "value": "eng" },
    { "title": "Sales", "value": "sales" }
  ],
  "value": "eng"                 // default value
}
```

## 🎬 Actions

### Action.OpenUrl
```json
{
  "type": "Action.OpenUrl",
  "title": "Open Link",
  "url": "https://example.com",
  "style": "positive"            // default, positive, destructive
}
```

### Action.Submit
```json
{
  "type": "Action.Submit",
  "title": "Submit",
  "data": {
    "actionType": "formSubmit",
    "formId": "feedback-form"
  },
  "style": "positive"
}
```

### Action.ShowCard
```json
{
  "type": "Action.ShowCard",
  "title": "Show Details",
  "card": {
    "type": "AdaptiveCard",
    "body": [ /* nested card elements */ ]
  }
}
```

## 🎨 Styling Properties

### Colors
- `Default` - Standard text color
- `Dark` - Darker shade
- `Light` - Lighter shade
- `Accent` - Brand/accent color
- `Good` - Success/positive (usually green)
- `Warning` - Warning (usually yellow/orange)
- `Attention` - Error/critical (usually red)

### Sizes
- `Small`
- `Default`
- `Medium`
- `Large`
- `ExtraLarge`

### Weights
- `Lighter`
- `Default`
- `Bolder`

### Spacing
- `None`
- `Small`
- `Default`
- `Medium`
- `Large`
- `ExtraLarge`
- `Padding`

## 📦 Teams Message Wrapper

**Required for Teams Webhooks:**

```json
{
  "type": "message",
  "attachments": [
    {
      "contentType": "application/vnd.microsoft.card.adaptive",
      "content": {
        /* Your Adaptive Card JSON */
      }
    }
  ]
}
```

## 💻 Quick Send Commands

### PowerShell
```powershell
$webhookUrl = "YOUR_URL"
$card = @{ ... } | ConvertTo-Json -Depth 20
Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $card -ContentType "application/json"
```

### Python
```python
import requests
requests.post(webhook_url, json=message)
```

### cURL
```bash
curl -X POST "YOUR_URL" -H "Content-Type: application/json" -d @card.json
```

### JavaScript
```javascript
fetch(webhookUrl, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(message)
});
```

## 🔍 Common Patterns

### Header with Icon
```json
{
  "type": "ColumnSet",
  "columns": [
    {
      "type": "Column",
      "width": "auto",
      "items": [{ "type": "Image", "url": "icon.png", "size": "Small" }]
    },
    {
      "type": "Column",
      "width": "stretch",
      "items": [
        { "type": "TextBlock", "text": "Title", "weight": "Bolder" },
        { "type": "TextBlock", "text": "Subtitle", "isSubtle": true }
      ]
    }
  ]
}
```

### Status Badge
```json
{
  "type": "Container",
  "style": "good",
  "items": [
    { "type": "TextBlock", "text": "✅ Success", "color": "Good" }
  ]
}
```

### Two-Column Layout
```json
{
  "type": "ColumnSet",
  "columns": [
    { "type": "Column", "width": "50", "items": [ /* left */ ] },
    { "type": "Column", "width": "50", "items": [ /* right */ ] }
  ]
}
```

## 🐛 Common Mistakes

❌ **Wrong:**
```json
"version": "2.0"  // Teams doesn't support 2.0
```
✅ **Correct:**
```json
"version": "1.5"  // Use 1.4 or 1.5
```

❌ **Wrong:**
```json
"type": "text"  // Lowercase, elements have capital T
```
✅ **Correct:**
```json
"type": "TextBlock"
```

❌ **Wrong:**
```json
{ "actions": { "type": "..." } }  // Actions is array, not object
```
✅ **Correct:**
```json
{ "actions": [{ "type": "..." }] }
```

## 📊 Version Compatibility

| Feature | Min Version | Teams Support |
|---------|-------------|---------------|
| Basic Elements | 1.0 | ✅ |
| Input.ChoiceSet | 1.0 | ✅ |
| Action.ShowCard | 1.0 | ✅ |
| FactSet | 1.0 | ✅ |
| Container.style | 1.2 | ✅ |
| Input.Date/Time | 1.0 | ✅ |
| RichTextBlock | 1.2 | ✅ |
| Table | 1.5 | ✅ |
| Action.Execute | 1.4 | ✅ |

## 🔄 Power Automate Integration

### Common Triggers

**Recurrence** (⏰ Scheduled Cards)
```
Trigger: Recurrence
Interval: 1, Frequency: Day
At these hours: 9
→ Sends card daily at 9 AM
```

**HTTP Request** (📝 Form Submissions)
```
Trigger: When a HTTP request is received
Request Body Schema: { "field1": "string", "field2": "string" }
→ Copy URL after saving, use in Action.Submit
```

**Manual** (🔘 Button-Triggered)
```
Trigger: Manually trigger a flow
Add inputs: Text, Number, Yes/No, etc.
→ Run from Power Automate or Teams
```

**SharePoint** (💎 Premium)
```
Trigger: When an item is created
List: "Requests"
→ Auto-send card when SharePoint item added
```

### Common Actions

**HTTP - Send Card to Teams**
```
Method: POST
URI: YOUR_TEAMS_WEBHOOK_URL
Headers: Content-Type = application/json
Body: Card wrapped in Teams message envelope
```

**Teams - Post Adaptive Card** (💎 Premium - Recommended)
```
Post as: Flow bot
Post in: Channel
Adaptive Card: { card JSON }
→ Returns message ID for updates
```

**Start and Wait for Approval**
```
Approval type: Approve/Reject - First to respond
Title: "Approve expense request"
Assigned to: manager@company.com
→ Use outcome in Condition: "Approve" or "Reject"
```

**Condition - Branch Logic**
```
If: @{outputs('Approval')?['body/outcome']}
Equals: Approve
Then: Send approval email + update SharePoint
Else: Send rejection email
```

**Update Message** (💎 Premium)
```
Message: @{outputs('Post_card')?['messageId']}
Update card: Modified card JSON with new status
→ Dynamically update existing card in Teams
```

### Dynamic Content Examples

**Current Date/Time**
```
@{formatDateTime(utcNow(), 'yyyy-MM-dd HH:mm')}
→ "2026-04-14 15:30"
```

**Add Days**
```
@{formatDateTime(addDays(utcNow(), 7), 'yyyy-MM-dd')}
→ Date 7 days from now
```

**Format Number**
```
@{formatNumber(1234.5, 'N2')}
→ "1,234.50"
```

**Reference Form Data**
```
@{triggerBody()?['feedbackRating']}
→ Value from Input.ChoiceSet with id="feedbackRating"
```

**Conditional Expression**
```json
{
  "color": "@{if(equals(triggerBody()?['priority'], 'High'), 'attention', 'default')}"
}
```

### Teams Message Envelope

**Required wrapper for webhook cards:**
```json
{
  "type": "message",
  "attachments": [{
    "contentType": "application/vnd.microsoft.card.adaptive",
    "content": {
      "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
      "type": "AdaptiveCard",
      "version": "1.5",
      "body": [ /* your card elements */ ]
    }
  }]
}
```

### Action.Submit Data Pattern

**In your card:**
```json
{
  "type": "Action.Submit",
  "title": "Submit Feedback",
  "data": {
    "action": "submitFeedback",
    "formId": "feedback-001"
  }
}
```

**In your flow (HTTP trigger):**
```
Parse JSON: @{triggerBody()}
Check action: @{body('Parse_JSON')?['action']}
Get form data: @{body('Parse_JSON')?['inputField']}
```

### Error Handling

**Configure Run After**
```
1. Click "..." on action → Configure run after
2. Check ☑ "has failed"
3. Add Send Email action to notify on errors
```

**Scope for Error Handling**
```
Scope: Main Flow Logic
  ├─ Send card
  ├─ Process form
  └─ Update database
  
Scope: Error Handler (runs if Main failed)
  ├─ Log error to SharePoint
  └─ Send alert email
```

### Quick Reference

| Task | Free Tier | Premium | 
|------|-----------|---------|
| Send scheduled cards | ✅ HTTP | ✅ Teams connector |
| Receive form data | ✅ HTTP trigger | ✅ HTTP trigger |
| Built-in approvals | ✅ Yes | ✅ Yes |
| Update cards | ❌ No | ✅ Update message |
| SharePoint integration | ❌ No | ✅ Yes |
| SQL connector | ❌ No | ✅ Yes |

**💡 Pro Tips:**
- Use native Teams connector (Premium) for card updates
- Store webhook URLs in environment variables
- Add retries for external API calls
- Test flows with "Test" button before deploying
- Name actions descriptively for easier debugging
- Use Compose action for complex JSON (better readability)

See [power-automate/](power-automate/) for complete flow examples!

## 🔗 Quick Links

- **Designer**: https://adaptivecards.microsoft.com/designer
- **Samples**: https://adaptivecards.io/samples/
- **Schema**: https://adaptivecards.io/explorer/
- **Docs**: https://learn.microsoft.com/en-us/adaptive-cards/

## 💡 Pro Tips

1. **Always test in the Designer** before sending to Teams
2. **Use `wrap: true`** for long text
3. **Include `altText`** for images (accessibility)
4. **Keep cards under 28KB** (Teams limit)
5. **Use `isRequired`** for mandatory form fields
6. **Add unique `id`** to all inputs
7. **Include `actionType`** in submit data for easy routing
8. **Test on mobile** - cards look different on small screens

---

**Print this page and keep it handy!**

For complete examples, see [examples/](examples/) directory.
