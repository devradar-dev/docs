---
title: Web Scan
description: Upload your package.json to analyze your entire tech stack for compatibility issues. Get instant insights on conflicts, Edge Runtime problems, and database warnings.
---

# Web Scan

The Web Scan feature analyzes your entire tech stack by reading your `package.json` file. Upload or paste your dependencies and get instant compatibility insights across all technology combinations.

## Overview

The scan detects potential issues before you deploy, saving hours of debugging time. It checks for:

- **Runtime conflicts** (Edge Runtime incompatibility)
- **Database-hosting mismatches**
- **Framework integration issues**
- **Deprecated package warnings**
- **Configuration requirements**

## Access

Navigate to [/scan](https://devradar.dev/scan) or click **Scan** in the navigation.

## How It Works

### 1. Upload Your package.json

You have two options:

#### Option A: File Upload
1. Click **Upload package.json**
2. Select your `package.json` file (max 100KB)
3. The scanner automatically extracts dependencies

#### Option B: Paste Content
1. Open your `package.json` file
2. Copy the entire content
3. Paste into the text area
4. Click **Analyze**

### 2. Technology Detection

The scanner identifies technologies by parsing your dependencies:

| Dependency Type | Examples |
|-----------------|----------|
| **Direct mappings** | `next` → Next.js, `react` → React |
| **Scoped packages** | `@radix-ui/*` → Radix UI |
| **Framework-specific** | `@prisma/client` → Prisma |
| **Wildcards** | `@google/*` → Google GenAI |

### 3. Compatibility Analysis

The scanner checks all detected technology pairs against DevRadar's compatibility database:

```
Your Stack: [Next.js, Prisma, Vercel, Tailwind, Auth.js]
         ↓
Check all pairs: (10 unique combinations checked)
         ↓
Results: 2 issues found
```

### 4. Results Display

#### Compatibility Score

| Score | Grade | Color | Meaning |
|-------|-------|-------|---------|
| 90-100 | A | Green | Excellent compatibility |
| 70-89 | B | Yellow | Good with minor notes |
| 50-69 | C | Orange | Some issues detected |
| 40-49 | D | Red | Significant problems |
| 0-39 | F | Dark Red | Critical incompatibilities |

#### Issues List

Issues are grouped by severity:

| Severity | Icon | Description |
|----------|------|-------------|
| **Critical** | 🚨 | Breaking changes, won't work together |
| **Error** | ❌ | Functional issues requiring attention |
| **Warning** | ⚠️ | Potential limitations to be aware of |
| **Info** | ℹ️ | General information and notes |

Each issue shows:
- Technology pair (e.g., `Next.js + Prisma`)
- Detailed explanation
- Workaround solutions (when available)
- Link to full compatibility check
- Badge for your README

#### Suggestions

Actionable recommendations based on your stack:

| Type | Description | Example |
|------|-------------|---------|
| **Alternative** | Suggest different technology | "Use Turso instead of Prisma" |
| **Upgrade** | Recommend package updates | "Upgrade to Next.js 14+" |
| **Configuration** | Setup guidance | "Enable `ssr: false` for API routes" |

## Common Issues Detected

### Edge Runtime Incompatibility

**Issue:** Databases like Prisma don't work in Edge Runtime.

**Detected with:**
- Next.js + Prisma on Vercel
- Next.js + MongoDB on Edge functions
- Any ORMs requiring Node.js runtime

**Suggestion:**
```
Consider Edge-compatible alternatives:
- Turso (libSQL)
- PlanetScale (Vitess)
- Supabase Edge Functions
- Or use dynamic imports: `ssr: false`
```

### Database Hosting Conflicts

**Issue:** Traditional databases don't work on serverless platforms.

**Examples:**
- PostgreSQL + Vercel (without external service)
- MongoDB + Netlify Functions

**Suggestion:**
```
Use managed database services:
- Supabase (PostgreSQL)
- Neon (Serverless Postgres)
- MongoDB Atlas
- PlanetScale (MySQL)
```

### Framework Integration Problems

**Issue:** Certain packages don't integrate well together.

**Examples:**
- MUI + Next.js App Router (requires specific configuration)
- Next-Auth + Next.js Edge Runtime
- Tailwind + Certain UI libraries

## Understanding Your Results

### Issue Cards

Each expandable card shows:

```
┌─────────────────────────────────────────────┐
│ Prisma + Vercel              [ERROR]         │
│                                             │
│ Prisma doesn't work in Vercel Edge Runtime │
│                                             │
│ 💡 Workaround:                              │
│ Use dynamic imports with ssr: false        │
│                                             │
│ [View Details] [Copy Badge]                 │
└─────────────────────────────────────────────┘
```

### Package Browser

Complete list of detected packages:
- Searchable by name
- Grouped by scope (e.g., `@radix-ui/*`)
- Categorized by dependency type

### Shareable Badges

Add compatibility badges to your README:

```markdown
![Next.js + Prisma](https://devradar.dev/api/v1/badge/nextjs-prisma.svg)
```

## API Integration

For automated scanning, use the Scan API:

```bash
curl -X POST https://devradar.dev/api/v1/scan \
  -H "Content-Type: application/json" \
  -d '{
    "packageJson": {
      "dependencies": {
        "next": "^14.0.0",
        "react": "^18.0.0",
        "prisma": "^5.0.0"
      }
    }
  }'
```

**Response:**
```json
{
  "version": "1.0",
  "stack": {
    "detected": ["nextjs", "react", "prisma"],
    "score": 75,
    "issues": [
      {
        "severity": "warning",
        "pair": "nextjs-prisma",
        "message": "Edge Runtime incompatibility detected",
        "workaround": "Use dynamic imports with ssr: false"
      }
    ]
  }
}
```

## Rate Limits

- **Limit:** 10 scans per minute per IP
- **Headers:** `X-RateLimit-Remaining`, `X-RateLimit-Reset`

## Best Practices

### 1. Scan Before Deploying

Run the scan when:
- Starting a new project
- Adding major dependencies
- Preparing for production deployment
- Changing hosting platforms

### 2. Review All Warnings

Even warnings can cause issues:
- Edge Runtime warnings → deployment failures
- Database conflicts → runtime errors
- Framework issues → poor performance

### 3. Check Suggestions First

Suggestions often provide the easiest fix:
- Alternative technologies may be drop-in replacements
- Configuration changes take minutes
- Upgrades may resolve multiple issues

### 4. Keep package.json Clean

- Remove unused dependencies
- Use exact versions when possible
- Avoid multiple packages for the same purpose
- Regular dependency audits

## What's Next

- [Stack Wizard](/docs/features/stack-wizard) - Build optimized stacks from scratch
- [Compatibility Checker](/docs/features/compatibility-checker) - Check specific pairs
- [API Reference](/docs/api/scan-endpoint) - Scan API documentation

---

**Scan your stack:** [devradar.dev/scan](https://devradar.dev/scan) →
