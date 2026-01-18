---
title: CLI Integration
description: Command-line interface for DevRadar. Check compatibility from your terminal without leaving your workflow.
description: Upcoming CLI tool for local development.
---

# CLI Integration

**Status:** Coming Soon

The DevRadar CLI will bring compatibility checking directly to your terminal. Check stacks without leaving your development environment.

## Planned Features

### Quick Check

Verify technology pairs instantly:

```bash
devradar check nextjs prisma
```

### Project Scan

Analyze your current project:

```bash
devradar scan
```

### Interactive Mode

Step-by-step stack builder:

```bash
devradar wizard
```

### CI/CD Integration

Run checks in your pipelines:

```bash
devradar ci --threshold=70
```

## Installation

**Planned installation methods:**

```bash
# npm
npm install -g @devradar/cli

# yarn
yarn global add @devradar/cli

# Homebrew (macOS/Linux)
brew install devradar

# Scoop (Windows)
scoop bucket add devradar
scoop install devradar
```

## Usage Examples

### Check Command

```bash
# Basic check
devradar check nextjs prisma

# Verbose output
devradar check nextjs prisma --verbose

# JSON output for scripting
devradar check nextjs prisma --json

# Check with specific version
devradar check "nextjs@14" "prisma@5"
```

**Example output:**

```
✓ nextjs + prisma: Compatible

Next.js works seamlessly with Prisma ORM for server-side rendering
and API routes.

Score: 95/100
```

### Scan Command

```bash
# Scan current directory
devradar scan

# Scan specific file
devradar scan --file package.json

# Scan with score threshold
devradar scan --fail-below 70

# Output report to file
devradar scan --output report.json
```

**Example output:**

```
Scanning project...

Detected: nextjs, prisma, vercel, tailwind, next-auth

Stack Score: 85/100

Issues:
  ⚠️  prisma + vercel (Edge Runtime not supported)
      → Use dynamic imports with 'ssr: false'

Suggestions:
  → Consider Turso for Edge-compatible database
  → 1 combination requires runtime configuration
```

### Wizard Command

```bash
# Start interactive wizard
devradar wizard

# Load from existing project
devradar wizard --from-project

# Save results to file
devradar wizard --output stack.json
```

### CI Command

```bash
# Run in CI/CD pipeline
devradar ci

# Custom threshold
devradar ci --threshold=70

# Generate report artifact
devradar ci --output ci-report.json
```

**CI behavior:**
- Exits with code 1 if score below threshold
- Outputs results in CI-friendly format
- Supports all major CI platforms

## CI/CD Integration

### GitHub Actions

```yaml
name: Compatibility Check

on: [pull_request]

jobs:
  devradar:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install DevRadar CLI
        run: npm install -g @devradar/cli
      - name: Check compatibility
        run: devradar ci --threshold=70
```

### GitLab CI

```yaml
compatibility:
  script:
    - npm install -g @devradar/cli
    - devradar ci --threshold=70
  only:
    - merge_requests
```

### Jenkins Pipeline

```groovy
pipeline {
    stages {
        stage('Compatibility') {
            steps {
                sh 'npm install -g @devradar/cli'
                sh 'devradar ci --threshold=70'
            }
        }
    }
}
```

## Configuration

**`.devradarrc`** (planned):

```json
{
  "threshold": 70,
  "failOnError": true,
  "ignore": ["devDependencies"],
  "output": "json",
  "exclude": ["eslint", "prettier"]
}
```

**`package.json`** (planned):

```json
{
  "scripts": {
    "check": "devradar scan",
    "check:ci": "devradar ci --threshold=70"
  },
  "devradar": {
    "threshold": 70,
    "exclude": ["eslint"]
  }
}
```

## Stay Informed

This CLI is under active development. To be notified when it launches:

1. **Watch the repository** - [github.com/devradar-dev/website](https://github.com/devradar-dev/website)
2. **Join the waitlist** - [devradar.dev/cli-waitlist](https://devradar.dev/cli-waitlist)
3. **Follow us** - [@DevRadar](https://twitter.com/devradar) on Twitter

## In the Meantime

While the CLI is in development, you can:

- **Use the API** - Build your own CLI wrapper around the REST API
- **Use the web interface** - Check compatibility at [devradar.dev/check](https://devradar.dev/check)

### Simple Wrapper Example

```bash
# Add to your .bashrc or .zshrc
devradar() {
  local techA=$1
  local techB=$2
  curl -s -X POST https://devradar.dev/api/v1/check \
    -H "Content-Type: application/json" \
    -d "{\"techA\":\"$techA\",\"techB\":\"$techB\"}" \
    | jq -r '.message'
}
```

## Feedback Wanted

Help us prioritize features:

- **Which platforms?** macOS, Linux, Windows priority?
- **Package managers?** npm, Homebrew, others?
- **Output formats?** Human-readable, JSON, others?
- **Must-have features?** What's blocking your adoption?

Share feedback: [github.com/devradar-dev/website/discussions](https://github.com/devradar-dev/website/discussions)

## What's Next

- [API Reference](/docs/api/overview) - REST API docs
- [GitHub Integration](/docs/integrations/github) - GitHub App
- [Webhooks](/docs/integrations/webhooks) - Webhook notifications

---

**Stay updated:** [Join the waitlist](https://devradar.dev/cli-waitlist) →
