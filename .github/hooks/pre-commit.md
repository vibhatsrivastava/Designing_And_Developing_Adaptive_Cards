---
description: Pre-commit validation hook that automatically checks Adaptive Card JSON files for schema compliance, syntax errors, and repository conventions before allowing commits.
trigger: before_commit
applyTo: 
  - "examples/**/*.json"
  - "teams-integration/**/*.{ps1,py}"
---

# Pre-Commit Validation Hook

Automatically validates changes to Adaptive Card files before committing to ensure quality and consistency.

## Purpose

This hook prevents common issues by catching them before they enter the repository:
- Invalid JSON syntax
- Missing required schema fields
- Incompatible version numbers
- Teams integration errors
- Repository convention violations

## When This Hook Runs

Automatically triggered when:
- Committing changes to `examples/**/*.json` files
- Committing changes to integration scripts (`teams-integration/**/*.ps1`, `teams-integration/**/*.py`)
- Running manual validation before push

## Validation Checks

### For JSON Card Files (`examples/**/*.json`)

#### 1. **Syntax Validation** ✅
```bash
✓ Valid JSON syntax
✓ No trailing commas
✓ Proper quote usage
✓ Balanced brackets/braces
✓ Correct indentation (2 spaces)
```

#### 2. **Schema Compliance** ✅
```json
Required fields:
✓ "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
✓ "type": "AdaptiveCard"
✓ "version": "1.4" or "1.5"
✓ "body": [array with at least one element]
```

#### 3. **Element Validation** ✅
```bash
✓ All element types are valid
✓ Required properties present for each type
✓ Images have "url" property
✓ Actions have "type" and "title"
```

#### 4. **Repository Conventions** ✅
```bash
✓ Filename matches pattern: DD-descriptive-name.json
✓ Located in correct directory (basic/intermediate/advanced)
✓ Sequential numbering within difficulty level
✓ No duplicate filenames
```

#### 5. **Best Practices** ⚠️
```bash
⚠ Images should have "altText" (accessibility)
⚠ Long TextBlocks should have "wrap": true
⚠ Containers should use appropriate spacing
⚠ Actions should have descriptive titles
```

### For PowerShell Scripts (`teams-integration/**/*.ps1`)

#### 1. **Syntax Check** ✅
```powershell
✓ Valid PowerShell syntax
✓ No parse errors
✓ Proper cmdlet usage
```

#### 2. **Teams Integration Pattern** ✅
```powershell
✓ Uses -Depth 20 for ConvertTo-Json
✓ Includes Teams message wrapper
✓ Proper webhook URL variable
✓ Error handling (try/catch)
```

#### 3. **Code Quality** ⚠️
```powershell
⚠ Success/error messages present
⚠ ContentType is "application/json"
⚠ No hardcoded credentials
⚠ Descriptive variable names
```

### For Python Scripts (`teams-integration/**/*.py`)

#### 1. **Syntax Check** ✅
```python
✓ Valid Python syntax
✓ No import errors
✓ Proper indentation
```

#### 2. **Teams Integration Pattern** ✅
```python
✓ Imports requests library
✓ Includes Teams message wrapper
✓ Proper JSON handling
✓ Error handling (try/except)
```

#### 3. **Code Quality** ⚠️
```python
⚠ Docstrings present
⚠ Type hints used (Python 3.7+)
⚠ No hardcoded credentials
⚠ Descriptive function/variable names
```

## Validation Process

### Step 1: Identify Changed Files
```bash
# Get list of staged files
Changed files:
  - examples/intermediate/04-new-card.json
  - teams-integration/python/send_new_card.py
```

### Step 2: Run Applicable Validators

For each changed file, run appropriate validation:

**JSON Files**:
1. Parse JSON and catch syntax errors
2. Extract and verify required fields
3. Check version compatibility
4. Validate element structure
5. Check naming convention
6. Verify sequential numbering
7. Scan for accessibility features

**PowerShell Scripts**:
1. Test PowerShell syntax
2. Search for `-Depth 20` pattern
3. Verify Teams wrapper structure
4. Check error handling
5. Validate ContentType usage

**Python Scripts**:
1. Compile Python code
2. Check imports
3. Verify Teams wrapper structure
4. Check error handling
5. Validate JSON handling

### Step 3: Report Results

**If all checks pass** ✅:
```
✅ Pre-commit validation passed

Files validated:
  ✓ examples/intermediate/04-new-card.json
  ✓ teams-integration/python/send_new_card.py

All checks passed. Safe to commit.
```

**If warnings found** ⚠️:
```
⚠️ Pre-commit validation passed with warnings

examples/intermediate/04-new-card.json:
  ⚠ Line 34: Image missing "altText" (accessibility)
  ⚠ Line 48: TextBlock should have "wrap": true

Continue with commit? (y/n)
```

**If critical errors found** ❌:
```
❌ Pre-commit validation failed

examples/intermediate/04-new-card.json:
  ✗ Missing required field: "$schema"
  ✗ Invalid version: "1.0" (must be 1.4 or 1.5)

teams-integration/powershell/send-new-card.ps1:
  ✗ Missing -Depth parameter in ConvertTo-Json
  ✗ No error handling (try/catch) present

Commit blocked. Fix errors and try again.
```

## Integration Configuration

### Git Hook (Optional)
To enable automatic validation on git commit, create `.git/hooks/pre-commit`:

```bash
#!/bin/bash
# Pre-commit hook for Adaptive Cards validation

echo "Running Adaptive Cards validation..."

# Get staged JSON files
staged_json=$(git diff --cached --name-only --diff-filter=ACM | grep "examples/.*\.json$")

if [ -n "$staged_json" ]; then
    echo "Validating JSON card files..."
    # Validation logic here
fi

# Get staged scripts
staged_ps1=$(git diff --cached --name-only --diff-filter=ACM | grep "teams-integration/.*\.ps1$")
staged_py=$(git diff --cached --name-only --diff-filter=ACM | grep "teams-integration/.*\.py$")

if [ -n "$staged_ps1" ] || [ -n "$staged_py" ]; then
    echo "Validating integration scripts..."
    # Validation logic here
fi

exit 0
```

### VS Code Tasks (Recommended)

Add to `.vscode/tasks.json`:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Validate Adaptive Cards",
      "type": "shell",
      "command": "echo 'Validating cards...'",
      "problemMatcher": [],
      "group": {
        "kind": "test",
        "isDefault": true
      }
    }
  ]
}
```

## Manual Invocation

Users can manually run validation:

**Via AI Agent**:
```
"Validate all my changes before committing"
"Run pre-commit checks"
"Check if my new card is valid"
```

**Via Command**:
```bash
# Validate specific file
copilot validate examples/intermediate/04-new-card.json

# Validate all staged files
copilot validate --staged
```

## Common Issues and Auto-Fixes

### Issue: Missing Schema Field
```json
// Before (❌)
{
  "type": "AdaptiveCard",
  "version": "1.5",
  ...
}

// Auto-fix suggestion
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  ...
}
```

### Issue: PowerShell Depth Missing
```powershell
# Before (❌)
$card | ConvertTo-Json

# Auto-fix suggestion
$card | ConvertTo-Json -Depth 20
```

### Issue: Missing Teams Wrapper
```python
# Before (❌)
requests.post(webhook_url, json=card)

# Auto-fix suggestion
message = {
    "type": "message",
    "attachments": [{
        "contentType": "application/vnd.microsoft.card.adaptive",
        "content": card
    }]
}
requests.post(webhook_url, json=message)
```

## Bypass Option

For emergency commits (use sparingly):

```bash
# Skip validation (not recommended)
git commit --no-verify
```

## Error Severity Levels

### 🔴 Critical (Blocks Commit)
- Invalid JSON syntax
- Missing required schema fields
- Wrong version number
- Invalid element types
- Syntax errors in scripts

### 🟡 Warning (Allows Commit)
- Missing accessibility features
- Missing best practices
- Suboptimal patterns
- Style guide deviations

### 🟢 Info (No Action Required)
- Suggestions for improvement
- Alternative approaches
- Optimization opportunities

## Integration with CI/CD

This validation can also run in GitHub Actions:

```yaml
name: Validate Adaptive Cards

on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Validate Cards
        run: |
          # Run validation on all JSON files
          # Run validation on all scripts
```

## Customization

Adjust validation rules in `.github/validation-config.json`:

```json
{
  "jsonValidation": {
    "requireSchema": true,
    "minVersion": "1.4",
    "requireAltText": "warn",
    "requireWrap": "warn"
  },
  "scriptValidation": {
    "requireErrorHandling": true,
    "requireDepth20": true,
    "requireTeamsWrapper": true
  }
}
```

---

*This hook ensures every commit maintains the high quality standards of the Adaptive Cards repository.*
