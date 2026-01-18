---
title: Check Endpoint
description: Verify compatibility of two technologies. Returns status, severity, message, and workarounds.
---

# Check Endpoint

Verify whether two technologies are compatible before committing to your tech stack.

## Endpoint

```
POST /api/v1/check
```

## Request

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `Content-Type` | string | Yes | Must be `application/json` |

### Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `techA` | string | Yes | First technology name |
| `techB` | string | Yes | Second technology name |

### Example Request

```json
{
  "techA": "nextjs",
  "techB": "prisma"
}
```

## Response

### Success Response (200)

```json
{
  "version": "1.0",
  "status": "compatible",
  "severity": "info",
  "message": "Next.js works seamlessly with Prisma ORM for server-side rendering and API routes.",
  "workaround": null,
  "docsUrl": "https://devradar.dev/check/nextjs/prisma",
  "lastUpdated": "2025-01-15T10:30:00Z"
}
```

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `version` | string | API version (always "1.0") |
| `status` | string | `compatible`, `partial`, `incompatible`, `deprecated`, `unknown` |
| `severity` | string | `info`, `warning`, `error`, `critical` (optional) |
| `message` | string | Human-readable compatibility description |
| `workaround` | string | null | Suggested fix for issues |
| `docsUrl` | string | URL to full compatibility documentation |
| `lastUpdated` | string | ISO 8601 timestamp of last data update |

### Status Values

| Status | Meaning |
|--------|---------|
| `compatible` | Works seamlessly |
| `partial` | Works with limitations or configuration |
| `incompatible` | Fundamental conflicts exist |
| `deprecated` | One or both technologies are end-of-life |
| `unknown` | No compatibility data available |

## Code Examples

### cURL

```bash
curl -X POST https://devradar.dev/api/v1/check \
  -H "Content-Type: application/json" \
  -d '{"techA": "nextjs", "techB": "prisma"}'
```

### JavaScript (fetch)

```javascript
const response = await fetch('https://devradar.dev/api/v1/check', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ techA: 'nextjs', techB: 'prisma' })
});

const data = await response.json();
console.log(data.status, data.message);
```

### JavaScript (axios)

```javascript
import axios from 'axios';

const { data } = await axios.post('https://devradar.dev/api/v1/check', {
  techA: 'nextjs',
  techB: 'prisma'
});

console.log(data.status, data.message);
```

### Python (requests)

```python
import requests

response = requests.post('https://devradar.dev/api/v1/check', json={
    'techA': 'nextjs',
    'techB': 'prisma'
})

data = response.json()
print(data['status'], data['message'])
```

### Go

```go
package main

import (
    "bytes"
    "encoding/json"
    "net/http"
)

func main() {
    payload := map[string]string{"techA": "nextjs", "techB": "prisma"}
    body, _ := json.Marshal(payload)

    resp, _ := http.Post(
        "https://devradar.dev/api/v1/check",
        "application/json",
        bytes.NewBuffer(body),
    )
    defer resp.Body.Close()

    var data map[string]interface{}
    json.NewDecoder(resp.Body).Decode(&data)
    println(data["status"], data["message"])
}
```

## Example Responses

### Compatible

```json
{
  "version": "1.0",
  "status": "compatible",
  "severity": "info",
  "message": "Next.js works seamlessly with Prisma ORM for server-side rendering and API routes."
}
```

### Partial with Workaround

```json
{
  "version": "1.0",
  "status": "partial",
  "severity": "warning",
  "message": "Prisma does not support Edge Runtime due to Node.js APIs required for the query engine.",
  "workaround": "Use dynamic imports with 'ssr: false' for routes using Prisma, or consider Turso for Edge-compatible database."
}
```

### Incompatible

```json
{
  "version": "1.0",
  "status": "incompatible",
  "severity": "error",
  "message": "This framework requires a different authentication approach.",
  "workaround": "Consider using Auth0 or Clerk instead."
}
```

### Unknown

```json
{
  "version": "1.0",
  "status": "unknown",
  "message": "This combination hasn't been analyzed yet. Proceed with caution and test thoroughly."
}
```

## Error Responses

### 400 Bad Request

```json
{
  "version": "1.0",
  "error": {
    "code": "INVALID_INPUT",
    "message": "Both techA and techB are required."
  }
}
```

### 429 Rate Limit Exceeded

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

## Rate Limiting

- **Limit**: 60 requests per minute
- **Headers**: `X-RateLimit-Remaining`, `X-RateLimit-Reset`

See [Rate Limits](/docs/api/rate-limits) for details.

## What's Next

- [Scan Endpoint](/docs/api/scan-endpoint) - Analyze complete projects
- [Badge Endpoint](/docs/api/badge-endpoint) - Generate badges
- [Errors](/docs/api/errors) - Error handling guide

---

**Try it now:** [Interactive API Demo](https://devradar.dev/check) →
