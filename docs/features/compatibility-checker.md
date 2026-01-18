---
title: Compatibility Checker
description: Instant verification of technology pair compatibility. Check if any two technologies work together before committing to your stack.
---

# Compatibility Checker

The Compatibility Checker provides instant verification of whether two technologies work together. Perfect for quick validation before committing to a tech choice.

## Overview

Access the checker from the **Check** page in the navigation. Simply enter two technology names and get instant results.

## How It Works

1. Enter your first technology (e.g., `nextjs`)
2. Enter your second technology (e.g., `prisma`)
3. Click **Check Compatibility**
4. View detailed results

## Response Structure

Every check returns:

```json
{
  "version": "1.0",
  "status": "compatible",
  "severity": "info",
  "message": "Next.js works seamlessly with Prisma ORM.",
  "workaround": null,
  "docsUrl": "https://devradar.dev/check/nextjs/prisma",
  "lastUpdated": "2025-01-15T10:30:00Z"
}
```

### Status Types

| Status | Meaning | Action |
|--------|---------|--------|
| **compatible** | Works seamlessly | Proceed with confidence |
| **partial** | Works with limitations | Review workaround |
| **incompatible** | Fundamental conflict | Choose alternative |
| **deprecated** | End-of-life technology | Consider migration |
| **unknown** | No data available | Contribute knowledge |

### Severity Levels

| Severity | Meaning |
|----------|---------|
| **info** | Information only, no concerns |
| **warning** | Caution advised, may need attention |
| **error** | Significant issue, workaround recommended |
| **critical** | Blocking issue, not recommended |

## Understanding Results

### Compatible ✅

```json
{
  "status": "compatible",
  "message": "Next.js works seamlessly with Prisma ORM for server-side rendering and API routes."
}
```

No action needed - proceed with confidence.

### Partial ⚠️

```json
{
  "status": "partial",
  "message": "React Server Components have limited support for this library.",
  "workaround": "Use 'use client' directive for components using this library."
}
```

Review the workaround and implement as suggested.

### Incompatible ❌

```json
{
  "status": "incompatible",
  "message": "This framework requires a different authentication approach.",
  "workaround": "Consider using Auth0 or Clerk instead."
}
```

Choose the suggested alternative or a different technology pair.

## Common Use Cases

### Validating a New Addition

"Can I add Stripe payments to my Next.js + Supabase app?"

Check: `nextjs` + `stripe`
→ Result: Compatible ✅

### Checking Database Options

"Which database works best with my Next.js app?"

Check multiple pairs:
- `nextjs` + `postgresql` → Compatible ✅
- `nextjs` + `mongodb` → Compatible ✅
- `nextjs` + `prisma` → Compatible ✅

### Evaluating AI Tools

"Is Cursor compatible with my React Native project?"

Check: `cursor` + `react-native`
→ Result: Partial ⚠️ (limited mobile support)

## Technology Name Format

Use lowercase, simplified names:

| Preferred | Also Recognized |
|-----------|-----------------|
| `nextjs` | next.js, next-js |
| `react` | reactjs |
| `prisma` | prisma-orm |
| `postgresql` | postgres, pg |
| `tailwind` | tailwindcss |

## Related Checks

After viewing a compatibility result, explore:

- **Related Technologies** - Other tools commonly used with this pair
- **Alternative Options** - Similar technologies with better compatibility
- **Community Examples** - Real-world projects using this combination

## Best Practices

### 1. Check Both Directions

While `nextjs + prisma` and `prisma + nextjs` return the same result, checking with your primary technology first helps you understand compatibility from your starting point.

### 2. Consider Your Context

A "partial" result might be fine for a prototype but problematic for production. Consider your project stage and requirements.

### 3. Check Multiple Pairs

Building a full stack? Check each pair:
- Framework + Database
- Framework + Hosting
- Framework + Auth
- Database + Hosting

Or use the **Stack Wizard** for comprehensive validation.

### 4. Keep Results Updated

Compatibility data is regularly updated. Re-check if you haven't validated in a few months.

## API Access

Need to integrate compatibility checks into your workflow?

```bash
curl -X POST https://devradar.dev/api/v1/check \
  -H "Content-Type: application/json" \
  -d '{"techA": "nextjs", "techB": "prisma"}'
```

See the [API Reference](/docs/api/check-endpoint) for details.

## What's Next

- [Stack Wizard](/docs/features/stack-wizard) - Build complete stacks
- [Project Scanner](/docs/features/project-management) - Analyze existing projects
- [API Reference](/docs/api/overview) - Integrate into your tools

---

**Have compatibility data to share?** [Contribute](/docs/contributing/adding-compatibility-rules) →
