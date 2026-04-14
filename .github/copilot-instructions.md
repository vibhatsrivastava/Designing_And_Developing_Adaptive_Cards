# Adaptive Cards Learning Repository - AI Agent Instructions

This repository is a **self-learning practical guide** for Microsoft Adaptive Cards, containing documentation, examples, and integration code for sending cards to Microsoft Teams.

## Repository Purpose

Educational resource teaching Adaptive Cards from beginner to advanced levels through:
- Comprehensive guides and quick references
- 9+ working JSON examples organized by difficulty
- PowerShell and Python integration code
- **Power Automate workflows** (basic, intermediate, advanced)
- Hands-on exercises with solutions

## Key Conventions

### File Organization

```
├── Documentation (Markdown)
│   ├── QUICK_START.md          # 5-minute getting started
│   ├── ADAPTIVE_CARDS_GUIDE.md # 100+ page comprehensive guide
│   ├── CHEAT_SHEET.md          # Quick reference for all elements
│   ├── INDEX.md                # Complete navigation index
│   └── EXERCISES.md            # Practice exercises
│
├── examples/                    # JSON card definitions
│   ├── basic/                  # Beginner (01-03)
│   ├── intermediate/           # Intermediate (01-03)
│   └── advanced/               # Advanced (01-03)
│
├── teams-integration/          # Code for sending cards
│   ├── powershell/             # PowerShell scripts
│   └── python/                 # Python scripts
│
└── power-automate/             # No-code workflow integration
    ├── basic/                  # Basic flows (free tier)
    ├── intermediate/           # Intermediate flows (free + premium)
    ├── advanced/               # Advanced flows (premium)
    ├── tutorials/              # Step-by-step guides
    └── flow-exports/           # Importable flow packages (.zip)
```

### Adaptive Card Structure

All cards follow this schema:
```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [ /* elements */ ],
  "actions": [ /* optional actions */ ]
}
```

**Version**: Use `1.4` or `1.5` (Teams supports both)

### Naming Conventions

- **Examples**: `01-descriptive-name.json` (numbered, kebab-case)
- **Scripts**: `snake_case.py` (Python), `kebab-case.ps1` (PowerShell)
- **Documentation**: `UPPER_SNAKE_CASE.md`
- **Flow exports**: `difficulty-flow-name.zip` (e.g., `basic-scheduled-card.zip`)
- **Tutorials**: `descriptive-flow-name.md` (e.g., `feedback-form-flow.md`)

### Documentation Style

- **Emojis**: Use liberally for visual scanning (📚 🎯 ✅ ❌ 🚀 etc.)
- **Difficulty indicators**: ⭐ (easy) to ⭐⭐⭐⭐⭐ (expert)
- **Code blocks**: Always specify language for syntax highlighting
- **Headings**: Use `##` for main sections, `###` for subsections
- **Cross-references**: Link to related files/examples

## Common Tasks

### Creating New Adaptive Cards

1. **Choose appropriate difficulty level** (basic/intermediate/advanced)
2. **Number sequentially** within that level (e.g., `04-new-card.json`)
3. **Include all required fields**:
   - `$schema`, `type`, `version`
   - At least one element in `body` array
   - Valid JSON (use JSON validator)
4. **Add descriptive comments** in accompanying README
5. **Test in Designer**: https://adaptivecards.microsoft.com/designer

**Common elements**:
- `TextBlock`: Text with styling
- `Image`: Images with size/alignment
- `FactSet`: Key-value pairs
- `Container`: Group elements with styling
- `ColumnSet`/`Column`: Multi-column layouts
- `Input.*`: Form inputs (Text, Number, Date, Toggle, ChoiceSet)
- `Action.*`: Buttons (OpenUrl, Submit, ShowCard)

### Updating Documentation

- **QUICK_START.md**: Getting started in 5 minutes
- **CHEAT_SHEET.md**: Quick reference for all elements/patterns
- **ADAPTIVE_CARDS_GUIDE.md**: Comprehensive deep-dive
- **EXERCISES.md**: Practice problems with learning goals
- **INDEX.md**: Navigation hub linking all resources

**Maintain consistency**:
- Keep INDEX.md updated when adding examples
- Update example counts and file lists
- Include "Learning Goals" for exercises
- Provide both PowerShell and Python code samples when showing integration

### Integration Code Patterns

**PowerShell**:
```powershell
$card = @{ /* hashtable */ } | ConvertTo-Json -Depth 20
Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $card -ContentType "application/json"
```

**Python**:
```python
message = {"type": "message", "attachments": [{"contentType": "application/vnd.microsoft.card.adaptive", "content": card}]}
requests.post(webhook_url, json=message)
```

**Teams webhook format**: Always wrap Adaptive Card in Teams message envelope:
```json
{
  "type": "message",
  "attachments": [{
    "contentType": "application/vnd.microsoft.card.adaptive",
    "content": { /* actual adaptive card */ }
  }

**Power Automate**:
- **Free tier flows**: HTTP action to webhook, Recurrence trigger, Manual trigger, built-in Approval action
- **Premium flows** (💎): Teams connector (Post card, Update message), SharePoint connector, SQL connector
- **Dynamic content**: Use Power Automate expressions `@{expression}` for dynamic values
- **Example**:
  ```json
  {
    "type": "TextBlock",
    "text": "Created on @{formatDateTime(utcNow(), 'yyyy-MM-dd')}"
  }
  ```
- **Error handling**: Wrap critical actions in "Scope", add "Configure run after" for error branches
- **Environment variables**: Store webhook URLs and API keys in environment variables, not hardcoded

### Creating Power Automate Flows

1. **Choose appropriate difficulty level** (basic/intermediate/advanced)
2. **Mark premium features** with 💎 icon clearly
3. **Include complete flow configuration**:
   - Trigger settings (frequency, parameters, schema)
   - All actions with detailed configuration
   - Dynamic content expressions
   - Error handling
4. **Provide both versions** when possible:
   - Free tier version (accessible to all learners)
   - Premium enhanced version (document what Premium adds)
5. **Document prerequisites**:
   - Required licenses (Free / Premium)
   - SharePoint lists setup (column names, types)
   - SQL tables schema (if applicable)
   - Environment variables needed

**Flow Documentation Structure**:
```markdown
### Flow Name

**Difficulty**: ⭐⭐⭐
**Premium**: 💎 Yes/No
**Triggers**: List triggers
**Actions**: List actions
**Use Cases**: Real-world scenarios

#### Flow Steps
### Power Automate Flow Documentation

- **power-automate/basic/README.md**: Basic flows with free-tier connectors only
- **power-automate/intermediate/README.md**: Mix of free and premium, clearly marked
- **power-automate/advanced/README.md**: Enterprise flows, mostly premium
- **power-automate/tutorials/*.md**: Step-by-step guides with screenshots for specific flows
- **power-automate/flow-exports/*.zip**: Packaged flows for direct import into Power Automate

**Maintain consistency**:
- Always mark premium features with 💎 icon
- Include power expressions in code blocks with proper formatting
- Provide alternative free-tier approaches when premium used
- Link flows to corresponding card examples in examples/ directory
- Specify SharePoint list/SQL table schemas needed
- Include error handling and retry logic in advanced flows

---

*Last updated: 2026-04-14
#### Complete Configuration
- Step 1: Detailed settings
- Step 2: JSON/expressions

#### Premium Enhancement (if applicable)
- What changes with Premium
- Additional features enabled
```]
}
```

## Anti-Patterns

❌ **Don't**:
- Create cards without the `$schema` field (breaks validation)
- Use version < 1.4 (outdated for Teams)
- Exceed `-Depth 20` in PowerShell JSON conversion (causes truncation)
- Mix display-only content with forms (separate concerns)
- Use `Action.Submit` without explaining backend requirements
- Create examples without testing in Designer first
- Duplicate documentation (link instead)

✅ **Do**:
- Validate all JSON before committing
- Test cards in both light and dark Teams themes
- Include `wrap: true` for text that may be long
- Add `altText` for images (accessibility)
- Use `spacing` and `separator` for visual hierarchy
- Provide complete, runnable code samples
- Keep JSON clean (no trailing commas, proper indentation)

## Reference Links

- **Adaptive Cards Designer**: https://adaptivecards.microsoft.com/designer
- **Schema Explorer**: https://adaptivecards.io/explorer/
- **Teams Documentation**: https://learn.microsoft.com/adaptive-cards/
- **Schema Versions**: https://adaptivecards.io/schemas/

## Testing Workflow

1. **JSON Validation**: Use VS Code JSON schema validation
2. **Visual Testing**: Paste in Designer, preview in Teams theme
3. **Integration Testing**: Send to real Teams webhook
4. **Cross-platform**: Verify in Teams desktop, web, mobile if possible

## File-Specific Notes

- **CHEAT_SHEET.md**: Keep examples minimal (3-5 lines) for scannability
- **Examples README files**: Include screenshot or preview when possible
- **Integration scripts**: Always include error handling and success messages
- **EXERCISES.md**: Specify time estimates and learning goals

---

*Last updated: 2026-04-13*
