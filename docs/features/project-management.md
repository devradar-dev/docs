---
title: Project Management
description: Track multiple tech stacks, manage compatibility scores, and document decisions. Your personal stack management system.
---

# Project Management

The Project Management system helps you track multiple tech stacks, monitor compatibility, and document your technical decisions over time.

## Overview

Access your projects from the **Dashboard** or manage individual projects from the **Projects** page.

## Creating a Project

### From the Wizard

After completing the Stack Wizard:
1. Review your compatibility score
2. Click **Create Project**
3. Name your project
4. Add optional description
5. Click **Save Project**

### From the Dashboard

1. Click **New Project** on the dashboard
2. Enter project name and description
3. Add your technologies manually
4. Click **Create**

## Project Dashboard

Your main project view shows:

### Active Projects
All your saved projects with:
- Project name and description
- Primary framework/stack
- Compatibility score badge
- Last updated timestamp
- Quick actions (edit, delete, share)

### Project Stats
- **Total Projects** - Number of saved stacks
- **Average Score** - Mean compatibility across projects
- **Critical Issues** - Projects with incompatibilities
- **Recent Updates** - Latest modifications

## Project Details

Click any project to see comprehensive information:

### Stack Summary
Your complete technology stack with compatibility indicators:

```
My E-commerce App
Compatibility: 87/100

Framework:     Next.js ✅
Database:      PostgreSQL ✅
Hosting:       Vercel ✅
Auth:          NextAuth.js ⚠️
Styling:       Tailwind CSS ✅
```

### Compatibility Breakdown
Each technology pair shows:
- **Status** - Compatible, Partial, or Incompatible
- **Severity** - Info, Warning, Error, Critical
- **Message** - Explanation of the compatibility
- **Workaround** - Suggested fixes for issues

## Tasks

Track your implementation progress with built-in task management:

### Adding Tasks
1. Navigate to your project
2. Click **Add Task** in the Tasks section
3. Enter task description
4. Set priority (Low, Medium, High)
5. Click **Add**

### Task States
- **Pending** - Not started
- **In Progress** - Currently working
- **Completed** - Finished

### Example Tasks
- "Configure NextAuth.js for edge runtime"
- "Set up PostgreSQL connection pooling"
- "Implement error handling for Stripe webhooks"

## Notes & Decisions

Document your technical decisions for future reference:

### Adding Notes
1. Scroll to the Notes section
2. Click **Add Note**
3. Enter your note content
4. Optionally tag with a technology
5. Click **Save**

### Note Types
- **Decision** - Why you chose X over Y
- **Configuration** - Important setup details
- **Issue** - Problems encountered and solutions
- **Idea** - Future improvements to consider

### Example Notes
```
DECISION: Chose Supabase over Firebase because:
- Better PostgreSQL support
- Real-time subscriptions built-in
- Lower costs at our scale

CONFIGURATION: Next.js edge runtime required for Clerk auth
```

## Activity Timeline

Track all changes to your project:

| Timestamp | Action | Details |
|-----------|--------|---------|
| 2025-01-15 10:30 | Stack Updated | Added Prisma ORM |
| 2025-01-14 15:45 | Task Completed | "Set up auth" |
| 2025-01-13 09:00 | Project Created | Initial stack |

## Sharing Projects

Share your stack with team members or the community:

### Private Sharing
1. Open your project
2. Click **Share**
3. Copy the private link
4. Share with your team (requires DevRadar login)

### Public Sharing
1. Open your project
2. Click **Make Public**
3. Copy the public URL
4. Share anywhere - no login required

### Embed Badge
Add a compatibility badge to your README:

```markdown
![Stack](https://devradar.dev/api/v1/badge?project=your-project-id)
```

## Project Settings

Configure your project settings:

- **Project Name** - Edit at any time
- **Description** - Update as your project evolves
- **Visibility** - Toggle between private and public
- **Archive** - Hide from active dashboard (kept for reference)

## Deleting Projects

Remove projects you no longer need:

1. Open the project
2. Click **Settings** (gear icon)
3. Scroll to **Danger Zone**
4. Click **Delete Project**
5. Confirm deletion

⚠️ **Warning:** Deletion is permanent. Export important data first.

## Best Practices

### 1. Document Your Decisions
Use notes to capture the "why" behind technology choices. Your future self will thank you.

### 2. Update Regularly
When you modify your stack, update the project in DevRadar. Compatibility scores change as technologies evolve.

### 3. Use Tasks for Tracking
Don't just track technologies - track the work needed to implement them properly.

### 4. Share Early
Share your project with team members before implementation to get feedback on compatibility.

### 5. Archive, Don't Delete
Completed projects can be archived rather than deleted. Keep the history for reference.

## What's Next

- [Stack Wizard](/docs/features/stack-wizard) - Create a new project
- [Compatibility Checker](/docs/features/compatibility-checker) - Verify additions
- [API Reference](/docs/api/overview) - Programmatic project access

---

**Manage your projects** [Go to Dashboard](https://devradar.dev/dashboard) →
