---
description: Validates Adaptive Card JSON files against schema requirements, ensuring compliance with Teams integration standards and repository conventions before committing or deploying.
tools:
  - read_file
  - grep_search
  - file_search
---

# Validate Adaptive Card Skill

This skill validates Adaptive Card JSON files to ensure they meet all requirements for Teams integration and repository standards.

## When to Use

Invoke this skill when:
- Creating or editing Adaptive Card JSON files
- Before committing card changes
- Testing card schema compliance
- Debugging card display issues
- Preparing cards for Teams deployment

## Validation Checks

### 1. Required Fields
- ✅ `$schema`: Must be present and point to adaptive card schema
- ✅ `type`: Must be "AdaptiveCard"
- ✅ `version`: Must be "1.4" or "1.5" (Teams requirement)
- ✅ `body`: Must be present and be a non-empty array

### 2. JSON Syntax
- Valid JSON format (no trailing commas, proper quotes)
- Proper indentation (2 spaces)
- No syntax errors

### 3. Teams Integration Compatibility
- For integration scripts: Verify Teams message wrapper format
- Check PowerShell conversion depth (must use `-Depth 20`)
- Validate webhook URL format in scripts

### 4. Repository Conventions
- File naming: `01-descriptive-name.json` (numbered, kebab-case)
- Location: Proper directory (basic/intermediate/advanced)
- Accessibility: Images have `altText` property

### 5. Best Practices
- TextBlocks with long content have `wrap: true`
- Containers use appropriate `spacing` and `separator`
- Actions have descriptive `title` properties
- No hardcoded sensitive data (URLs, tokens, etc.)

## Validation Workflow

1. **Locate the file** to validate
2. **Parse JSON** and check syntax
3. **Verify required fields** exist and have correct values
4. **Check version compatibility** (>= 1.4)
5. **Validate element structure**:
   - All `type` properties are valid Adaptive Card elements
   - Required properties for each element type are present
   - Images have `url` and `altText`
   - Actions have `type` and `title`
6. **Review for best practices**:
   - Long text has `wrap: true`
   - Proper spacing between elements
   - Accessibility considerations
7. **Check integration code** (if applicable):
   - PowerShell uses `-Depth 20`
   - Teams wrapper format is correct
   - Error handling is present

## Output Format

Provide validation results as:

```markdown
## Validation Results: [filename]

### ✅ Passed Checks
- Valid JSON syntax
- Required fields present
- Version 1.5 (compatible)
- 12 elements validated

### ⚠️ Warnings
- Line 45: TextBlock missing `wrap: true` for long content
- Line 78: Image missing `altText` (accessibility)

### ❌ Critical Issues
None

### Recommendations
1. Add `wrap: true` to TextBlock at line 45
2. Add descriptive `altText` to image for screen readers
```

## Example Usage

**User**: "Validate the new approval workflow card"

**Agent Response**:
1. Reads `examples/advanced/01-approval-workflow.json`
2. Parses JSON structure
3. Checks all validation criteria
4. Reports findings with specific line numbers
5. Suggests fixes for any issues found

## Common Issues and Fixes

### Missing Schema
```json
// ❌ Missing
{
  "type": "AdaptiveCard",
  ...
}

// ✅ Correct
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  ...
}
```

### Wrong Version
```json
// ❌ Too old
"version": "1.0"

// ✅ Teams compatible
"version": "1.5"
```

### PowerShell Depth Issue
```powershell
# ❌ Will truncate nested objects
ConvertTo-Json

# ✅ Handles full depth
ConvertTo-Json -Depth 20
```

### Missing Teams Wrapper
```python
# ❌ Sends card directly (won't work in Teams)
requests.post(webhook_url, json=card)

# ✅ Wrapped in Teams message format
message = {
    "type": "message",
    "attachments": [{
        "contentType": "application/vnd.microsoft.card.adaptive",
        "content": card
    }]
}
requests.post(webhook_url, json=message)
```

## Integration with Workflow

This skill should be automatically invoked:
- When user asks to "validate", "check", or "test" a card
- Before committing card changes
- When debugging card display issues
- As part of the card creation workflow

## Related Skills

- **card-generator**: Creates new cards with validation built-in
- **send-to-teams**: Tests validated cards in real Teams environment

---

*Use this skill to ensure every Adaptive Card meets Teams requirements and repository standards.*
