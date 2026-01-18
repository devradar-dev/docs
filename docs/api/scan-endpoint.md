---
title: Scan Endpoint
description: Analyze package.json for compatibility issues. Get a comprehensive score and detailed issue breakdown.
---

# Scan Endpoint

Analyze your project's dependencies for compatibility issues. Get a comprehensive compatibility score and detailed breakdown of any problems.

## Endpoint

```
POST /api/v1/scan
```

## Request

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `Content-Type` | string | Yes | Must be `application/json` |

### Body

Provide **one** of the following:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `dependencies` | string[] | Conditional | Array of package names |
| `packageJson` | object | Conditional | Full package.json object |

### Example Requests

**With dependencies array:**

```json
{
  "dependencies": ["next", "prisma", "@vercel/node", "tailwindcss"]
}
```

**With package.json:**

```json
{
  "packageJson": {
    "dependencies": {
      "next": "^14.0.0",
      "prisma": "^5.0.0",
      "@vercel/node": "^3.0.0"
    },
    "devDependencies": {
      "tailwindcss": "^3.4.0"
    }
  }
}
```

## Response

### Success Response (200)

```json
{
  "version": "1.0",
  "stack": {
    "detected": ["nextjs", "prisma", "vercel", "tailwind"],
    "allPackages": ["next", "prisma", "@vercel/node", "tailwindcss"],
    "score": 85,
    "issues": [
      {
        "severity": "warning",
        "pair": "prisma-vercel",
        "message": "Prisma does not support Edge Runtime.",
        "workaround": "Use dynamic imports with 'ssr: false' for routes using Prisma."
      }
    ],
    "suggestions": [
      {
        "type": "alternative",
        "message": "Consider using Turso or PlanetScale for Edge-compatible database",
        "url": "https://devradar.dev/check/prisma-vercel-edge"
      }
    ]
  },
  "metadata": {
    "scannedAt": "2025-01-15T10:30:00Z",
    "dependencyCount": 4
  }
}
```

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `version` | string | API version (always "1.0") |
| `stack.detected` | string[] | Recognized technology IDs |
| `stack.allPackages` | string[] | All packages from input |
| `stack.score` | number | Overall compatibility score (0-100) |
| `stack.issues` | array | Compatibility issues found |
| `stack.suggestions` | array | Improvement suggestions |
| `metadata.scannedAt` | string | ISO 8601 timestamp |
| `metadata.dependencyCount` | number | Total dependencies scanned |

### Issue Object

| Field | Type | Description |
|-------|------|-------------|
| `severity` | string | `info`, `warning`, `error` |
| `pair` | string | Technology pair identifier |
| `message` | string | Issue description |
| `workaround` | string | null | Suggested fix |

### Suggestion Types

| Type | Description |
|------|-------------|
| `alternative` | Suggests different technology |
| `upgrade` | Suggests version upgrade |
| `config` | Suggests configuration change |

## Score Calculation

Scores are calculated based on compatibility pairs:

| Status | Points |
|--------|--------|
| Compatible | +10 |
| Partial | +5 |
| Incompatible | -20 |
| Unknown | 0 |

Final score is normalized to 0-100 scale.

## Code Examples

### cURL

```bash
curl -X POST https://devradar.dev/api/v1/scan \
  -H "Content-Type: application/json" \
  -d '{"dependencies": ["next", "prisma", "vercel"]}'
```

### JavaScript (fetch)

```javascript
const response = await fetch('https://devradar.dev/api/v1/scan', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    dependencies: ['next', 'prisma', 'vercel']
  })
});

const data = await response.json();
console.log(`Score: ${data.stack.score}/100`);
data.stack.issues.forEach(issue => {
  console.log(`- ${issue.severity}: ${issue.message}`);
});
```

### Python (requests)

```python
import requests

response = requests.post('https://devradar.dev/api/v1/scan', json={
    'dependencies': ['next', 'prisma', 'vercel']
})

data = response.json()
print(f"Score: {data['stack']['score']}/100")

for issue in data['stack']['issues']:
    print(f"- {issue['severity']}: {issue['message']}")
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
    payload := map[string][]string{
        "dependencies": {"next", "prisma", "vercel"},
    }
    body, _ := json.Marshal(payload)

    resp, _ := http.Post(
        "https://devradar.dev/api/v1/scan",
        "application/json",
        bytes.NewBuffer(body),
    )
    defer resp.Body.Close()

    var data map[string]interface{}
    json.NewDecoder(resp.Body).Decode(&data)

    stack := data["stack"].(map[string]interface{})
    println("Score:", stack["score"])
}
```

## Error Responses

### 400 Bad Request - No Tech Detected

```json
{
  "version": "1.0",
  "error": {
    "code": "NO_TECH_DETECTED",
    "message": "No recognized technologies found in dependencies",
    "details": "Ensure you have valid packages in your package.json"
  }
}
```

### 400 Bad Request - Invalid Input

```json
{
  "version": "1.0",
  "error": {
    "code": "INVALID_INPUT",
    "message": "Request body is required."
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

## Package Recognition

Common package patterns are automatically recognized:

| Package Pattern | Detected As |
|-----------------|-------------|
| `next` | `nextjs` |
| `prisma` | `prisma` |
| `@prisma/client` | `prisma` |
| `tailwindcss` | `tailwind` |
| `next-auth` | `next-auth` |
| `@supabase/supabase-js` | `supabase` |

Packages not in the recognition database are ignored for compatibility checking but included in `allPackages`.

## Rate Limiting

- **Limit**: 10 requests per minute
- **Headers**: `X-RateLimit-Remaining`, `X-RateLimit-Reset`

See [Rate Limits](/docs/api/rate-limits) for details.

## What's Next

- [Check Endpoint](/docs/api/check-endpoint) - Verify individual pairs
- [Badge Endpoint](/docs/api/badge-endpoint) - Generate badges
- [Rate Limits](/docs/api/rate-limits) - Usage limits

---

**Try it now:** [Project Scanner](https://devradar.dev/scan) →
