---
title: Webhooks
description: Configure webhooks to receive real-time notifications about compatibility changes and updates.
description: Upcoming webhook system for event notifications.
---

# Webhooks

**Status:** Coming Soon

Webhooks will allow you to receive real-time notifications about compatibility changes, data updates, and other events in DevRadar.

## Planned Events

### compatibility.updated

Fired when compatibility data between two technologies changes.

```json
{
  "event": "compatibility.updated",
  "timestamp": "2025-01-15T10:30:00Z",
  "data": {
    "techA": "nextjs",
    "techB": "prisma",
    "oldStatus": "partial",
    "newStatus": "compatible",
    "reason": "Prisma 5.0 added Edge Runtime support"
  }
}
```

### technology.added

Fired when a new technology is added to the database.

```json
{
  "event": "technology.added",
  "timestamp": "2025-01-15T10:30:00Z",
  "data": {
    "id": "windsurf",
    "name": "Windsurf",
    "category": "ai-ide",
    "url": "https://devradar.dev/tools/windsurf"
  }
}
```

### content.published

Fired when new content is published (guides, reviews, comparisons, insights).

```json
{
  "event": "content.published",
  "timestamp": "2025-01-15T10:30:00Z",
  "data": {
    "type": "review",
    "slug": "cursor-2-review",
    "title": "Cursor 2 Deep Dive",
    "url": "https://devradar.dev/reviews/cursor-2-review",
    "tags": ["cursor", "ai-ide"]
  }
}
```

## Webhook Configuration

### Creating a Webhook

**Planned UI flow:**

1. Navigate to Settings → Webhooks
2. Click "Add Webhook"
3. Enter your endpoint URL
4. Select events to subscribe to
5. Optionally add a secret for signature verification

### Webhook Object

```json
{
  "id": "wh_123abc",
  "url": "https://your-app.com/webhooks/devradar",
  "events": ["compatibility.updated", "technology.added"],
  "secret": "whsec_your_secret_here",
  "active": true,
  "createdAt": "2025-01-15T10:00:00Z"
}
```

## Security

### Signature Verification

Each webhook POST includes a signature header:

```
X-DevRadar-Signature: t=1715757600,v1=abc123...
```

**Verification (Node.js):**

```javascript
import crypto from 'crypto';

function verifySignature(payload, signature, secret) {
  const [t, v1] = signature.split(',');
  const timestamp = t.split('=')[1];
  const signatureHash = v1.split('=')[1];

  // Check timestamp (prevent replay attacks)
  const now = Math.floor(Date.now() / 1000);
  if (now - parseInt(timestamp) > 300) { // 5 minutes
    return false;
  }

  // Verify signature
  const payloadString = JSON.stringify(payload);
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(`${timestamp}.${payloadString}`)
    .digest('hex');

  return crypto.timingSafeEqual(
    Buffer.from(signatureHash),
    Buffer.from(expectedSignature)
  );
}
```

**Verification (Python):**

```python
import hmac
import hashlib
import time
import json

def verify_signature(payload, signature, secret):
    # Parse signature
    parts = signature.split(',')
    t_part = next(p for p in parts if p.startswith('t='))
    v1_part = next(p for p in parts if p.startswith('v1='))

    timestamp = int(t_part.split('=')[1])
    signature_hash = v1_part.split('=')[1]

    # Check timestamp (prevent replay attacks)
    if time.time() - timestamp > 300:  # 5 minutes
        return False

    # Verify signature
    payload_string = json.dumps(payload)
    expected = hmac.new(
        secret.encode(),
        f"{timestamp}.{payload_string}".encode(),
        hashlib.sha256
    ).hexdigest()

    return hmac.compare_digest(signature_hash, expected)
```

### Best Practices

1. **Always verify signatures** - Prevent fake webhook deliveries
2. **Check timestamps** - Prevent replay attacks
3. **Use HTTPS** - Ensure your endpoint is secure
4. **Return 200 quickly** - Acknowledge receipt before processing

## Delivery & Retries

### Delivery Behavior

| Scenario | Behavior |
|----------|----------|
| **Success (2xx)** | Webhook marked as delivered |
| **Failure (5xx)** | Retry with exponential backoff |
| **Failure (4xx)** | Disable webhook after 3 failures |

### Retry Schedule

| Attempt | Delay |
|---------|-------|
| 1 | Immediate |
| 2 | 1 minute |
| 3 | 5 minutes |
| 4 | 30 minutes |
| 5 | 2 hours |

After 5 failed attempts, the webhook is disabled and a notification is sent.

## Example Implementation

### Express.js Endpoint

```javascript
import express from 'express';
import crypto from 'crypto';

const app = express();
const WEBHOOK_SECRET = process.env.DEVRADAR_WEBHOOK_SECRET;

app.post('/webhooks/devradar', express.raw({type: 'application/json'}), (req, res) => {
  const signature = req.headers['x-devradar-signature'];

  // Verify signature
  const payload = req.body;
  if (!verifySignature(payload, signature, WEBHOOK_SECRET)) {
    return res.status(401).send('Invalid signature');
  }

  // Parse event
  const event = JSON.parse(payload.toString());

  // Handle events
  switch (event.event) {
    case 'compatibility.updated':
      handleCompatibilityUpdate(event.data);
      break;
    case 'technology.added':
      handleTechnologyAdded(event.data);
      break;
    case 'content.published':
      handleContentPublished(event.data);
      break;
  }

  // Acknowledge receipt
  res.status(200).send('OK');
});

function handleCompatibilityUpdate(data) {
  // Your handling logic
  console.log(`Compatibility updated: ${data.techA} + ${data.techB}`);
}

app.listen(3000);
```

### Python Flask Endpoint

```python
from flask import Flask, request, jsonify
import hmac
import hashlib

app = Flask(__name__)
WEBHOOK_SECRET = 'your_webhook_secret'

@app.post('/webhooks/devradar')
def webhook():
    signature = request.headers.get('X-DevRadar-Signature')
    payload = request.get_data()

    # Verify signature
    if not verify_signature(payload, signature, WEBHOOK_SECRET):
        return jsonify({'error': 'Invalid signature'}), 401

    event = request.get_json()

    # Handle events
    if event['event'] == 'compatibility.updated':
        handle_compatibility_update(event['data'])
    elif event['event'] == 'technology.added':
        handle_technology_added(event['data'])

    return jsonify({'status': 'ok'}), 200

def handle_compatibility_update(data):
    print(f"Compatibility updated: {data['techA']} + {data['techB']}")

if __name__ == '__main__':
    app.run(port=3000)
```

## Stay Informed

Webhooks are under active development. To be notified when they launch:

1. **Watch the repository** - [github.com/devradar-dev/website](https://github.com/devradar-dev/website)
2. **Join the waitlist** - [devradar.dev/webhooks-waitlist](https://devradar.dev/webhooks-waitlist)
3. **Follow us** - [@DevRadar](https://x.com/devradardev) on Twitter

## What's Next

- [API Reference](/docs/api/overview) - REST API docs
- [GitHub Integration](/docs/integrations/github) - GitHub App
- [CLI Integration](/docs/integrations/cli) - Command-line tool

---

**Stay updated:** [Join the waitlist](https://devradar.dev/webhooks-waitlist) →
