---
title: Adding Compatibility Rules
description: Submit compatibility data for technology pairs. Help improve DevRadar's compatibility database.
---

# Adding Compatibility Rules

Contribute compatibility data by submitting verified information about how two technologies work together.

## Before You Submit

### Check if Data Exists

1. Visit [devradar.dev/check](https://devradar.dev/check)
2. Search for your technology pair
3. If status is "unknown", your submission is valuable

### Verify Your Information

Only submit what you've personally verified:

| Verification Method | Acceptance |
|---------------------|------------|
| **Personal testing** | Preferred |
| **Official docs** | Accepted |
| **GitHub issue** | Accepted |
| **Community consensus** | Case-by-case |

**Required information:**
- Technology names
- Compatibility status
- Evidence/source links
- Your testing context

## What to Submit

### Required Fields

| Field | Description | Example |
|-------|-------------|---------|
| **Technology A** | First technology name | `nextjs` |
| **Technology B** | Second technology name | `prisma` |
| **Status** | Compatibility level | `compatible`, `partial`, `incompatible` |
| **Severity** | Impact level | `info`, `warning`, `error`, `critical` |
| **Explanation** | Why this status | Detailed description |
| **Evidence** | Source links | Official docs, GitHub issues |
| **Tested Versions** | What you tested | `nextjs@14.2`, `prisma@5.2` |

### Optional Fields

| Field | Description | Example |
|-------|-------------|---------|
| **Workaround** | Fix for partial/incompatible | "Use dynamic imports" |
| **Platform** | Specific platform | "web", "mobile", "edge" |
| **Notes** | Additional context | "Only for API routes" |

## Status Guidelines

### Compatible ✅

Use when technologies work together without issues.

**Example:**
```
Technology A: nextjs
Technology B: prisma
Status: compatible
Severity: info
Explanation: Prisma works seamlessly with Next.js API routes.
Evidence: https://www.prisma.io/docs/orm/more/help-and-support/faq
```

### Partial ⚠️

Use when technologies work but with limitations.

**Example:**
```
Technology A: prisma
Technology B: vercel-edge
Status: partial
Severity: warning
Explanation: Prisma doesn't support Edge Runtime due to Node.js APIs.
Workaround: Use dynamic imports with 'ssr: false' for Prisma routes.
Evidence: https://github.com/prisma/prisma/issues/16492
```

### Incompatible ❌

Use when technologies have fundamental conflicts.

**Example:**
```
Technology A: nextjs
Technology B: material-ui
Status: partial
Severity: warning
Explanation: MUI requires client-side rendering. Use 'use client' directive.
Workaround: Wrap MUI components in client components.
Evidence: https://mui.com/material-ui/guides/nextjs-app-router/
```

## Submission Methods

### Method 1: GitHub Issue (Recommended)

1. Go to [github.com/startupsolellc/stackpilot/issues](https://github.com/startupsolellc/stackpilot/issues)
2. Click "New Issue"
3. Use "Compatibility Data" template
4. Fill in all fields
5. Submit

**Template:**

```markdown
## Compatibility Data Submission

**Technologies:** techA + techB
**Status:** compatible/partial/incompatible
**Severity:** info/warning/error/critical

### Explanation
[Detailed explanation of compatibility]

### Workaround (if applicable)
[How to make it work if partial/incompatible]

### Evidence
- Official docs: [URL]
- GitHub issue: [URL]
- Your testing: [description]

### Tested Versions
- techA: version
- techB: version

### Additional Context
[Any other relevant information]
```

### Method 2: Discussion Post

For complex submissions or questions:

1. Go to [github.com/startupsolellc/stackpilot/discussions](https://github.com/startupsolellc/stackpilot/discussions)
2. Start a new discussion
3. Use "Compatibility Data" category
4. Post your submission

### Method 3: API (Developers)

Submit via API (requires authentication):

```bash
curl -X POST https://devradar.dev/api/v1/contributions/compatibility \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "techA": "nextjs",
    "techB": "prisma",
    "status": "compatible",
    "severity": "info",
    "explanation": "...",
    "evidence": ["https://..."]
  }'
```

## Review Process

### What We Check

1. **Accuracy** - Can we verify the information?
2. **Evidence** - Are sources provided?
3. **Clarity** - Is the explanation clear?
4. **Completeness** - Are all required fields filled?

### Timeline

| Stage | Duration |
|-------|----------|
| Initial review | 1-2 days |
| Verification | 2-3 days |
| Merge | Within 7 days |

### After Submission

You'll receive updates:
- **Under Review** - Being evaluated
- **Approved** - Added to database
- **Changes Requested** - Needs more info
- **Declined** - Cannot accept (with reason)

## Best Practices

### 1. Test Before Submitting

Actually use the technologies together:

```bash
# Create a test project
npx create-next-app@latest test-prisma
cd test-prisma
npm install prisma
# Verify it works
```

### 2. Document Your Context

Help us understand your setup:

```
I tested this with:
- Next.js 14.2 (App Router)
- Prisma 5.2 (PostgreSQL)
- Vercel deployment
- Only API routes (no Edge)
```

### 3. Link to Evidence

Strong sources strengthen your submission:

- Official documentation (best)
- Official GitHub issues (good)
- Blog posts from maintainers (acceptable)
- Your own testing (with details)

### 4. Be Specific

```
Good: "Next.js 14 App Router + Prisma 5 works in API routes but not Edge Runtime"
Bad: "Next.js works with Prisma"
```

### 5. Update When Versions Change

If you submitted data for older versions, submit updates when new versions release:

```markdown
## Update to Previous Submission

**Original:** #123
**New Versions:** nextjs@15, prisma@6
**Changes:** [What changed?]
```

## What Happens After Approval

Your submission becomes part of the database:

1. **Compatibility checks** - API returns your data
2. **Scans** - Projects scanned with this info
3. **Content** - May be used in guides/comparisons
4. **Attribution** - Your username credited (optional)

## Examples

### Example 1: Compatible

```markdown
## Compatibility Data Submission

**Technologies:** remix + prisma
**Status:** compatible
**Severity:** info

### Explanation
Remix works seamlessly with Prisma. Loaders can use Prisma Client
directly for database queries.

### Evidence
- Official docs: https://prisma.io/docs/orm/more/support-and-oss/frameworks/remix
- Remix docs: https://remix.run/docs/en/main/guides/database

### Tested Versions
- remix: 2.8.0
- prisma: 5.20.0

### Additional Context
Tested with SQLite in development, PostgreSQL in production.
```

### Example 2: Partial with Workaround

```markdown
## Compatibility Data Submission

**Technologies:** nextjs + supabase-auth
**Status:** partial
**Severity:** warning

### Explanation
Supabase Auth works in Next.js but requires configuration for App Router.
The helper components are client-side only.

### Workaround
Use 'use client' directive for auth components, or implement
server-side auth with cookies.

### Evidence
- Official guide: https://supabase.com/docs/guides/auth/server-side/nextjs
- GitHub discussion: https://github.com/vercel/next.js/discussions/44929

### Tested Versions
- nextjs: 14.2.0
- supabase: 2.39.0

### Additional Context
Tested with App Router (not Pages Router).
```

## What's Next

- [Adding AI Tools](/docs/contributing/adding-ai-tools) - Submit tool information
- [Contributing Overview](/docs/contributing/overview) - Other ways to contribute
- [API Reference](/docs/api/overview) - API documentation

---

**Submit now:** [Open an Issue](https://github.com/startupsolellc/stackpilot/issues/new) →
