---
title: Badge Endpoint
description: Generate SVG compatibility badges for your README. Show tech stack compatibility at a glance.
---

# Badge Endpoint

Generate SVG compatibility badges for your README, documentation, or website. Badges update automatically with the latest compatibility data.

## Endpoint

```
GET /api/v1/badge/{slug}.svg
```

The badge endpoint uses a **slug-based URL format** - no query parameters required.

## URL Format

The slug combines two technology names with a separator:

### Format

```
/api/v1/badge/{techA}-{techB}.svg
```

### Supported Separators

You can use any of these separators between technology names:

| Separator | Example |
|-----------|---------|
| `-` (dash) | `/api/v1/badge/nextjs-prisma.svg` |
| `.` (dot) | `/api/v1/badge/nextjs.prisma.svg` |
| `_` (underscore) | `/api/v1/badge/nextjs_prisma.svg` |

The dash (`-`) separator is **recommended** for best compatibility.

### Path vs Query Parameters

**Important:** This endpoint uses path parameters, NOT query parameters.

| Format | Example | Status |
|--------|---------|--------|
| **Slug-based** (correct) | `/api/v1/badge/nextjs-prisma.svg` | ✅ Use this |
| Query params (incorrect) | `/api/v1/badge?techA=nextjs&techB=prisma` | ❌ Not supported |

### Examples

```
GET /api/v1/badge/nextjs-prisma.svg
GET /api/v1/badge/react-vercel.svg
GET /api/v1/badge/tailwind-vite.svg
GET /api/v1/badge/supabase-next.auth.svg
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

### Default Badge

Classic flat badge design:

```
[ compatibility | compatible ]
```

## Usage Examples

### Markdown (GitHub, GitLab, etc.)

```markdown
![Next.js + Prisma](https://devradar.dev/api/v1/badge/nextjs-prisma.svg)
```

### HTML

```html
<img src="https://devradar.dev/api/v1/badge/nextjs-prisma.svg" alt="Next.js + Prisma compatibility">
```

### reStructuredText (ReadTheDocs)

```rst
.. image:: https://devradar.dev/api/v1/badge/nextjs-prisma.svg
   :alt: Next.js + Prisma compatibility
```

### AsciiDoc

```asciidoc
image:https://devradar.dev/api/v1/badge/nextjs-prisma.svg["Next.js + Prisma compatibility"]
```

### Asciidoc with Link

```asciidoc
image:https://devradar.dev/api/v1/badge/nextjs-prisma.svg[link=https://devradar.dev/check/nextjs/prisma]
```

## Example README Section

```markdown
## Tech Stack

| Component | Technology | Compatibility |
|-----------|------------|---------------|
| Framework | [Next.js](https://nextjs.org) | ![Framework](https://devradar.dev/api/v1/badge/nextjs-vercel.svg) |
| Database | [Prisma](https://prisma.io) | ![Database](https://devradar.dev/api/v1/badge/nextjs-prisma.svg) |
| Auth | [Auth.js](https://authjs.dev) | ![Auth](https://devradar.dev/api/v1/badge/nextjs-authjs.svg) |
| Styling | [Tailwind](https://tailwindcss.com) | ![Styling](https://devradar.dev/api/v1/badge/nextjs-tailwind.svg) |
```

## Linking to Full Details

Wrap badges in links to detailed compatibility information:

```markdown
[![Next.js + Prisma](https://devradar.dev/api/v1/badge/nextjs-prisma.svg)](https://devradar.dev/check/nextjs/prisma)
```

This creates a clickable badge that takes users to the full compatibility report.

## Caching

Badge responses are cached for **24 hours** by default.

**Cache-Control header:**

```
Cache-Control: public, max-age=86400, s-maxage=86400
```

This ensures fast loading and reduces API load while keeping badges reasonably fresh.

## Error Responses

When the slug format is invalid or technologies cannot be parsed, a generic "Unknown" badge is returned:

### Unknown Badge

The endpoint always returns valid SVG (even for errors):

```
[ compatibility | unknown ]
```

This ensures your README never breaks due to missing compatibility data.

## Hybrid Lookup

The badge endpoint uses intelligent fallback:

1. **Slug Match** - First tries to find a pre-computed compatibility slug
2. **Pair Match** - Falls back to parsing the slug and finding a matching pair

This means badges work even without explicit slugs in the database.

### Example: Parsed Slug

```
Request: /api/v1/badge/nextjs-prisma.svg

1. Try slug match: "nextjs-prisma" in database
2. If not found, parse into: {techA: "nextjs", techB: "prisma"}
3. Search for any rule with nextjs + prisma (bidirectional)
4. Generate badge from result
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

Always wrap badges in links to detailed compatibility information:

```markdown
[![Next.js + Prisma](https://devradar.dev/api/v1/badge/nextjs-prisma.svg)](https://devradar.dev/check/nextjs/prisma)
```

### 3. Consider Your Audience

For internal docs, badges provide quick verification. For public READMEs, they signal compatibility awareness.

### 4. Use Recommended Separator

The dash (`-`) separator is most reliable:

```
✅ /api/v1/badge/nextjs-prisma.svg (recommended)
⚠️ /api/v1/badge/nextjs.prisma.svg (works but less common)
⚠️ /api/v1/badge/nextjs_prisma.svg (works but less common)
```

### 5. Update When Stacks Change

When you add or change technologies, update your badges accordingly.

## What's Next

- [Check Endpoint](/docs/api/check-endpoint) - Get detailed compatibility info
- [Scan Endpoint](/docs/api/scan-endpoint) - Analyze complete projects
- [Rate Limits](/docs/api/rate-limits) - Usage limits

---

**Generate badges:** [Badge Generator](https://devradar.dev/check) →
