# Microsoft Adaptive Cards - Self-Learning Practical Guide

A comprehensive, hands-on guide to learning and mastering Microsoft Adaptive Cards. This repository contains everything you need to go from zero to expert in building and deploying adaptive cards across Microsoft platforms.

## 🎯 What You'll Learn

- **What** are Adaptive Cards and **why** use them
- **Features and capabilities** of Adaptive Cards
- **When** to use Adaptive Cards vs other solutions
- **Benefits** for users, developers, and organizations
- **Prerequisites** and environment setup
- **Building your first card** from scratch
- **Integration with Microsoft Teams**
- **Sending and receiving notifications**
- **🔄 Power Automate workflows** - Handle forms, approvals, and automation **NEW!**
- **Advanced workflows** and best practices

## 🚀 Quick Start

**New to Adaptive Cards?** Start here:
1. Read the [5-Minute Quick Start Guide](QUICK_START.md)
2. Send your first card to Teams in minutes!

## 📚 Documentation Structure

```
├── QUICK_START.md              # 5-minute getting started guide
├── ADAPTIVE_CARDS_GUIDE.md     # Complete comprehensive guide
├── examples/                    # Sample adaptive cards
│   ├── basic/                  # Beginner-friendly examples
│   ├── intermediate/           # Forms and interactive cards
│   └── advanced/               # Complex workflows
├── teams-integration/          # Teams integration code
│   ├── powershell/             # PowerShell examples
│   └── python/                 # Python examples
└── power-automate/             # 🔄 Power Automate workflows NEW!
    ├── basic/                  # Beginner flows (free tier)
    ├── intermediate/           # Interactive workflows
    ├── advanced/               # Enterprise-grade flows
    ├── tutorials/              # Step-by-step guides
    └── flow-exports/           # Importable flow packages
```

## 📖 Main Guide

The [Complete Adaptive Cards Guide](ADAPTIVE_CARDS_GUIDE.md) covers:

### 1. Foundations
- Understanding Adaptive Cards
- Use cases and benefits
- Features and capabilities
- When to use them

### 2. Getting Started
- Prerequisites
- Environment setup
- Building your first card
- Using the visual designer

### 3. Integration
- Microsoft Teams integration
- Incoming webhooks
- Bot framework
- Sending notifications

### 4. Advanced Topics
- Complex workflows
- Approval systems
- Form handling
- Best practices

## 💻 Code Examples

### PowerShell
```powershell
# Send a card to Teams
$webhookUrl = "YOUR_WEBHOOK_URL"
$card = @{ ... } | ConvertTo-Json -Depth 20
Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $card -ContentType "application/json"
```

### Python
### 🔄 Power Automate (No Code!)
```
1. Create flow with Recurrence trigger
2. Add HTTP action to send card to Teams webhook
3. Dynamically generate card content with expressions
4. Handle form submissions via HTTP triggers
5. Build approval workflows with built-in actions
```

See [Teams Integration](teams-integration/) for PowerShell/Python examples.  
See [Power Automate Integration](power-automate/) for no-code workflow
# Send a card to Teams
import requests
response = requests.post(webhook_url, json=card)
```

See [Teams Integration](teams-integration/) for complete examples.

## 🎨 Example Cards

### Basic Examples
- Simple welcome card
- Notification cards
- Contact cards

### Intermediate Examples
- Feedback forms
- Task assignments
- Polls and surveys

### Advanced Examples
- Approval workflows
- Incident reports
- Dashboard metrics

Browse [Examples Directory](examples/) for full catalog with working JSON files.

## 🔧 Hands-On Practice

Each section includes:
- ✅ Working code examples
- ✅ Complete JSON card definitions
- ✅ Step-by-step tutorials
- ✅ Practical exercises
- ✅ Troubleshooting guides

## 🌐 Official Resources

- **Adaptive Cards Homepage**: https://adaptivecards.io
- **Designer Tool**: https://adaptivecards.microsoft.com/designer
- **Microsoft Learn**: https://learn.microsoft.com/en-us/adaptive-cards/
- **Schema Reference**: https://adaptivecards.io/explorer/

## 🎓 Learning Path

### Beginner (30 minutes)
1. Read [Quick Start](QUICK_START.md)
2. Try basic examples from [examples/basic/](examples/basic/)
3. Send your first card to Teams

### Intermediate (2 hours)
1. Study the [Full Guide](ADAPTIVE_CARDS_GUIDE.md)
2. Build forms and interactive cards
3. Set up Teams webhook integration
4. Try [intermediate examples](examples/intermediate/)

### Advanced (4+ hours)
1. Implement approval workflows
2. Create custom templates
3. Build a Teams bot
4. Explore [advanced examples](examples/advanced/)

## 💡 Use Cases

Perfect for:
- 📢 **Notifications & Alerts**: System status, build results, monitoring
- 📋 **Forms & Surveys**: Feedback collection, data entry, polls
- ✅ **Approval Workflows**: Purchase orders, expense reports, HR processes
- 🤖 **Bot Interactions**: Welcome messages, help menus, conversations
- 📊 **Dashboards**: KPIs, reports, metrics
- 🔔 **Event Notifications**: Calendar events, reminders, updates

## 🛠️ Prerequisites

### Basic
- Web browser
- Microsoft Teams account
- Text editor

### Development (Optional)
- PowerShell 5.1+ or Python 3.7+
- Node.js (for bot development)
- Visual Studio Code (recommended)

## 📦 What's Included

- ✅ Complete learning guide (100+ pages)
- ✅ 9+ working card examples
- ✅ PowerShell integration code
- ✅ Python integration code
- ✅ Teams webhook setup guide
- ✅ Bot development examples
- ✅ Best practices and patterns
- ✅ Troubleshooting guides

## 🤝 Contributing

This is a learning resource. Feel free to:
- Use the examples in your projects
- Adapt the code for your needs
- Share feedback and suggestions

## 📄 License

See [LICENSE](LICENSE) file for details.

## 🎯 Next Steps

1. **Start Learning**: Open [QUICK_START.md](QUICK_START.md)
2. **Explore Examples**: Browse [examples/](examples/)
3. **Build Something**: Use [teams-integration/](teams-integration/)
4. **Go Deep**: Read [ADAPTIVE_CARDS_GUIDE.md](ADAPTIVE_CARDS_GUIDE.md)

---

**Ready to begin?** 🚀 Start with the [Quick Start Guide](QUICK_START.md)!

*Last Updated: April 2026*