# Practical Exercises - Adaptive Cards

Hands-on exercises to practice your Adaptive Cards skills. Start with beginner exercises and progress to advanced challenges.

## 🎯 How to Use This Guide

1. Choose an exercise based on your skill level
2. Try to build the card yourself first
3. Compare with the solution in the `/examples` folder
4. Test your card in the Designer: https://adaptivecards.microsoft.com/designer
5. Send it to Teams using the webhook method

---

## 🟢 Beginner Exercises

### Exercise 1: Welcome Card
**Difficulty**: ⭐ Easy  
**Time**: 10 minutes

**Task**: Create a welcome card for new team members

**Requirements**:
- Title: "Welcome to the Team! 👋"
- Subtitle with new member's name
- A welcoming message
- Company logo image
- Two buttons:
  - "View Team Handbook" (opens a URL)
  - "Meet the Team" (opens a URL)

**Learning Goals**:
- Basic card structure
- TextBlock elements
- Image element
- Action.OpenUrl

**Hints**:
- Use `size: "Large"` for the title
- Set `weight: "Bolder"` for emphasis
- Remember to set `wrap: true` for longer text

---

### Exercise 2: Daily Reminder
**Difficulty**: ⭐ Easy  
**Time**: 15 minutes

**Task**: Create a daily reminder card for team standup

**Requirements**:
- Header: "Daily Standup Reminder 📅"
- Current date displayed
- Time: "9:00 AM"
- Location: "Conference Room A"
- Agenda items (use FactSet):
  - Yesterday's progress
  - Today's plan
  - Blockers
- One action button: "Join Meeting"

**Learning Goals**:
- FactSet for structured data
- Container with styling
- Date/time display

**Hints**:
- Use `style: "emphasis"` for the container
- FactSet is perfect for key-value pairs

---

### Exercise 3: System Notification
**Difficulty**: ⭐⭐ Easy-Medium  
**Time**: 20 minutes

**Task**: Create a system maintenance notification

**Requirements**:
- Alert icon (⚠️) in the title
- Title: "Scheduled Maintenance"
- Description of maintenance
- FactSet with:
  - Start time
  - Duration
  - Affected services
  - Impact level
- Status indicator (use color)
- "View Details" button

**Learning Goals**:
- Using emojis in text
- Color coding (Warning, Attention)
- Container styling
- Multiple text blocks

**Hints**:
- Use `color: "Warning"` for the title
- Use `style: "warning"` for the container

---

## 🟡 Intermediate Exercises

### Exercise 4: Feedback Form
**Difficulty**: ⭐⭐ Medium  
**Time**: 30 minutes

**Task**: Create a customer satisfaction survey

**Requirements**:
- Form title
- Input fields:
  - Name (text input)
  - Email (email input)
  - Rating (choice set with 5 options)
  - Comments (multi-line text)
  - Subscribe checkbox (toggle)
- Submit button
- All fields except comments should be required

**Learning Goals**:
- Input elements
- Form validation
- Input.ChoiceSet
- Input.Toggle
- Action.Submit with data

**Hints**:
- Set `isRequired: true` for mandatory fields
- Use `isMultiline: true` for comments
- Include an `id` for each input
- Add `actionType` in the submit data

**Sample Data Structure**:
```json
{
  "data": {
    "actionType": "submitFeedback",
    "formId": "customer-satisfaction-2024"
  }
}
```

---

### Exercise 5: Task Card
**Difficulty**: ⭐⭐⭐ Medium  
**Time**: 45 minutes

**Task**: Create a task assignment card with status update

**Requirements**:
- Task header with priority badge
- Task details:
  - Title
  - Description
  - Assigned to
  - Due date
  - Priority (High/Medium/Low)
- Current status display
- Quick actions:
  - Status dropdown (Not Started, In Progress, Completed, Blocked)
  - Date picker for deadline
  - Notes field
- Three buttons:
  - Accept Task
  - Update Status
  - View in Portal

**Learning Goals**:
- Complex layouts
- Multiple input types
- ColumnSet for badges
- Combining inputs and actions

**Hints**:
- Use ColumnSet for header with badge
- Color code priority (High = Attention, Medium = Warning, Low = Default)
- Use Input.Date for deadline picker

---

### Exercise 6: Poll Card
**Difficulty**: ⭐⭐⭐ Medium  
**Time**: 40 minutes

**Task**: Create a team poll/voting card

**Requirements**:
- Poll question
- 5 voting options (expanded choice set)
- Optional "Other" text input
- Current results section showing:
  - Total votes
  - Percentage for each option
  - Visual representation (use characters like ████)
- Submit vote button
- View results button

**Learning Goals**:
- Input.ChoiceSet with expanded style
- FactSet for results
- Separator usage
- ShowCard action (bonus)

**Hints**:
- Use `style: "expanded"` for choice set
- Use FactSet for results display
- Consider using a Container with separator

---

## 🔴 Advanced Exercises

### Exercise 7: Approval Workflow
**Difficulty**: ⭐⭐⭐⭐ Hard  
**Time**: 60 minutes

**Task**: Create a purchase order approval card

**Requirements**:
- Header with PO number and status
- Requester information (with avatar)
- Item details section:
  - Item name, quantity, price (in a table-like layout)
  - Multiple items
  - Total amount
- Business justification section
- Approval history (previous approvers)
- Decision inputs:
  - Approve/Reject choice set
  - Comments field
  - Notify requester toggle
- Three action buttons:
  - Approve (positive style)
  - Reject (destructive style)
  - Show attachments (ShowCard)

**Learning Goals**:
- Complex multi-section layouts
- ColumnSet for tabular data
- Action.ShowCard
- Multiple containers with styling
- Historical data display

**Hints**:
- Use `style: "emphasis"` for the header container
- Create item rows using ColumnSet
- Use `bleed: true` for full-width containers
- Add separator between sections

---

### Exercise 8: Dashboard Card
**Difficulty**: ⭐⭐⭐⭐ Hard  
**Time**: 60 minutes

**Task**: Create a performance dashboard card

**Requirements**:
- Dashboard title with date range
- Three KPI cards (in columns):
  - Revenue (with percentage change)
  - New customers (with trend)
  - Churn rate (with indicator)
- Department performance section:
  - 4 departments
  - Progress bars (using text characters)
  - Percentage for each
- Key achievements list
- Nested card for feedback (ShowCard action)

**Learning Goals**:
- Multi-column layouts
- Visual data representation
- Color coding for metrics
- Nested cards
- Complex container organization

**Hints**:
- Use ColumnSet with 3 equal columns for KPIs
- Use colored TextBlocks for trend indicators (↑ ↓)
- Create progress bars with repeated characters: `████████░░`
- Use `color: "Good"` for positive metrics

---

### Exercise 9: Incident Report Card
**Difficulty**: ⭐⭐⭐⭐⭐ Very Hard  
**Time**: 90 minutes

**Task**: Create a critical incident management card

**Requirements**:
- Attention-grabbing header (red/critical style)
- Incident ID and severity badge
- Incident details:
  - Title
  - Description
  - Impact statement
- Technical details (FactSet):
  - Affected systems
  - Error codes
  - Region
  - Environment
- Timeline section (chronological updates)
- Current status metrics:
  - Duration
  - Users impacted
  - Services affected
- Communication tracking:
  - Status page updated
  - Customer emails sent
  - Internal notifications
- Action inputs:
  - Action dropdown (Join war room, Post update, Escalate, etc.)
  - Update notes field
- Multiple styled buttons:
  - Join War Room (positive)
  - Post Update (default)
  - Escalate (destructive)
  - View Dashboard (default)

**Learning Goals**:
- Master all card elements
- Complex styling and theming
- Multi-section organization
- Professional incident card design
- Advanced FactSet usage

**Hints**:
- Use `style: "attention"` for critical header
- Use `bleed: true` for dramatic effect
- Organize timeline with small TextBlocks
- Color code severity levels
- Make it dense but readable

---

## 🎓 Challenge Projects

### Challenge 1: Complete Bot Integration
Create a full Teams bot that:
- Sends welcome cards to new members
- Handles form submissions
- Updates cards based on user actions
- Stores data in a database

### Challenge 2: Template System
Build a template system that:
- Generates cards from data
- Supports multiple card types
- Has a web UI for card creation
- Exports to JSON

### Challenge 3: Dashboard Automation
Create an automated dashboard that:
- Fetches data from APIs
- Generates daily report cards
- Sends to Teams on schedule
- Includes charts and metrics

---

## ✅ Solution Check

For each exercise, verify:
- ✅ Card validates in the Designer
- ✅ Displays correctly in Teams (light and dark mode)
- ✅ All inputs have unique IDs
- ✅ Required fields are marked
- ✅ Actions have proper data structures
- ✅ Text wraps appropriately
- ✅ Images have alt text
- ✅ Mobile-friendly layout

## 📊 Track Your Progress

| Exercise | Completed | Tested in Teams | Notes |
|----------|-----------|-----------------|-------|
| E1: Welcome Card | ☐ | ☐ | |
| E2: Daily Reminder | ☐ | ☐ | |
| E3: System Notification | ☐ | ☐ | |
| E4: Feedback Form | ☐ | ☐ | |
| E5: Task Card | ☐ | ☐ | |
| E6: Poll Card | ☐ | ☐ | |
| E7: Approval Workflow | ☐ | ☐ | |
| E8: Dashboard Card | ☐ | ☐ | |
| E9: Incident Report | ☐ | ☐ | |

## 💡 Tips for Success

1. **Start Simple**: Don't try to build everything at once
2. **Use the Designer**: Build visually, then copy JSON
3. **Test Frequently**: Check your card after each major addition
4. **Learn from Examples**: Study the sample cards in `/examples`
5. **Iterate**: Your first version doesn't have to be perfect
6. **Get Feedback**: Send to Teams and ask colleagues what they think

## 🐛 Common Issues While Practicing

**Issue**: Card doesn't show in Teams
- Check JSON validity at jsonlint.com
- Verify Teams wrapper is correct
- Confirm version is 1.4 or 1.5

**Issue**: Inputs not appearing
- Ensure each has a unique `id`
- Check input type spelling (Input.Text, not input.text)

**Issue**: Layout looks wrong
- Test on different screen sizes
- Use `wrap: true` for text
- Avoid fixed widths when possible

**Issue**: Submit action doesn't work
- Webhooks don't support Action.Submit
- Need a bot for interactive submissions
- Use Action.OpenUrl as alternative

---

## 🎉 When You're Done

After completing these exercises, you'll be able to:
- ✅ Design adaptive cards from scratch
- ✅ Handle complex layouts
- ✅ Create interactive forms
- ✅ Build professional workflows
- ✅ Integrate with Teams
- ✅ Solve real-world problems with cards

**Next**: Build real cards for your actual use cases!

---

**Good luck and happy card building!** 🚀

Need help? Check:
- [Complete Guide](ADAPTIVE_CARDS_GUIDE.md)
- [Cheat Sheet](CHEAT_SHEET.md)
- [Example Cards](examples/)
