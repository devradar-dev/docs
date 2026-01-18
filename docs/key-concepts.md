---
title: Key Concepts
description: Understanding compatibility scores, status levels, severity types, and technology categories in DevRadar.
---

# Key Concepts

Understanding these core concepts will help you get the most out of DevRadar.

## Compatibility Score

The **Compatibility Score** (0-100) represents how well two technologies work together:

| Score Range | Meaning |
|-------------|---------|
| **90-100** | Excellent compatibility - works seamlessly |
| **70-89** | Good compatibility - minor considerations |
| **50-69** | Partial compatibility - some limitations |
| **30-49** | Limited compatibility - significant workarounds needed |
| **0-29** | Poor compatibility - not recommended together |

Scores are calculated based on:
- Official documentation support
- Community adoption patterns
- Known integration issues
- Performance benchmarks
- Security considerations

## Status Levels

Each compatibility check returns one of five status levels:

### Compatible ✅
The technologies work together as expected. No major issues known.

```json
{
  "status": "compatible",
  "message": "Next.js works seamlessly with Prisma ORM."
}
```

### Partial ⚠️
The technologies can work together, but with limitations or configuration requirements.

```json
{
  "status": "partial",
  "message": "Works with additional configuration required for SSR.",
  "workaround": "Configure dynamic imports for client-only components."
}
```

### Incompatible ❌
The technologies have fundamental conflicts and should not be used together.

```json
{
  "status": "incompatible",
  "message": "This framework requires a different rendering approach."
}
```

### Deprecated 📜
The combination is technically functional but one or both technologies are end-of-life.

```json
{
  "status": "deprecated",
  "message": "Consider migrating to actively maintained alternatives."
}
```

### Unknown ❓
Insufficient data to determine compatibility. Community contributions welcome!

```json
{
  "status": "unknown",
  "message": "No compatibility data available yet."
}
```

## Severity Levels

When issues are found, they're categorized by severity:

| Severity | Description | Color |
|----------|-------------|-------|
| **Info** | Useful information, no blocking issue | Blue |
| **Warning** | Caution advised - may need attention | Yellow |
| **Error** | Significant issue - workaround recommended | Orange |
| **Critical** | Blocking issue - not recommended for production | Red |

## Technology Categories

DevRadar organizes technologies into categories for structured evaluation:

### Frameworks
Web frameworks and meta-frameworks for building user interfaces:
- Next.js, Remix, SvelteKit, Nuxt, Astro
- React, Vue, Svelte, Angular
- Solid, Qwik, Redwood

### Databases
Data persistence solutions:
- **SQL**: PostgreSQL, MySQL, SQLite, SQL Server
- **NoSQL**: MongoDB, Firebase, CouchDB
- **Edge**: Turso, Neon, PlanetScale
- **Search**: Algolia, Meilisearch, Typesense

### Hosting Platforms
Deployment and infrastructure:
- **Vercel**, Netlify, Cloudflare Pages
- **AWS**, Google Cloud, Azure
- **Railway**, Fly.io, Render
- **Firebase Hosting**, Supabase

### Auth Providers
Authentication and authorization:
- **Standalone**: Auth0, Clerk, PropelAuth
- **BaaS**: Firebase Auth, Supabase Auth
- **Library**: NextAuth.js, Lucia

### Styling
CSS and UI solutions:
- **Utilities**: Tailwind CSS, UnoCSS
- **CSS-in-JS**: Styled Components, Emotion
- **Component**: MUI, Chakra UI, Shadcn/UI

## AI Tool Categories

AI-assisted development tools are organized differently:

| Category | Examples | Primary Use |
|----------|----------|-------------|
| **Vibe Coding** | Lovable, Bolt.new | AI-powered full-stack generation |
| **AI IDE** | Cursor, Windsurf | AI-integrated code editors |
| **CLI Agent** | Trae, Aider, Kiro | Terminal-based AI assistants |
| **Managed Platform** | Replit, Firebase Studio | Cloud-based development environments |

## Project Types

Different development approaches have different compatibility considerations:

| Type | Description | Key Considerations |
|------|-------------|-------------------|
| **Web** | Traditional web applications | SSR, SEO, hydration |
| **Mobile** | iOS/Android apps | Native modules, performance |
| **Cross-Platform** | React Native, Flutter | Platform-specific APIs |
| **API** | Backend services | Database, hosting, auth only |
| **Desktop** | Electron, Tauri | OS-level integrations |

## Understanding Badges

Compatibility badges provide quick visual reference:

```markdown
![Compatibility](https://devradar.dev/api/v1/badge?techA=nextjs&techB=prisma)
```

Badge colors match status levels:
- 🟢 Green = Compatible
- 🟡 Yellow = Partial
- 🔴 Red = Incompatible
- ⚪ Gray = Unknown/Deprecated

## What's Next

- [Stack Wizard Guide](/docs/features/stack-wizard) - Building stacks with confidence
- [Compatibility Checker](/docs/features/compatibility-checker) - Deep dive into pair checking
- [API Reference](/docs/api/overview) - Integrate into your workflow

---

**Still have questions?** [Check our Features docs](/docs/features/stack-wizard) or [join the community](https://github.com/startupsolellc/stackpilot).
