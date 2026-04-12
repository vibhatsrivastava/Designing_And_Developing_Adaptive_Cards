# Adaptive Cards Examples

This directory contains practical examples of Adaptive Cards organized by complexity level.

## 📁 Directory Structure

```
examples/
├── basic/           - Simple cards for beginners
├── intermediate/    - Forms and interactive cards
└── advanced/        - Complex workflows and dashboards
```

## 🎯 How to Use These Examples

### 1. **Visual Testing**
- Go to https://adaptivecards.microsoft.com/designer
- Copy the JSON from any example file
- Paste it into the "Card Payload Editor"
- See it render in real-time!

### 2. **Teams Integration**
- Use the Teams integration scripts in `/teams-integration`
- Replace the card payload with examples from this folder
- Send to your Teams channel

### 3. **Learning Path**
Start with basic examples and progress to advanced:
1. Basic → Intermediate → Advanced
2. Modify examples to fit your needs
3. Combine elements from different examples

## 📋 Basic Examples

### 01-simple-welcome.json
A minimal welcome card demonstrating:
- TextBlock elements
- Images
- Action buttons
- Basic styling

**Perfect for:** First-time users, simple notifications

### 02-simple-notification.json
A notification card showing:
- Container styling
- FactSet for structured data
- Emphasis styling

**Perfect for:** System alerts, status updates

### 03-contact-card.json
A contact information card with:
- ColumnSet for layout
- Multiple action buttons
- Profile image styling

**Perfect for:** Team directories, user profiles

## 📊 Intermediate Examples

### 01-feedback-form.json
A comprehensive feedback form featuring:
- Multiple input types (Text, Email, ChoiceSet)
- Multi-select options
- Form validation (isRequired)
- Toggle switches
- Multi-line text input

**Perfect for:** Surveys, customer feedback, data collection

### 02-task-assignment.json
A task management card with:
- Complex layouts with multiple containers
- FactSet for task details
- Date input
- Status selection
- Multiple action buttons

**Perfect for:** Project management, task tracking, workflow automation

### 03-poll-voting.json
An interactive voting card demonstrating:
- Expanded choice set style
- Real-time results display
- Optional text input
- Visual result representation

**Perfect for:** Team polls, decision making, gathering opinions

## 🚀 Advanced Examples

### 01-approval-workflow.json
A sophisticated approval card featuring:
- Multi-section layout
- Approval history tracking
- Nested ShowCard actions
- Item list display
- Business justification section
- Complex action handling

**Perfect for:** Purchase approvals, expense reports, HR workflows

### 02-incident-report.json
A critical incident management card with:
- Attention-grabbing styling
- Timeline of events
- Multiple status indicators
- Technical details section
- Communication tracking
- War room integration

**Perfect for:** IT operations, DevOps, incident management

### 03-dashboard-metrics.json
A performance dashboard featuring:
- Multi-column KPI display
- Visual progress bars
- Achievement tracking
- Hierarchical information
- Nested feedback card

**Perfect for:** Executive dashboards, performance reviews, reporting

## 🎨 Key Learning Points

### From Basic Examples Learn:
- Card structure basics
- Simple elements (TextBlock, Image)
- Basic actions (Action.OpenUrl)
- Element properties (size, weight, color)

### From Intermediate Examples Learn:
- Form inputs and validation
- ChoiceSet options (compact vs expanded)
- Multi-line inputs
- Container organization
- Action.Submit usage

### From Advanced Examples Learn:
- Complex layouts
- ShowCard actions
- Conditional styling
- Multi-step workflows
- Professional card design patterns

## 💡 Tips for Modification

### Changing Colors
```json
"color": "Default" | "Dark" | "Light" | "Accent" | "Good" | "Warning" | "Attention"
```

### Adjusting Sizes
```json
"size": "Small" | "Default" | "Medium" | "Large" | "ExtraLarge"
```

### Layout Widths
```json
"width": "auto" | "stretch" | "30px" | "50"
```

### Container Styles
```json
"style": "default" | "emphasis" | "good" | "attention" | "warning" | "accent"
```

## 🔗 Testing Your Cards

### Online Designer
1. Visit: https://adaptivecards.microsoft.com/designer
2. Select "Sample Data Editor" if you want to test with dynamic data
3. Choose different host apps (Teams, Outlook, etc.) to see rendering differences

### Teams Testing
1. Set up an Incoming Webhook in Teams
2. Use the PowerShell or Python scripts in `/teams-integration`
3. Modify the card JSON path to test different examples

## 📚 Next Steps

1. **Experiment**: Modify these examples to understand each property
2. **Combine**: Mix elements from different examples
3. **Create**: Build your own cards based on your requirements
4. **Share**: Use in your Teams channels and workflows

## ⚠️ Important Notes

- **Version Compatibility**: All examples use version 1.5, but Teams supports up to 1.5
- **Action Limitations**: `Action.Submit` requires a bot or backend service
- **Testing**: Always test cards in your target platform (Teams, Outlook, etc.)
- **Images**: Replace placeholder URLs with your actual image URLs

## 🤝 Contributing Your Examples

Have a great card example? Consider:
1. Following the naming convention (##-descriptive-name.json)
2. Adding comments to explain complex sections
3. Including realistic data
4. Testing across platforms

---

**Happy Card Building!** 🎉

For more information, refer to the main [Adaptive Cards Guide](../ADAPTIVE_CARDS_GUIDE.md)
