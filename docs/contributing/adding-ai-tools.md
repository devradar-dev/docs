---
title: Adding AI Tools
description: Submit new AI coding tools to the DevRadar directory. Help developers discover the best AI tools.
---

# Adding AI Tools

Contribute to the AI Tools Directory by submitting new tools or updating existing ones.

## Before You Submit

### Search for the Tool

1. Visit [devradar.dev/tools](https://devradar.dev/tools)
2. Search for the tool by name
3. Check if it already exists

### Verify Tool Information

Only submit tools you can verify:

- Official website exists
- Active development (updated within 6 months)
- Clear description of capabilities
- Pricing information available

## Required Information

| Field | Description | Example |
|-------|-------------|---------|
| **Tool Name** | Official name | `Cursor` |
| **Category** | Tool type | `ai-ide`, `cli-agent`, `vibe-coding`, `managed` |
| **Website** | Official URL | `https://cursor.sh` |
| **Description** | What it does | `AI code editor with GPT-4 integration` |
| **Platforms** | Supported platforms | `desktop`, `web`, `cli` |
| **Pricing** | Cost information | `Free tier available, $20/month Pro` |
| **Privacy** | Data handling | `Trains on code, cloud storage` |
| **Evidence** | Sources | Official docs, website, GitHub |

## Tool Categories

### AI IDEs
Code editors with integrated AI capabilities.

**Examples:** Cursor, Windsurf, Juno

### CLI Agents
Terminal-based AI coding assistants.

**Examples:** Aider, Trae, Kiro

### Vibe Coding
AI platforms that generate full applications from prompts.

**Examples:** Lovable, Bolt.new, v0

### Managed Platforms
Cloud-based development environments with AI.

**Examples:** Replit, Firebase Studio

## Capabilities to Document

| Capability | Description | How to Verify |
|------------|-------------|---------------|
| **SSR Support** | Works with server-side rendering | Check docs/examples |
| **Mobile** | Supports mobile development | Framework mentions |
| **Database** | Can handle database operations | Features list |
| **API** | API development support | Documentation |
| **Deployment** | Can deploy apps | Platform features |

### Verification Methods

```bash
# Check official documentation
# Look for framework mentions
# Search for tutorial examples
# Verify with product team if uncertain
```

## Privacy Information

Accurate privacy information is critical:

### Data Training

| Model | Description | Example |
|-------|-------------|---------|
| **Trains on code** | Code used for model training | "May use code for improvements" |
| **Local-only** | All processing on your machine | "Data never leaves your device" |
| **Optional** | User can choose | "Opt-in for training" |

### Data Storage

| Model | Description | Example |
|-------|-------------|---------|
| **Cloud** | Stored on provider servers | "Data stored securely in cloud" |
| **Local** | Stored on your device | "All data local" |
| **Hybrid** | Mixed approach | "Cache local, sync cloud" |

## Submission Template

### Method 1: GitHub Issue (Recommended)

```markdown
## AI Tool Submission

**Tool Name:** [Name]
**Category:** ai-ide / cli-agent / vibe-coding / managed

### Basic Information
- **Website:** [URL]
- **GitHub:** [URL if applicable]
- **Description:** [1-2 sentences]
- **Launch Date:** [Month/Year if known]

### Platforms
- [ ] Web
- [ ] Desktop (Windows/Mac/Linux)
- [ ] CLI
- [ ] Mobile

### Capabilities
- [ ] SSR Support (which frameworks?)
- [ ] Mobile Development
- [ ] Database Operations
- [ ] API Development
- [ ] Deployment

### Privacy
- **Data Training:** yes/no/optional
- **Data Storage:** cloud/local/hybrid
- **Enterprise Options:** yes/no

### Pricing
- **Free Tier:** yes/no (details)
- **Paid Plans:** [pricing information]
- **Starting Price:** [amount/period]

### Evidence
- Official website: [URL]
- Documentation: [URL]
- Pricing page: [URL]
- Privacy policy: [URL]

### Why This Tool?
[Why should developers know about this tool?]
```

### Method 2: API Submission

```bash
curl -X POST https://devradar.dev/api/v1/contributions/ai-tool \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "name": "Tool Name",
    "category": "ai-ide",
    "website": "https://...",
    "description": "...",
    "platforms": {"desktop": true, "web": false},
    "capabilities": {"ssr": true, "mobile": false},
    "privacy": {
      "trainsOnCode": true,
      "dataStorage": "cloud"
    },
    "pricing": {
      "free": true,
      "startingPrice": "$20/month"
    },
    "evidence": ["https://..."]
  }'
```

## Review Criteria

### Accuracy Checklist

- [ ] Tool name matches official branding
- [ ] Website is official/current
- [ ] Category is correct
- [ ] Capabilities are verified
- [ ] Privacy info is accurate
- [ ] Pricing is current
- [ ] Evidence links work

### Quality Checklist

- [ ] Description is clear and concise
- [ ] No marketing language (objective tone)
- [ ] Relevant details included
- [ ] Sources are authoritative

## Review Process

| Stage | Duration | Action |
|-------|----------|--------|
| **Initial Review** | 1-2 days | Verify completeness |
| **Verification** | 3-5 days | Test tool, check docs |
| **Approval** | Within 7 days | Add to directory |

### Status Updates

- **Received** - Submission acknowledged
- **Under Review** - Being verified
- **More Info Needed** - We have questions
- **Approved** - Added to directory
- **Declined** - Cannot accept (reason provided)

## Best Practices

### 1. Use Official Sources

```
Good: Official website, documentation, GitHub repo
Bad: Blog posts, news articles, third-party reviews
```

### 2. Be Current

```
Good: Information from last 3 months
Bad: Information from 2 years ago
```

### 3. Be Specific

```
Good: "Supports Next.js App Router and Pages Router"
Bad: "Works with Next.js"
```

### 4. Verify Privacy Claims

Privacy claims are critical - double-check:

```
Good: Checked privacy policy, confirmed data training
Bad: "I think they don't train on code"
```

### 5. Include Pricing Details

Vague pricing isn't helpful:

```
Good: "Free tier: 120 requests/day. Pro: $20/month unlimited."
Bad: "Has free and paid plans"
```

## Examples

### Example 1: AI IDE Submission

```markdown
## AI Tool Submission

**Tool Name:** Juno
**Category:** ai-ide

### Basic Information
- **Website:** https://junobuild.com
- **GitHub:** https://github.com/junobuild/juno
- **Description:** AI-native IDE for macOS with local-first privacy
- **Launch Date:** January 2025

### Platforms
- [x] Desktop (macOS only)
- [ ] Web
- [ ] CLI
- [ ] Mobile

### Capabilities
- [x] SSR Support (Next.js, SvelteKit)
- [ ] Mobile Development
- [x] Database Operations
- [x] API Development
- [ ] Deployment

### Privacy
- **Data Training:** no (local-only)
- **Data Storage:** local (with optional cloud backup)
- **Enterprise Options:** no

### Pricing
- **Free Tier:** yes (full features, community support)
- **Paid Plans:** Pro ($29/month) with priority support
- **Starting Price:** $29/month

### Evidence
- Official website: https://junobuild.com
- Documentation: https://junobuild.com/docs
- Privacy policy: https://junobuild.com/privacy

### Why This Tool?
Juno is the first AI IDE with truly local processing - important for
developers who can't use cloud-based tools due to company policies.
```

### Example 2: CLI Agent Submission

```markdown
## AI Tool Submission

**Tool Name:** Kiro
**Category:** cli-agent

### Basic Information
- **Website:** https://kiro.dev
- **GitHub:** https://github.com/kiro-dev/kiro
- **Description:** Fast, lightweight CLI AI agent for quick edits
- **Launch Date:** November 2024

### Platforms
- [ ] Web
- [ ] Desktop
- [x] CLI (macOS, Linux)
- [ ] Mobile

### Capabilities
- [x] SSR Support (all frameworks)
- [x] Mobile Development (React Native, Flutter)
- [x] Database Operations (any ORM)
- [x] API Development
- [ ] Deployment

### Privacy
- **Data Training:** optional (opt-in)
- **Data Storage:** local (optional cloud sync)
- **Enterprise Options:** yes (self-hosted available)

### Pricing
- **Free Tier:** yes (open source)
- **Paid Plans:** None (donations accepted)
- **Starting Price:** Free

### Evidence
- Official website: https://kiro.dev
- GitHub: https://github.com/kiro-dev/kiro
- Documentation: https://kiro.dev/docs

### Why This Tool?
Kiro fills a gap for developers who want CLI AI assistance without
the overhead of heavier tools like Aider.
```

## Updating Existing Tools

Tool information changes - help keep it current:

```markdown
## Tool Update

**Tool:** [Tool Name]
**Update Type:** pricing / capabilities / privacy / general

### What Changed?
[Description of changes]

### New Information
[Updated details]

### Evidence
[Sources for new information]
```

## What's Next

- [Adding Compatibility Rules](/docs/contributing/adding-compatibility-rules) - Submit compatibility data
- [Contributing Overview](/docs/contributing/overview) - Other ways to contribute

---

**Submit now:** [Open an Issue](https://github.com/devradar-dev/website/issues/new) →
