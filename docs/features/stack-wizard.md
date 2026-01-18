---
title: Stack Wizard
description: Build your perfect tech stack with the 5-step guided wizard. Real-time compatibility feedback ensures all components work together.
---

# Stack Wizard

The Stack Wizard is a guided 5-step process that helps you build and validate a complete technology stack. Get real-time compatibility feedback at every step.

## Overview

The wizard adapts to your development approach:

- **Track A**: AI-Assisted Development - Select your AI tool first, then build around it
- **Track B**: Traditional Development - Start with your framework, then layer on services

Both tracks end with a comprehensive compatibility score for your complete stack.

## Step 1: Development Approach

Choose how you want to build:

| Option | Description | When to Choose |
|--------|-------------|----------------|
| **AI-Assisted** | You'll use an AI coding tool (Cursor, Windsurf, etc.) | You want AI-powered development |
| **Traditional** | Manual development with standard tooling | You prefer conventional workflows |

**Track A (AI-Assisted)** continues to AI tool selection first.
**Track B (Traditional)** continues to platform selection first.

## Step 2: Platform & Requirements

Define your application scope:

### Platform Type
- **Web** - Browser-based applications
- **Mobile** - iOS/Android native apps
- **Cross-Platform** - React Native, Flutter, etc.
- **API** - Backend services only
- **Desktop** - Electron, Tauri applications

### Key Requirements
Select what your application needs:

| Requirement | Description |
|-------------|-------------|
| **Server-Side Rendering** | For SEO and initial load performance |
| **Mobile Support** | Native iOS/Android apps |
| **Real-time Features** | WebSockets, live updates |
| **E-commerce** | Payment processing, inventory |
| **Database Required** | Data persistence needs |
| **Authentication** | User login/authorization |
| **File Storage** | User uploads, media handling |

Your selections filter available options in subsequent steps.

## Step 3A: AI Tool Selection (Track A Only)

If you chose AI-Assisted development, select your primary AI tool:

### Tool Categories
1. **Vibe Coding** - Lovable, Bolt.new (full-stack generation)
2. **AI IDEs** - Cursor, Windsurf, Juno (editor-integrated)
3. **CLI Agents** - Trae, Aider, Kiro (terminal-based)
4. **Managed Platforms** - Replit, Firebase Studio (cloud-based)

### Compatibility Insights
Each tool shows:
- **Framework Compatibility** - Which frameworks work best
- **Platform Support** - Web, mobile, CLI, etc.
- **Privacy Model** - Data handling and training policies
- **Pricing** - Free tier availability and plans

## Step 3B: Framework Selection (Track B Only)

If you chose Traditional development, select your framework:

### Web Frameworks
- **Next.js** - React meta-framework with SSR/SSG
- **Remix** - Nested routes, forms-first approach
- **SvelteKit** - Svelte-based, compile-time optimizations
- **Nuxt** - Vue meta-framework
- **Astro** - Content-focused, framework-agnostic

### Mobile Frameworks
- **React Native** - Cross-platform mobile
- **Flutter** - Dart-based mobile apps
- **NativeScript** - Native API access

### API Frameworks
- **Express** - Minimal Node.js framework
- **Fastify** - High-performance Node.js
- **tRPC** - End-to-end typesafe APIs

## Step 4: Full Stack Selection

Build your complete technology stack:

### Core Stack Components

| Component | Options |
|-----------|---------|
| **Framework** | Selected in previous step |
| **Database** | PostgreSQL, MongoDB, MySQL, SQLite, etc. |
| **Hosting** | Vercel, Netlify, AWS, Railway, etc. |
| **Auth** | Auth0, Clerk, Firebase Auth, NextAuth, etc. |
| **Styling** | Tailwind, CSS Modules, Styled Components, etc. |

### Real-Time Compatibility
As you select each component, the wizard shows:
- ✅ **Green** - Fully compatible with your current stack
- ⚠️ **Yellow** - Compatible with caveats
- ❌ **Red** - Known incompatibilities

Click any warning to see detailed explanations and workarounds.

## Step 5: Results & Project Creation

### Compatibility Report

Your complete stack receives an overall score:

```
Stack Compatibility Score: 87/100

✅ Framework + Database: Compatible
✅ Framework + Hosting: Compatible
⚠️  Framework + Auth: Partial (requires config)
✅ Database + Hosting: Compatible
```

### Issue Breakdown
- **Critical Issues** - Must resolve before production
- **Warnings** - Review before committing
- **Suggestions** - Optimization opportunities

### Save as Project
Click **Create Project** to save your stack:
- Access from dashboard anytime
- Add tasks and notes
- Track decisions and rationale
- Share with your team

## Tips for Best Results

### 1. Start Broad, Then Refine
Select your framework first, then narrow down database/hosting based on compatibility.

### 2. Read Warnings Carefully
Yellow status doesn't mean "broken" - it means "pay attention." Workarounds are provided.

### 3. Consider Your Team
Choose technologies your team already knows when scores are similar.

### 4. Check Privacy Policies
Some AI tools train on code. Verify this aligns with your requirements.

### 5. Save Early, Save Often
Create a project even before finalizing - you can always edit later.

## Example: Building a Full-Stack Web App

**Requirements:** Web app with SSR, user auth, PostgreSQL database

1. **Approach**: Traditional
2. **Platform**: Web + SSR required
3. **Framework**: Next.js (selected)
4. **Database**: Prisma + PostgreSQL (compatible ✅)
5. **Hosting**: Vercel (compatible ✅)
6. **Auth**: NextAuth.js (compatible ✅)

**Result**: 95/100 compatibility score - ready to build!

## What's Next

- [Compatibility Checker](/docs/features/compatibility-checker) - Quick pair verification
- [AI Tools Directory](/docs/features/ai-tools-directory) - Explore AI-assisted options
- [Project Management](/docs/features/project-management) - Track your stacks

---

**Ready to build?** [Start the Wizard](https://devradar.dev/wizard) →
