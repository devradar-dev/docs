---
title: Badge Endpoint
description: Generate SVG compatibility badges for your README. Show tech stack compatibility at a glance.
---

# Badge Endpoint

Generate SVG compatibility badges for your README, documentation, or website. Badges update automatically with the latest compatibility data.

## Endpoint

```
GET /api/v1/badge
```

## Request

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `techA` | string | Yes | First technology name |
| `techB` | string | Yes | Second technology name |
| `style` | string | No | Badge style: `flat` (default) or `plastic` |

### Example Requests

```
GET /api/v1/badge?techA=nextjs&techB=prisma
GET /api/v1/badge?techA=nextjs&techB=prisma&style=plastic
```

## Response

### Content Type

```
Content-Type: image/svg+xml
```

### Response Body

SVG image data (rendered as a badge).

## Badge Colors

Badges use GitHub-style colors:

| Status | Color | Hex |
|--------|-------|-----|
| Compatible | Green | `#3FB950` |
| Partial | Amber | `#D29922` |
| Incompatible | Red | `#F85149` |
| Unknown | Gray | `#8B949E` |
| Deprecated | Gray | `#8B949E` |

## Badge Styles

### Flat (Default)

Classic flat badge design:

```
[ compatibility | compatible ]
```

```
GET /api/v1/badge?techA=nextjs&techB=prisma
```

### Plastic

Subtle gradient effect:

```
[ compatibility | compatible ] (with gradient)
```

```
GET /api/v1/badge?techA=nextjs&techB=prisma&style=plastic
```

## Usage Examples

### Markdown (GitHub, GitLab, etc.)

```markdown
![Next.js + Prisma](https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma)
```

### HTML

```html
<img src="https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma" alt="Next.js + Prisma compatibility">
```

### reStructuredText (ReadTheDocs)

```rst
.. image:: https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma
   :alt: Next.js + Prisma compatibility
```

### AsciiDoc

```asciidoc
image:https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma["Next.js + Prisma compatibility"]
```

## Example README Section

```markdown
## Tech Stack

| Component | Technology | Compatibility |
|-----------|------------|---------------|
| Framework | [Next.js](https://nextjs.org) | ![Framework](https://devradar.dev/api/v1/badge?techA=nextjs&techB=vercel) |
| Database | [Prisma](https://prisma.io) | ![Database](https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma) |
| Auth | [NextAuth](https://next-auth.js.org) | ![Auth](https://devradar.dev/api/v1/badge?techA=nextjs&techB=next-auth) |
| Styling | [Tailwind](https://tailwindcss.com) | ![Styling](https://devradar.dev/api/v1/badge?techA=nextjs&techB=tailwind) |
```

## Caching

Badge responses are cached for **24 hours** by default.

**Cache-Control header:**

```
Cache-Control: public, max-age=86400, s-maxage=86400
```

This ensures fast loading and reduces API load while keeping badges reasonably fresh.

## Error Responses

When parameters are invalid, the endpoint returns an error (not an SVG):

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

## Rate Limiting

- **Limit**: 120 requests per minute
- **Headers**: `X-RateLimit-Remaining`, `X-RateLimit-Reset`

Badges are cached, so rate limits are rarely hit in normal usage.

See [Rate Limits](/docs/api/rate-limits) for details.

## Best Practices

### 1. Use in Project READMEs

Add badges for your key technology pairs to help contributors understand your stack at a glance.

### 2. Link to Full Details

Wrap badges in links to detailed compatibility information:

```markdown
[![Next.js + Prisma](https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma)](https://devradar.dev/check/nextjs/prisma)
```

### 3. Consider Your Audience

For internal docs, badges provide quick verification. For public READMEs, they signal compatibility awareness.

### 4. Update When Stacks Change

When you add or change technologies, update your badges accordingly.

## What's Next

- [Check Endpoint](/docs/api/check-endpoint) - Get detailed compatibility info
- [Scan Endpoint](/docs/api/scan-endpoint) - Analyze complete projects
- [Rate Limits](/docs/api/rate-limits) - Usage limits

---

**Generate badges:** [Badge Generator](https://devradar.dev/check) →
