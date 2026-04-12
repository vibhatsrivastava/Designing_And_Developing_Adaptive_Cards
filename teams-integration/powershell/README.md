# PowerShell Integration Examples

Send Adaptive Cards to Microsoft Teams using PowerShell.

## Prerequisites

- PowerShell 5.1 or later (Windows)
- PowerShell 7+ (Cross-platform)
- Teams Incoming Webhook URL

## Setup: Create an Incoming Webhook

1. Open Microsoft Teams
2. Navigate to your target channel
3. Click "..." → "Connectors" or "Workflows"
4. Search for "Incoming Webhook"
5. Click "Configure"
6. Name your webhook (e.g., "Adaptive Cards")
7. Copy the webhook URL

## Examples

### Example 1: Simple Notification

**File: `send-simple-card.ps1`**

```powershell
# Configuration
$webhookUrl = "YOUR_WEBHOOK_URL_HERE"

# Simple Adaptive Card
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
                        text = "Hello from PowerShell! 🎉"
                        size = "Large"
                        weight = "Bolder"
                    }
                    @{
                        type = "TextBlock"
                        text = "This card was sent using PowerShell."
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

try {
    Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $card -ContentType "application/json"
    Write-Host "✅ Card sent successfully!" -ForegroundColor Green
}
catch {
    Write-Host "❌ Error: $($_.Exception.Message)" -ForegroundColor Red
}
```

### Example 2: Send Card from JSON File

**File: `send-from-file.ps1`**

```powershell
param(
    [Parameter(Mandatory=$true)]
    [string]$WebhookUrl,
    
    [Parameter(Mandatory=$true)]
    [string]$CardFilePath
)

# Read the adaptive card JSON
$cardContent = Get-Content -Path $CardFilePath -Raw | ConvertFrom-Json

# Wrap in Teams message format
$message = @{
    type = "message"
    attachments = @(
        @{
            contentType = "application/vnd.microsoft.card.adaptive"
            content = $cardContent
        }
    )
} | ConvertTo-Json -Depth 20

try {
    Invoke-RestMethod -Uri $WebhookUrl -Method Post -Body $message -ContentType "application/json"
    Write-Host "✅ Card sent successfully from $CardFilePath" -ForegroundColor Green
}
catch {
    Write-Host "❌ Failed to send card: $($_.Exception.Message)" -ForegroundColor Red
    exit 1
}
```

**Usage:**
```powershell
.\send-from-file.ps1 -WebhookUrl "YOUR_URL" -CardFilePath "..\..\examples\basic\01-simple-welcome.json"
```

### Example 3: Build Notification

**File: `send-build-notification.ps1`**

```powershell
param(
    [string]$WebhookUrl,
    [string]$BuildNumber,
    [string]$Status,  # "Success", "Failed", "Warning"
    [string]$Branch = "main",
    [string]$Duration = "Unknown"
)

# Determine color and icon based on status
$statusColor = switch ($Status) {
    "Success" { "Good"; "✅" }
    "Failed" { "Attention"; "❌" }
    "Warning" { "Warning"; "⚠️" }
    default { "Default"; "ℹ️" }
}

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
                        text = "$($statusColor[1]) Build $Status"
                        size = "Large"
                        weight = "Bolder"
                        color = $statusColor[0]
                    }
                    @{
                        type = "FactSet"
                        facts = @(
                            @{
                                title = "Build Number:"
                                value = $BuildNumber
                            }
                            @{
                                title = "Branch:"
                                value = $Branch
                            }
                            @{
                                title = "Duration:"
                                value = $Duration
                            }
                            @{
                                title = "Timestamp:"
                                value = (Get-Date -Format "yyyy-MM-dd HH:mm:ss")
                            }
                        )
                    }
                )
                actions = @(
                    @{
                        type = "Action.OpenUrl"
                        title = "View Build"
                        url = "https://dev.azure.com/your-org/your-project/_build/results?buildId=$BuildNumber"
                    }
                )
            }
        }
    )
} | ConvertTo-Json -Depth 20

try {
    Invoke-RestMethod -Uri $WebhookUrl -Method Post -Body $card -ContentType "application/json"
    Write-Host "✅ Build notification sent" -ForegroundColor Green
}
catch {
    Write-Host "❌ Error: $($_.Exception.Message)" -ForegroundColor Red
    exit 1
}
```

**Usage:**
```powershell
.\send-build-notification.ps1 `
    -WebhookUrl "YOUR_URL" `
    -BuildNumber "1234" `
    -Status "Success" `
    -Branch "main" `
    -Duration "3m 42s"
```

### Example 4: Daily Report

**File: `send-daily-report.ps1`**

```powershell
param(
    [Parameter(Mandatory=$true)]
    [string]$WebhookUrl
)

# Gather some metrics (example data)
$tasksCompleted = 12
$tasksInProgress = 5
$tasksPending = 3
$totalTasks = $tasksCompleted + $tasksInProgress + $tasksPending
$completionPercent = [math]::Round(($tasksCompleted / $totalTasks) * 100, 1)

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
                        text = "📊 Daily Team Report"
                        size = "Large"
                        weight = "Bolder"
                    }
                    @{
                        type = "TextBlock"
                        text = (Get-Date -Format "dddd, MMMM dd, yyyy")
                        isSubtle = $true
                        spacing = "None"
                    }
                    @{
                        type = "Container"
                        separator = $true
                        spacing = "Medium"
                        items = @(
                            @{
                                type = "TextBlock"
                                text = "Task Summary"
                                weight = "Bolder"
                            }
                            @{
                                type = "ColumnSet"
                                columns = @(
                                    @{
                                        type = "Column"
                                        width = "stretch"
                                        items = @(
                                            @{
                                                type = "TextBlock"
                                                text = "Completion Rate"
                                            }
                                        )
                                    }
                                    @{
                                        type = "Column"
                                        width = "auto"
                                        items = @(
                                            @{
                                                type = "TextBlock"
                                                text = "$completionPercent%"
                                                weight = "Bolder"
                                                color = "Good"
                                            }
                                        )
                                    }
                                )
                            }
                            @{
                                type = "FactSet"
                                spacing = "Small"
                                facts = @(
                                    @{
                                        title = "✅ Completed:"
                                        value = "$tasksCompleted tasks"
                                    }
                                    @{
                                        title = "🔄 In Progress:"
                                        value = "$tasksInProgress tasks"
                                    }
                                    @{
                                        title = "⏸️ Pending:"
                                        value = "$tasksPending tasks"
                                    }
                                )
                            }
                        )
                    }
                )
                actions = @(
                    @{
                        type = "Action.OpenUrl"
                        title = "View Full Report"
                        url = "https://example.com/reports/daily"
                    }
                )
            }
        }
    )
} | ConvertTo-Json -Depth 20

try {
    Invoke-RestMethod -Uri $WebhookUrl -Method Post -Body $card -ContentType "application/json"
    Write-Host "✅ Daily report sent" -ForegroundColor Green
}
catch {
    Write-Host "❌ Error: $($_.Exception.Message)" -ForegroundColor Red
    exit 1
}
```

### Example 5: Error/Alert Notification

**File: `send-alert.ps1`**

```powershell
param(
    [Parameter(Mandatory=$true)]
    [string]$WebhookUrl,
    
    [Parameter(Mandatory=$true)]
    [string]$AlertTitle,
    
    [Parameter(Mandatory=$true)]
    [string]$AlertMessage,
    
    [ValidateSet("Info", "Warning", "Critical")]
    [string]$Severity = "Info"
)

# Map severity to colors and icons
$severityMap = @{
    "Info" = @{ Color = "Accent"; Icon = "ℹ️"; Style = "default" }
    "Warning" = @{ Color = "Warning"; Icon = "⚠️"; Style = "warning" }
    "Critical" = @{ Color = "Attention"; Icon = "🚨"; Style = "attention" }
}

$config = $severityMap[$Severity]

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
                        type = "Container"
                        style = $config.Style
                        items = @(
                            @{
                                type = "TextBlock"
                                text = "$($config.Icon) $AlertTitle"
                                size = "Large"
                                weight = "Bolder"
                                color = $config.Color
                            }
                        )
                    }
                    @{
                        type = "TextBlock"
                        text = $AlertMessage
                        wrap = $true
                        spacing = "Medium"
                    }
                    @{
                        type = "FactSet"
                        facts = @(
                            @{
                                title = "Severity:"
                                value = $Severity
                            }
                            @{
                                title = "Time:"
                                value = (Get-Date -Format "yyyy-MM-dd HH:mm:ss")
                            }
                            @{
                                title = "Source:"
                                value = $env:COMPUTERNAME
                            }
                        )
                    }
                )
                actions = @(
                    @{
                        type = "Action.OpenUrl"
                        title = "View Logs"
                        url = "https://example.com/logs"
                    }
                )
            }
        }
    )
} | ConvertTo-Json -Depth 20

try {
    Invoke-RestMethod -Uri $WebhookUrl -Method Post -Body $card -ContentType "application/json"
    Write-Host "✅ Alert sent: $AlertTitle" -ForegroundColor Green
}
catch {
    Write-Host "❌ Error: $($_.Exception.Message)" -ForegroundColor Red
    exit 1
}
```

**Usage:**
```powershell
# Info alert
.\send-alert.ps1 -WebhookUrl "YOUR_URL" -AlertTitle "System Update" -AlertMessage "Scheduled maintenance completed" -Severity Info

# Warning
.\send-alert.ps1 -WebhookUrl "YOUR_URL" -AlertTitle "High CPU Usage" -AlertMessage "Server CPU at 85%" -Severity Warning

# Critical
.\send-alert.ps1 -WebhookUrl "YOUR_URL" -AlertTitle "Service Down" -AlertMessage "API endpoint not responding" -Severity Critical
```

## Best Practices

### 1. Store Webhook URL Securely
```powershell
# Don't hardcode URLs in scripts
# Instead, use environment variables or secure storage

# Option 1: Environment Variable
$webhookUrl = $env:TEAMS_WEBHOOK_URL

# Option 2: Azure Key Vault (for production)
# $webhookUrl = Get-AzKeyVaultSecret -VaultName "MyVault" -Name "TeamsWebhook" -AsPlainText

# Option 3: Encrypted file
# $webhookUrl = Get-Content "webhook.encrypted" | ConvertTo-SecureString | ConvertFrom-SecureString
```

### 2. Error Handling
```powershell
try {
    $response = Invoke-RestMethod -Uri $webhookUrl -Method Post -Body $card -ContentType "application/json"
    Write-Host "✅ Success" -ForegroundColor Green
}
catch {
    Write-Host "❌ Failed: $($_.Exception.Message)" -ForegroundColor Red
    
    # Log error details
    $errorDetails = $_.ErrorDetails.Message
    Write-Host "Details: $errorDetails" -ForegroundColor Yellow
    
    # Exit with error code for CI/CD pipelines
    exit 1
}
```

### 3. Rate Limiting
```powershell
# Avoid sending too many cards too quickly
# Teams has rate limits on webhooks

function Send-CardWithRetry {
    param($WebhookUrl, $Card, $MaxRetries = 3)
    
    $retryCount = 0
    $success = $false
    
    while (-not $success -and $retryCount -lt $MaxRetries) {
        try {
            Invoke-RestMethod -Uri $WebhookUrl -Method Post -Body $Card -ContentType "application/json"
            $success = $true
            Write-Host "✅ Sent successfully" -ForegroundColor Green
        }
        catch {
            $retryCount++
            if ($retryCount -lt $MaxRetries) {
                $waitTime = [math]::Pow(2, $retryCount)  # Exponential backoff
                Write-Host "⏳ Retry $retryCount after ${waitTime}s..." -ForegroundColor Yellow
                Start-Sleep -Seconds $waitTime
            }
            else {
                Write-Host "❌ Failed after $MaxRetries attempts" -ForegroundColor Red
                throw
            }
        }
    }
}
```

## Scheduling with Windows Task Scheduler

To send daily reports automatically:

1. Create a scheduled task:
```powershell
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-File C:\Scripts\send-daily-report.ps1 -WebhookUrl 'YOUR_URL'"
$trigger = New-ScheduledTaskTrigger -Daily -At 9am
Register-ScheduledTask -TaskName "Teams Daily Report" -Action $action -Trigger $trigger
```

## Troubleshooting

### Issue: "Invoke-RestMethod: The underlying connection was closed"
**Solution:** Check if your PowerShell version supports TLS 1.2
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

### Issue: Card doesn't display in Teams
**Solution:** Ensure proper Teams wrapper format
- Must have `type: "message"`
- Must have `attachments` array
- ContentType must be `application/vnd.microsoft.card.adaptive`

### Issue: JSON conversion depth error
**Solution:** Use `-Depth 20` parameter
```powershell
ConvertTo-Json -Depth 20
```

## Additional Resources

- [Microsoft Teams Webhooks Documentation](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook)
- [PowerShell Invoke-RestMethod](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-restmethod)

---

**Next:** Check out [Python examples](../python/) for cross-platform integration
