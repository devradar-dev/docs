---
title: Rate Limits
description: API rate limiting policies and best practices. Stay within limits and handle rate limit errors gracefully.
---

# Rate Limits

All DevRadar API endpoints are rate-limited to ensure fair usage and system stability.

## Rate Limit Tiers

| Endpoint | Limit | Window | Per |
|----------|-------|--------|-----|
| `POST /api/v1/check` | 60 | 1 minute | IP address |
| `POST /api/v1/scan` | 10 | 1 minute | IP address |
| `GET /api/v1/badge` | 120 | 1 minute | IP address |

## Rate Limit Headers

Every API response includes rate limit information:

| Header | Description |
|--------|-------------|
| `X-RateLimit-Remaining` | Requests remaining in current window |
| `X-RateLimit-Reset` | ISO 8601 timestamp when window resets |
| `Retry-After` | Seconds until retry is allowed (429 responses only) |

### Example Headers

```
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 2025-01-15T10:31:00Z
```

## Rate Limit Error Response

When limits are exceeded, you'll receive a `429` status code:

```json
{
  "version": "1.0",
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Please try again later.",
    "details": "Reset at 2025-01-15T10:31:00Z"
  }
}
```

## Handling Rate Limits

### Best Practices

1. **Check Headers** - Always inspect rate limit headers
2. **Exponential Backoff** - Implement retry with increasing delays
3. **Cache Responses** - Store results to avoid repeated requests
4. **Batch Requests** - Combine checks when possible

### JavaScript Example

```javascript
async function checkWithRetry(techA, techB, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    const response = await fetch('https://devradar.dev/api/v1/check', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ techA, techB })
    });

    // Success - return data
    if (response.ok) {
      const remaining = response.headers.get('X-RateLimit-Remaining');
      if (parseInt(remaining) < 10) {
        console.warn('Low rate limit remaining:', remaining);
      }
      return await response.json();
    }

    // Rate limited - wait and retry
    if (response.status === 429) {
      const resetAt = response.headers.get('X-RateLimit-Reset');
      const resetDate = new Date(resetAt);
      const waitMs = resetDate - Date.now() + 1000;

      console.log(`Rate limited. Waiting ${waitMs}ms...`);
      await new Promise(resolve => setTimeout(resolve, waitMs));
      continue;
    }

    // Other error - don't retry
    throw new Error(`Request failed: ${response.status}`);
  }

  throw new Error('Max retries exceeded');
}
```

### Python Example

```python
import time
import requests
from datetime import datetime

def check_with_retry(tech_a, tech_b, max_retries=3):
    for i in range(max_retries):
        response = requests.post(
            'https://devradar.dev/api/v1/check',
            json={'techA': tech_a, 'techB': tech_b}
        )

        # Success - return data
        if response.ok:
            remaining = response.headers.get('X-RateLimit-Remaining')
            if int(remaining) < 10:
                print(f"Warning: Low rate limit remaining: {remaining}")
            return response.json()

        # Rate limited - wait and retry
        if response.status_code == 429:
            reset_at = response.headers.get('X-RateLimit-Reset')
            reset_date = datetime.fromisoformat(reset_at.replace('Z', '+00:00'))
            wait_seconds = (reset_date - datetime.now()).total_seconds() + 1

            print(f"Rate limited. Waiting {wait_seconds}s...")
            time.sleep(wait_seconds)
            continue

        # Other error - don't retry
        response.raise_for_status()

    raise Exception('Max retries exceeded')
```

## Caching Strategy

To minimize API usage, implement caching:

### Client-Side Cache (JavaScript)

```javascript
const cache = new Map();
const CACHE_TTL = 3600000; // 1 hour

function getCacheKey(techA, techB) {
  return `${techA}-${techB}`.toLowerCase();
}

async function checkWithCache(techA, techB) {
  const key = getCacheKey(techA, techB);
  const cached = cache.get(key);

  if (cached && Date.now() - cached.timestamp < CACHE_TTL) {
    return cached.data;
  }

  const data = await checkWithRetry(techA, techB);
  cache.set(key, { data, timestamp: Date.now() });
  return data;
}
```

### Server-Side Cache Recommendations

| Endpoint | Cache Duration |
|----------|----------------|
| `/check` | 1 hour |
| `/scan` | 5 minutes |
| `/badge` | 24 hours (built-in) |

## Increasing Rate Limits

Need higher limits for your application?

### Options

1. **Implement Caching** - Reduces requests by 90%+
2. **Use Badges** - Badges have higher limits and built-in caching
3. **Contact Us** - Enterprise plans available with dedicated limits

### Enterprise Inquiries

For production applications requiring higher throughput:
- Email: support@devradar.dev
- Subject: "API Rate Limit Request"
- Include: Use case, expected volume, timeframe

## Rate Limit Calculations

### Check Endpoint

- **60 requests/minute** = **3,600 requests/hour**
- With caching (90% hit rate): ~36,000 checks/hour effective

### Scan Endpoint

- **10 requests/minute** = **600 requests/hour**
- Intended for periodic project analysis, not real-time use

### Badge Endpoint

- **120 requests/minute** = **7,200 requests/hour**
- Badges are cached, so actual serving capacity is much higher

## Monitoring Your Usage

Track these metrics to stay within limits:

| Metric | How to Track |
|--------|--------------|
| Requests per minute | `X-RateLimit-Remaining` header |
| Reset time | `X-RateLimit-Reset` header |
| 429 errors | Monitor for rate limit responses |

### Monitoring Script

```javascript
let rateLimitStatus = {
  remaining: null,
  resetAt: null,
  last429: null
};

function trackRateLimit(response) {
  rateLimitStatus.remaining = response.headers.get('X-RateLimit-Remaining');
  rateLimitStatus.resetAt = response.headers.get('X-RateLimit-Reset');

  if (response.status === 429) {
    rateLimitStatus.last429 = new Date();
  }

  if (rateLimitStatus.remaining < 10) {
    console.warn('Approaching rate limit:', rateLimitStatus);
  }
}
```

## What's Next

- [Check Endpoint](/docs/api/check-endpoint) - Compatibility checking
- [Errors](/docs/api/errors) - Error handling guide
- [API Overview](/docs/api/overview) - API introduction

---

**Questions about rate limits?** [Contact Support](mailto:support@devradar.dev) →
