# Python Integration Examples

Send Adaptive Cards to Microsoft Teams using Python.

## Prerequisites

- Python 3.7 or later
- `requests` library
- Teams Incoming Webhook URL

## Installation

```bash
pip install requests
```

## Examples

### Example 1: Simple Card Sender

**File: `send_simple_card.py`**

```python
import requests
import json

def send_adaptive_card(webhook_url, card):
    """
    Send an adaptive card to Microsoft Teams via webhook.
    
    Args:
        webhook_url (str): The Teams incoming webhook URL
        card (dict): The adaptive card JSON as a dictionary
    
    Returns:
        bool: True if successful, False otherwise
    """
    # Wrap the card in Teams message format
    message = {
        "type": "message",
        "attachments": [
            {
                "contentType": "application/vnd.microsoft.card.adaptive",
                "content": card
            }
        ]
    }
    
    try:
        response = requests.post(
            webhook_url,
            headers={"Content-Type": "application/json"},
            data=json.dumps(message)
        )
        response.raise_for_status()
        print("✅ Card sent successfully!")
        return True
    except requests.exceptions.RequestException as e:
        print(f"❌ Error sending card: {e}")
        return False


# Example usage
if __name__ == "__main__":
    WEBHOOK_URL = "YOUR_WEBHOOK_URL_HERE"
    
    card = {
        "type": "AdaptiveCard",
        "version": "1.4",
        "body": [
            {
                "type": "TextBlock",
                "text": "Hello from Python! 🐍",
                "size": "Large",
                "weight": "Bolder"
            },
            {
                "type": "TextBlock",
                "text": "This card was sent using Python.",
                "wrap": True
            }
        ],
        "actions": [
            {
                "type": "Action.OpenUrl",
                "title": "Learn Python",
                "url": "https://python.org"
            }
        ]
    }
    
    send_adaptive_card(WEBHOOK_URL, card)
```

### Example 2: Send Card from JSON File

**File: `send_from_file.py`**

```python
import requests
import json
import sys
from pathlib import Path

def load_card_from_file(file_path):
    """Load an adaptive card from a JSON file."""
    try:
        with open(file_path, 'r', encoding='utf-8') as f:
            return json.load(f)
    except FileNotFoundError:
        print(f"❌ File not found: {file_path}")
        sys.exit(1)
    except json.JSONDecodeError as e:
        print(f"❌ Invalid JSON in file: {e}")
        sys.exit(1)

def send_card_to_teams(webhook_url, card):
    """Send adaptive card to Teams."""
    message = {
        "type": "message",
        "attachments": [
            {
                "contentType": "application/vnd.microsoft.card.adaptive",
                "content": card
            }
        ]
    }
    
    try:
        response = requests.post(
            webhook_url,
            headers={"Content-Type": "application/json"},
            json=message
        )
        response.raise_for_status()
        return True
    except requests.exceptions.RequestException as e:
        print(f"❌ Error: {e}")
        if hasattr(e.response, 'text'):
            print(f"Response: {e.response.text}")
        return False

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: python send_from_file.py <webhook_url> <card_file_path>")
        sys.exit(1)
    
    webhook_url = sys.argv[1]
    card_file = sys.argv[2]
    
    print(f"📤 Loading card from: {card_file}")
    card = load_card_from_file(card_file)
    
    print("📨 Sending to Teams...")
    if send_card_to_teams(webhook_url, card):
        print("✅ Success!")
    else:
        print("❌ Failed to send card")
        sys.exit(1)
```

**Usage:**
```bash
python send_from_file.py "YOUR_WEBHOOK_URL" "../../examples/basic/01-simple-welcome.json"
```

### Example 3: Build Notification System

**File: `build_notifier.py`**

```python
import requests
import json
from datetime import datetime
from enum import Enum

class BuildStatus(Enum):
    SUCCESS = ("Good", "✅", "Success")
    FAILED = ("Attention", "❌", "Failed")
    WARNING = ("Warning", "⚠️", "Warning")
    IN_PROGRESS = ("Accent", "🔄", "In Progress")

class BuildNotifier:
    def __init__(self, webhook_url):
        self.webhook_url = webhook_url
    
    def send_build_notification(self, build_number, status: BuildStatus, 
                                branch="main", duration="Unknown", 
                                build_url=None):
        """Send a build status notification to Teams."""
        
        color, icon, status_text = status.value
        
        facts = [
            {"title": "Build Number:", "value": str(build_number)},
            {"title": "Branch:", "value": branch},
            {"title": "Duration:", "value": duration},
            {"title": "Timestamp:", "value": datetime.now().strftime("%Y-%m-%d %H:%M:%S")}
        ]
        
        card = {
            "type": "AdaptiveCard",
            "version": "1.4",
            "body": [
                {
                    "type": "TextBlock",
                    "text": f"{icon} Build {status_text}",
                    "size": "Large",
                    "weight": "Bolder",
                    "color": color
                },
                {
                    "type": "FactSet",
                    "facts": facts
                }
            ]
        }
        
        # Add action if build URL is provided
        if build_url:
            card["actions"] = [
                {
                    "type": "Action.OpenUrl",
                    "title": "View Build",
                    "url": build_url
                }
            ]
        
        return self._send_card(card)
    
    def _send_card(self, card):
        """Internal method to send card to Teams."""
        message = {
            "type": "message",
            "attachments": [
                {
                    "contentType": "application/vnd.microsoft.card.adaptive",
                    "content": card
                }
            ]
        }
        
        try:
            response = requests.post(
                self.webhook_url,
                headers={"Content-Type": "application/json"},
                json=message,
                timeout=10
            )
            response.raise_for_status()
            print(f"✅ Build notification sent successfully")
            return True
        except requests.exceptions.RequestException as e:
            print(f"❌ Failed to send notification: {e}")
            return False

# Example usage
if __name__ == "__main__":
    WEBHOOK_URL = "YOUR_WEBHOOK_URL_HERE"
    
    notifier = BuildNotifier(WEBHOOK_URL)
    
    # Success notification
    notifier.send_build_notification(
        build_number=1234,
        status=BuildStatus.SUCCESS,
        branch="main",
        duration="3m 42s",
        build_url="https://dev.azure.com/your-org/_build/1234"
    )
    
    # Failed notification
    notifier.send_build_notification(
        build_number=1235,
        status=BuildStatus.FAILED,
        branch="feature/new-api",
        duration="1m 15s",
        build_url="https://dev.azure.com/your-org/_build/1235"
    )
```

### Example 4: Monitoring Alert System

**File: `monitoring_alerts.py`**

```python
import requests
import json
from datetime import datetime
from typing import Dict, List, Optional

class MonitoringAlert:
    def __init__(self, webhook_url):
        self.webhook_url = webhook_url
    
    def send_alert(self, 
                   alert_title: str,
                   alert_message: str,
                   severity: str = "info",  # info, warning, critical
                   metrics: Optional[Dict[str, str]] = None,
                   action_url: Optional[str] = None):
        """
        Send a monitoring alert to Teams.
        
        Args:
            alert_title: Title of the alert
            alert_message: Detailed alert message
            severity: Alert severity (info, warning, critical)
            metrics: Optional dictionary of metric key-value pairs
            action_url: Optional URL for action button
        """
        
        severity_config = {
            "info": {"color": "Accent", "icon": "ℹ️", "style": "default"},
            "warning": {"color": "Warning", "icon": "⚠️", "style": "warning"},
            "critical": {"color": "Attention", "icon": "🚨", "style": "attention"}
        }
        
        config = severity_config.get(severity.lower(), severity_config["info"])
        
        # Build card body
        body = [
            {
                "type": "Container",
                "style": config["style"],
                "items": [
                    {
                        "type": "TextBlock",
                        "text": f"{config['icon']} {alert_title}",
                        "size": "Large",
                        "weight": "Bolder",
                        "color": config["color"]
                    }
                ]
            },
            {
                "type": "TextBlock",
                "text": alert_message,
                "wrap": True,
                "spacing": "Medium"
            }
        ]
        
        # Add metrics if provided
        if metrics:
            facts = [{"title": k, "value": v} for k, v in metrics.items()]
            body.append({
                "type": "FactSet",
                "facts": facts,
                "spacing": "Medium",
                "separator": True
            })
        
        # Add timestamp
        body.append({
            "type": "TextBlock",
            "text": f"🕐 {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}",
            "size": "Small",
            "isSubtle": True,
            "spacing": "Medium"
        })
        
        card = {
            "type": "AdaptiveCard",
            "version": "1.4",
            "body": body
        }
        
        # Add action if URL provided
        if action_url:
            card["actions"] = [
                {
                    "type": "Action.OpenUrl",
                    "title": "View Details",
                    "url": action_url
                }
            ]
        
        return self._send_card(card)
    
    def _send_card(self, card):
        """Send the card to Teams."""
        message = {
            "type": "message",
            "attachments": [
                {
                    "contentType": "application/vnd.microsoft.card.adaptive",
                    "content": card
                }
            ]
        }
        
        try:
            response = requests.post(
                self.webhook_url,
                headers={"Content-Type": "application/json"},
                json=message,
                timeout=10
            )
            response.raise_for_status()
            return True
        except requests.exceptions.RequestException as e:
            print(f"Error sending alert: {e}")
            return False

# Example usage
if __name__ == "__main__":
    WEBHOOK_URL = "YOUR_WEBHOOK_URL_HERE"
    
    alerter = MonitoringAlert(WEBHOOK_URL)
    
    # Critical alert
    alerter.send_alert(
        alert_title="Database Connection Failed",
        alert_message="Unable to connect to primary database. Failover initiated.",
        severity="critical",
        metrics={
            "Service": "API Server",
            "Error Code": "CONN_TIMEOUT",
            "Affected Users": "~1,500",
            "Region": "US-EAST-1"
        },
        action_url="https://monitoring.example.com/incidents/123"
    )
    
    # Warning alert
    alerter.send_alert(
        alert_title="High Memory Usage",
        alert_message="Server memory usage exceeded 85% threshold.",
        severity="warning",
        metrics={
            "Server": "web-server-03",
            "Memory Usage": "87%",
            "Threshold": "85%"
        }
    )
    
    # Info alert
    alerter.send_alert(
        alert_title="Deployment Completed",
        alert_message="Successfully deployed version 2.1.0 to production.",
        severity="info",
        metrics={
            "Version": "2.1.0",
            "Environment": "Production",
            "Duration": "5m 23s"
        }
    )
```

### Example 5: Configuration Management

**File: `config_manager.py`**

```python
import os
import json
from pathlib import Path
from typing import Optional

class TeamsConfig:
    """Manage Teams webhook configuration securely."""
    
    def __init__(self):
        self.config_file = Path.home() / '.teams_config.json'
    
    def save_webhook(self, name: str, url: str):
        """Save a webhook URL with a friendly name."""
        config = self._load_config()
        config['webhooks'] = config.get('webhooks', {})
        config['webhooks'][name] = url
        self._save_config(config)
        print(f"✅ Saved webhook: {name}")
    
    def get_webhook(self, name: str) -> Optional[str]:
        """Retrieve a webhook URL by name."""
        config = self._load_config()
        return config.get('webhooks', {}).get(name)
    
    def list_webhooks(self):
        """List all saved webhook names."""
        config = self._load_config()
        webhooks = config.get('webhooks', {})
        if webhooks:
            print("Saved webhooks:")
            for name in webhooks.keys():
                print(f"  - {name}")
        else:
            print("No webhooks saved")
        return list(webhooks.keys())
    
    def delete_webhook(self, name: str):
        """Delete a saved webhook."""
        config = self._load_config()
        if 'webhooks' in config and name in config['webhooks']:
            del config['webhooks'][name]
            self._save_config(config)
            print(f"✅ Deleted webhook: {name}")
        else:
            print(f"❌ Webhook not found: {name}")
    
    def _load_config(self) -> dict:
        """Load configuration from file."""
        if self.config_file.exists():
            with open(self.config_file, 'r') as f:
                return json.load(f)
        return {}
    
    def _save_config(self, config: dict):
        """Save configuration to file."""
        with open(self.config_file, 'w') as f:
            json.dump(config, f, indent=2)
        # Make file readable only by user (Unix-like systems)
        try:
            os.chmod(self.config_file, 0o600)
        except:
            pass

# Example usage
if __name__ == "__main__":
    config = TeamsConfig()
    
    # Save webhooks
    config.save_webhook("dev-team", "https://outlook.office.com/webhook/...")
    config.save_webhook("alerts", "https://outlook.office.com/webhook/...")
    
    # List webhooks
    config.list_webhooks()
    
    # Get a webhook
    webhook = config.get_webhook("dev-team")
    if webhook:
        print(f"Dev team webhook: {webhook[:50]}...")
```

**File: `teams_sender.py`**

Using the config manager:

```python
from config_manager import TeamsConfig
import requests
import json

def send_to_channel(channel_name: str, card: dict):
    """Send a card to a named channel."""
    config = TeamsConfig()
    webhook_url = config.get_webhook(channel_name)
    
    if not webhook_url:
        print(f"❌ No webhook found for channel: {channel_name}")
        print("Available channels:")
        config.list_webhooks()
        return False
    
    message = {
        "type": "message",
        "attachments": [
            {
                "contentType": "application/vnd.microsoft.card.adaptive",
                "content": card
            }
        ]
    }
    
    try:
        response = requests.post(
            webhook_url,
            headers={"Content-Type": "application/json"},
            json=message
        )
        response.raise_for_status()
        print(f"✅ Card sent to {channel_name}")
        return True
    except requests.exceptions.RequestException as e:
        print(f"❌ Error: {e}")
        return False

# Example usage
if __name__ == "__main__":
    card = {
        "type": "AdaptiveCard",
        "version": "1.4",
        "body": [
            {
                "type": "TextBlock",
                "text": "Hello from configured channel!",
                "size": "Large"
            }
        ]
    }
    
    send_to_channel("dev-team", card)
```

## Best Practices

### 1. Environment Variables

**File: `.env`**
```
TEAMS_WEBHOOK_URL=https://outlook.office.com/webhook/...
```

**File: `load_env.py`**
```python
import os
from pathlib import Path

def load_env_file(env_file='.env'):
    """Load environment variables from .env file."""
    env_path = Path(env_file)
    if env_path.exists():
        with open(env_path) as f:
            for line in f:
                line = line.strip()
                if line and not line.startswith('#'):
                    key, value = line.split('=', 1)
                    os.environ[key] = value

# Usage
load_env_file()
webhook_url = os.getenv('TEAMS_WEBHOOK_URL')
```

### 2. Retry Logic with Exponential Backoff

```python
import time
import requests

def send_with_retry(webhook_url, message, max_retries=3):
    """Send card with retry logic."""
    for attempt in range(max_retries):
        try:
            response = requests.post(
                webhook_url,
                json=message,
                timeout=10
            )
            response.raise_for_status()
            return True
        except requests.exceptions.RequestException as e:
            wait_time = 2 ** attempt  # Exponential backoff
            if attempt < max_retries - 1:
                print(f"⏳ Retry {attempt + 1}/{max_retries} after {wait_time}s...")
                time.sleep(wait_time)
            else:
                print(f"❌ Failed after {max_retries} attempts: {e}")
                return False
```

### 3. Logging

```python
import logging

# Setup logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('teams_integration.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)

def send_card(webhook_url, card):
    """Send card with logging."""
    logger.info("Sending adaptive card to Teams")
    try:
        # ... send logic ...
        logger.info("Card sent successfully")
        return True
    except Exception as e:
        logger.error(f"Failed to send card: {e}", exc_info=True)
        return False
```

## Testing

**File: `test_integration.py`**

```python
import unittest
from unittest.mock import patch, Mock
from send_simple_card import send_adaptive_card

class TestTeamsIntegration(unittest.TestCase):
    
    @patch('requests.post')
    def test_send_card_success(self, mock_post):
        """Test successful card sending."""
        mock_response = Mock()
        mock_response.status_code = 200
        mock_post.return_value = mock_response
        
        result = send_adaptive_card("https://test.webhook", {})
        
        self.assertTrue(result)
        mock_post.assert_called_once()
    
    @patch('requests.post')
    def test_send_card_failure(self, mock_post):
        """Test failed card sending."""
        mock_post.side_effect = requests.exceptions.RequestException("Error")
        
        result = send_adaptive_card("https://test.webhook", {})
        
        self.assertFalse(result)

if __name__ == '__main__':
    unittest.main()
```

## Running as a Service

Use these examples in production with:
- **Flask/FastAPI** for webhook receivers
- **Celery** for async task queue
- **APScheduler** for scheduled tasks
- **systemd** for Linux services

## Troubleshooting

### SSL Certificate Issues
```python
import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Or install certificates
# pip install certifi
```

### Timeout Issues
```python
response = requests.post(webhook_url, json=message, timeout=30)
```

## Additional Resources

- [Requests Documentation](https://requests.readthedocs.io/)
- [Python JSON Module](https://docs.python.org/3/library/json.html)

---

**Next:** Check out [JavaScript/Node.js examples](../nodejs/) for web integration
