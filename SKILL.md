---
name: sonar-cli
description: Work with SonarQube CLI - analyze code, scan for secrets, manage authentication, list issues/projects, and integrate with Claude Code. Use when the user asks about SonarQube, code quality, static analysis, code smells, vulnerability scanning, security scans, sonar issues, or integrating sonar with Claude.
metadata:
  version: "0.7.0"
  homepage: "https://github.com/SonarSource/sonarqube-cli"
  openclaw_emoji: "🔍"
  openclaw_requires_bins: "sonar"
---

# SonarQube CLI

A CLI for interacting with SonarQube/SonarCloud - analyze code quality, scan for secrets, manage projects and issues.

## Installation

**Linux/macOS:**
```bash
curl -o- https://raw.githubusercontent.com/SonarSource/sonarqube-cli/refs/heads/master/user-scripts/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://raw.githubusercontent.com/SonarSource/sonarqube-cli/refs/heads/master/user-scripts/install.ps1 | iex
```

## Common Commands

### Authentication
```bash
# Login (opens browser for interactive auth)
sonar auth login -o my-org

# Check authentication status
sonar auth status

# Logout
sonar auth logout
```

### Secrets Scanning
```bash
# Scan a file for hardcoded secrets
sonar analyze secrets ./src/config.js
# ✅ Success: "Scan completed successfully"
# ❌ Issues found: Exit code 1 with details

# Scan multiple files or directories
sonar analyze secrets ./src ./tests

# Scan from stdin with validation
echo "API_KEY=sk_test" | sonar analyze secrets --stdin && echo "No secrets found" || echo "Secrets detected"
```

### List Resources
```bash
# List issues for a project
sonar list issues -p my-project --severity CRITICAL

# List all projects
sonar list projects -q "my-app"
```

### Integration
```bash
# Integrate with Claude Code globally
sonar integrate claude -g

# Project-specific integration with project key
sonar integrate claude -p my-project
```

See [REFERENCE.md](REFERENCE.md) for complete command documentation, options, and advanced workflows.

## First-Time Setup Workflow
```bash
# 1. Login to SonarQube Cloud
sonar auth login -o my-org

# 2. Verify authentication
sonar auth status

# If authentication fails, recover with:
sonar auth purge                                           # Clear all tokens
sonar auth login -o my-org --with-token <your-token>      # Login with token
# Or for self-hosted:
sonar auth login -s https://sonarqube.company.com

# 3. Integrate with Claude Code (installs hooks and MCP server)
sonar integrate claude -g

# 4. Test secrets scanner (auto-installs on first use)
echo "test_key=abc123" | sonar analyze secrets --stdin
# ✅ Expected: "Scan completed successfully"
```

## Quick Troubleshooting

**Authentication:** Run `sonar auth status` to check connection. Use `sonar auth purge` and re-login if needed.

**Secrets scanner:** Auto-installs when you run `sonar analyze secrets` for the first time.

**Project not found:** List projects with `sonar list projects` or search by name with `-q "project-name"`.

For detailed options, advanced workflows, and comprehensive troubleshooting, see [REFERENCE.md](REFERENCE.md).
