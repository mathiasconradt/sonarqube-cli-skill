# SonarQube CLI Skill

> A [Claude Code](https://claude.ai/code) skill for working with the SonarQube CLI - analyze code quality, scan for secrets, manage authentication, and integrate security analysis into your development workflow.

## Features

🔐 **Secrets Scanning** - Detect hardcoded secrets and credentials in your code
🔍 **Code Analysis** - Run SQAA and file verification with SonarQube Cloud
🎯 **Issue Management** - List and filter SonarQube issues by severity and branch
📊 **Project Management** - Search and explore SonarQube projects
🔑 **Authentication** - Seamless login and token management
🪝 **Git Hooks** - Automatic secrets scanning on commit/push
🤖 **Claude Integration** - MCP server and hooks for Claude Code

## Prerequisites

**SonarQube CLI v0.7.0+**

Install the SonarQube CLI first:

<details>
<summary><strong>Linux/macOS</strong></summary>

```bash
curl -o- https://raw.githubusercontent.com/SonarSource/sonarqube-cli/refs/heads/master/user-scripts/install.sh | bash
```
</details>

<details>
<summary><strong>Windows (PowerShell)</strong></summary>

```powershell
irm https://raw.githubusercontent.com/SonarSource/sonarqube-cli/refs/heads/master/user-scripts/install.ps1 | iex
```
</details>

Verify installation:
```bash
sonar --version  # Should show 0.7.0 or higher
```

## Installation

### Install as Claude Code Skill

```bash
# Clone this skill to your Claude skills directory
git clone https://github.com/yourusername/sonar-cli-skill ~/.claude/skills/sonar-cli

# Or use the Claude Code skills system (if available)
claude skills install sonar-cli
```

## Quick Start

### 1. Initial Setup

```bash
# Login to SonarQube Cloud
sonar auth login -o your-org

# Verify authentication
sonar auth status

# Integrate with Claude Code (installs hooks and MCP server)
sonar integrate claude -g
```

### 2. Scan for Secrets

```bash
# Scan a single file
sonar analyze secrets ./src/config.js

# Scan multiple files or directories
sonar analyze secrets ./src ./tests

# Scan from stdin
cat .env | sonar analyze secrets --stdin
```

### 3. List Issues

```bash
# List critical issues in a project
sonar list issues -p my-project --severity CRITICAL

# Filter by branch
sonar list issues -p my-project --branch main
```

### 4. Use in Claude Code

Once integrated, Claude Code can automatically:
- Scan files for secrets before committing
- Query SonarQube for issues and code quality metrics
- Analyze code with SQAA (SonarQube AI Agent)
- List projects and issues directly in conversation

Example Claude Code interaction:
```
You: "Scan this file for secrets"
Claude: [Uses sonar analyze secrets automatically]

You: "What are the critical issues in my-project?"
Claude: [Uses sonar list issues with severity filter]
```

## Documentation

- **[SKILL.md](SKILL.md)** - Quick reference and common commands
- **[REFERENCE.md](REFERENCE.md)** - Complete command reference and advanced workflows

## Usage Examples

### Authentication Management

```bash
# Interactive login (opens browser)
sonar auth login -o my-org

# Login with token (non-interactive)
sonar auth login -o my-org --with-token <your-token>

# Self-hosted SonarQube
sonar auth login -s https://sonarqube.company.com

# Check status
sonar auth status

# Logout
sonar auth logout
```

### Secrets Scanning with Validation

```bash
# Scan and validate results
sonar analyze secrets ./src && echo "✅ Clean" || echo "❌ Secrets found"

# Scan stdin and check exit code
echo "API_KEY=test123" | sonar analyze secrets --stdin
# Exit code 0 = no secrets, 1 = secrets detected
```

### Git Integration

```bash
# Install pre-commit hook (scans staged files)
sonar integrate git --hook pre-commit

# Install pre-push hook (scans unpushed commits)
sonar integrate git --hook pre-push

# Install globally for all repositories
sonar integrate git --hook pre-commit --global
```

### Code Analysis

```bash
# Verify a file (server-side analysis)
sonar verify --file ./src/app.js --branch main

# SQAA analysis (AI-powered, SonarQube Cloud only)
sonar analyze sqaa --file ./src/app.js --project my-project
```

## Troubleshooting

### Authentication Issues

```bash
# Clear all tokens and re-login
sonar auth purge
sonar auth login -o your-org
```

### Secrets Scanner Not Found

The secrets scanner auto-installs on first use. If you see an error:
1. Ensure you have internet connectivity
2. Run any `sonar analyze secrets` command
3. The scanner will download automatically (sonar-secrets v2.41.0+)

### Project Not Found

```bash
# List all available projects
sonar list projects

# Search by name
sonar list projects -q "my-project-name"
```

## Configuration

### Telemetry

Control anonymous usage statistics:

```bash
# Enable telemetry
sonar config telemetry --enabled

# Disable telemetry
sonar config telemetry --disabled
```

### Environment Variables

The CLI respects standard SonarQube environment variables:

```bash
export SONAR_TOKEN="your-token"
export SONAR_HOST_URL="https://sonarcloud.io"
```

## Contributing

Contributions are welcome! This skill wraps the official [SonarQube CLI](https://github.com/SonarSource/sonarqube-cli).

### Reporting Issues

- **Skill issues**: Open an issue in this repository
- **CLI issues**: Report to [SonarQube CLI repo](https://github.com/SonarSource/sonarqube-cli/issues)

### Development

```bash
# Test the skill locally
cd ~/.claude/skills/sonar-cli

# Verify all commands work
sonar --help
sonar auth status
```

## License

This skill is provided under the MIT License. The SonarQube CLI is maintained by [SonarSource](https://www.sonarsource.com/).

## Resources

- [SonarQube CLI Documentation](https://github.com/SonarSource/sonarqube-cli)
- [SonarCloud](https://sonarcloud.io)
- [Claude Code Documentation](https://claude.ai/code)
- [Skill Development Guide](https://docs.anthropic.com/claude-code/skills)

## Version

**Current Version**: 0.7.0
**Compatible with**: SonarQube CLI v0.7.0+
**Tested with**: Claude Code CLI, Desktop, and Web

---

Made with 🔍 for secure, high-quality code analysis in Claude Code
