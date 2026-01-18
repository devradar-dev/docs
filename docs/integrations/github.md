---
title: GitHub Integration
description: Connect DevRadar with your GitHub repositories for automated compatibility checking in pull requests.
description: Upcoming feature for GitHub App integration with DevRadar.
---

# GitHub Integration

**Status:** Coming Soon

The DevRadar GitHub integration will automatically check compatibility in your pull requests, helping you catch issues before merge.

## Planned Features

### Pull Request Comments

Automatic compatibility checks when:
- `package.json` is modified
- New dependencies are added
- Technology stack changes

### Status Checks

GitHub status checks showing:
- Overall compatibility score
- Specific issues found
- Suggestions for fixes

### Repository Scanning

One-time and scheduled scans of your codebase:
- Detect all technologies in use
- Identify compatibility issues
- Track score over time

### README Badges

Auto-generated badges showing your project's compatibility status.

## How It Will Work

### Installation

1. Visit the GitHub Marketplace listing
2. Install DevRadar on your selected repositories
3. Configure which branches to check
4. Customize notification preferences

### Configuration

**`.devradar.yml`** (planned):

```yaml
# DevRadar configuration
branches:
  - main
  - develop

checks:
  - package.json
  - requirements.txt
  - Gemfile

notifications:
  pull_requests: true
  issues: false

score_threshold: 70  # Fail checks below this score
```

### Pull Request Flow

1. **Developer opens PR**
2. **DevRadar detects changes**
3. **Compatibility scan runs**
4. **Comment posted with results**
5. **Status check updated**

## Example PR Comment

```
## 🔍 DevRadar Compatibility Check

**Score:** 85/100 ⚠️

### Issues Found
| Severity | Pair | Issue |
|----------|------|-------|
| Warning | prisma-vercel | Edge Runtime not supported |
| Info | nextjs-14-auth | Consider upgrading to Auth.js |

### Suggestions
- Consider Turso for Edge-compatible database
- Review Auth.js migration guide

[View full report →](https://devradar.dev/scope/your-repo/pr/123)
```

## Stay Informed

This integration is under active development. To be notified when it launches:

1. **Watch the repository** - [github.com/devradar-dev/website](https://github.com/devradar-dev/website)
2. **Join the waitlist** - [devradar.dev/github-waitlist](https://devradar.dev/github-waitlist)
3. **Follow us** - [@devradardev](https://x.com/devradardev) on Twitter

## In the Meantime

While the GitHub integration is in development, you can:

- **Use the API** - Integrate compatibility checking into your CI/CD
- **Use the CLI** - Manual scanning of local projects (coming soon)
- **Use the web interface** - Check compatibility at [devradar.dev/check](https://devradar.dev/check)

### CI/CD Integration Example

```yaml
# .github/workflows/devradar-check.yml
name: DevRadar Compatibility Check

on: [pull_request]

jobs:
  compatibility:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Check compatibility
        run: |
          curl -X POST https://devradar.dev/api/v1/scan \
            -H "Content-Type: application/json" \
            -d '{"packageJson": $(cat package.json)}'
```

## Feedback Wanted

Help us prioritize features by sharing your needs:

- **What matters most?** PR comments, status checks, scanning?
- **Which file formats?** package.json, requirements.txt, others?
- **Any specific workflows?** Tell us about your process

Share feedback: [github.com/devradar-dev/website/discussions](https://github.com/devradar-dev/website/discussions)

## What's Next

- [CLI Integration](/docs/integrations/cli) - Command-line tool
- [API Reference](/docs/api/overview) - REST API docs
- [Webhooks](/docs/integrations/webhooks) - Webhook notifications

---

**Stay updated:** [Join the waitlist](https://devradar.dev/github-waitlist) →
