---
title: API Overview
description: Complete API reference for DevRadar public endpoints. Check compatibility, scan projects, and embed badges.
---

# API Overview

The DevRadar Public API provides programmatic access to technology compatibility data, project scanning, and badge generation.

## Base URL

```
https://devradar.dev/api/v1
```

All endpoints use the `v1` API version. Future versions will be announced with backward compatibility considerations.

## Authentication

Most endpoints are **public** and do not require authentication:

- `POST /check` - Public, rate-limited
- `GET /badge` - Public, rate-limited
- `POST /scan` - Public, rate-limited

Rate limiting is based on IP address. See [Rate Limits](/docs/api/rate-limits) for details.

## Response Format

All successful responses include a `version` field:

```json
{
  "version": "1.0",
  "status": "compatible",
  "message": "Next.js works seamlessly with Prisma ORM."
}
```

Error responses follow a consistent format:

```json
{
  "version": "1.0",
  "error": {
    "code": "INVALID_INPUT",
    "message": "Both techA and techB are required.",
    "details": "Optional additional context"
  }
}
```

## Available Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/check` | POST | Verify compatibility of two technologies |
| `/scan` | POST | Analyze package.json for compatibility issues |
| `/badge` | GET | Generate SVG compatibility badge |

## Quick Start

### Check Compatibility

```bash
curl -X POST https://devradar.dev/api/v1/check \
  -H "Content-Type: application/json" \
  -d '{"techA": "nextjs", "techB": "prisma"}'
```

### Scan Project

```bash
curl -X POST https://devradar.dev/api/v1/scan \
  -H "Content-Type: application/json" \
  -d '{"dependencies": ["next", "prisma", "vercel"]}'
```

### Get Badge

```bash
curl "https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma"
```

## Rate Limiting

All endpoints are rate-limited to ensure fair usage:

| Endpoint | Limit | Window |
|----------|-------|--------|
| `POST /check` | 60 requests | 1 minute |
| `POST /scan` | 10 requests | 1 minute |
| `GET /badge` | 120 requests | 1 minute |

Rate limit headers are included in every response:

```
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 2025-01-15T10:31:00Z
```

See [Rate Limits](/docs/api/rate-limits) for complete details.

## CORS

The API supports CORS for browser-based requests:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

## Status Codes

| Code | Meaning |
|------|---------|
| `200` | Success |
| `400` | Invalid input |
| `429` | Rate limit exceeded |
| `500` | Internal error |

## Error Codes

| Code | Description |
|------|-------------|
| `INVALID_INPUT` | Request validation failed |
| `RATE_LIMIT_EXCEEDED` | Rate limit reached |
| `INTERNAL_ERROR` | Server error occurred |
| `NOT_FOUND` | Resource not found |

See [Errors](/docs/api/errors) for complete error handling documentation.

## Technology Name Format

Use lowercase, simplified names:

| Preferred | Also Recognized |
|-----------|-----------------|
| `nextjs` | next.js, next-js |
| `react` | reactjs |
| `prisma` | prisma-orm |
| `postgresql` | postgres, pg |

## SDKs

Official SDKs are coming soon:
- JavaScript/TypeScript
- Python
- Go

For now, use the REST API directly with HTTP clients.

## What's Next

- [Check Endpoint](/docs/api/check-endpoint) - Compatibility verification
- [Scan Endpoint](/docs/api/scan-endpoint) - Project scanning
- [Badge Endpoint](/docs/api/badge-endpoint) - Badge generation
- [Rate Limits](/docs/api/rate-limits) - Usage limits

---

**Ready to integrate?** [Check Endpoint Reference](/docs/api/check-endpoint) →
