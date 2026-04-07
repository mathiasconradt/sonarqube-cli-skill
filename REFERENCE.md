# SonarQube CLI Reference

Complete command reference for the SonarQube CLI.

## Authentication (`sonar auth`)

### Login
Save authentication token to keychain. Opens browser for interactive login unless token is provided.

```bash
# Interactive login (opens browser)
sonar auth login

# SonarQube Cloud with organization
sonar auth login -o my-org

# Self-hosted SonarQube
sonar auth login -s https://sonarqube.mycompany.com

# Non-interactive with token
sonar auth login -t <token> -o my-org
```

**Options:**
- `-s, --server <server>` - SonarQube URL (default: https://sonarcloud.io)
- `-o, --org <org>` - Organization key (required for SonarQube Cloud)
- `-t, --with-token <token>` - Token value (skips browser, non-interactive)

### Logout
Remove authentication token from keychain.

```bash
# Logout from default server
sonar auth logout

# Logout from specific server/org
sonar auth logout -s https://sonarcloud.io -o my-org
```

**Options:**
- `-s, --server <server>` - SonarQube server URL
- `-o, --org <org>` - Organization key

### Status
Show active authentication connection with token verification.

```bash
sonar auth status
```

### Purge
Remove all authentication tokens from keychain (interactive).

```bash
sonar auth purge
```

## Installation Management (`sonar install`)

### Install Secrets Scanner
Install sonar-secrets binary from binaries.sonarsource.com.

```bash
# Install secrets scanner
sonar install secrets

# Check installation status
sonar install secrets --status

# Force reinstall
sonar install secrets --force
```

**Options:**
- `--force` - Force reinstall even if already installed
- `--status` - Check installation status instead of installing

## Integration (`sonar integrate`)

### Claude Code Integration
Setup SonarQube integration for Claude Code with secrets scanning hooks and MCP server.

```bash
# Interactive setup (will prompt for server, project, etc.)
sonar integrate claude

# Global installation (installs to ~/.claude for all projects)
sonar integrate claude -g

# Non-interactive with all parameters
sonar integrate claude -s https://sonarcloud.io -p my-project -o my-org -t <token> --non-interactive

# Project-specific installation
sonar integrate claude -p my-project

# Skip hooks installation (MCP server only)
sonar integrate claude --skip-hooks
```

**Options:**
- `-s, --server <server>` - SonarQube server URL
- `-p, --project <project>` - Project key
- `-t, --token <token>` - Existing authentication token
- `-o, --org <org>` - Organization key (for SonarQube Cloud)
- `--non-interactive` - Non-interactive mode (no prompts)
- `--skip-hooks` - Skip hooks installation
- `-g, --global` - Install to ~/.claude instead of project directory

## List Resources (`sonar list`)

### List Issues
Search for issues in SonarQube.

```bash
# List all issues for a project
sonar list issues -p my-project

# Filter by severity
sonar list issues -p my-project --severity BLOCKER
sonar list issues -p my-project --severity CRITICAL
sonar list issues -p my-project --severity MAJOR

# Specific branch
sonar list issues -p my-project --branch main

# Pull request issues
sonar list issues -p my-project --pull-request 123

# Pagination
sonar list issues -p my-project --page 2 --page-size 100

# Different output format
sonar list issues -p my-project --format toon
```

**Options:**
- `-p, --project <project>` - Project key (required)
- `--severity <severity>` - Filter by severity (BLOCKER, CRITICAL, MAJOR, MINOR, INFO)
- `--format <format>` - Output format: json (default), toon
- `--branch <branch>` - Branch name
- `--pull-request <pr-id>` - Pull request ID
- `--page-size <size>` - Page size (1-500, default: 500)
- `--page <number>` - Page number (default: 1)

### List Projects
Search for projects in SonarQube.

```bash
# List all projects
sonar list projects

# Search by name or key
sonar list projects -q "my-app"

# Pagination
sonar list projects --page 2 --page-size 50
```

**Options:**
- `-q, --query <query>` - Search query to filter by name or key
- `--page <number>` - Page number (default: 1)
- `--page-size <size>` - Page size (1-500, default: 500)

## Code Analysis

### Secrets Scanning
Scan files or stdin for hardcoded secrets.

```bash
# Scan a specific file
sonar analyze secrets --file ./src/config.js

# Scan from stdin
echo "API_KEY=sk_test_12345" | sonar analyze secrets --stdin

# Scan multiple files with shell expansion
for file in src/**/*.js; do sonar analyze secrets --file "$file"; done
```

**Options:**
- `--file <file>` - File path to scan for secrets
- `--stdin` - Read from standard input instead of a file

**Note:** Either `--file` or `--stdin` must be specified, not both.

## Configuration (`sonar config`)

### Telemetry Settings
Configure telemetry to control anonymous usage statistics.

```bash
# Enable telemetry
sonar config telemetry --enabled

# Disable telemetry
sonar config telemetry --disabled
```

**Options:**
- `--enabled` - Enable collection of anonymous usage statistics
- `--disabled` - Disable collection of anonymous usage statistics

## Advanced Workflows

### Multi-Environment Setup
```bash
# Login to production SonarQube Cloud
sonar auth login -s https://sonarcloud.io -o prod-org

# Login to staging self-hosted instance
sonar auth login -s https://staging.sonarqube.internal

# Verify both connections are active
sonar auth status
# The status output shows which server/org combination is currently active
# Use the appropriate -s and -o flags in subsequent commands to target the right environment
```

## Troubleshooting

### Authentication issues
```bash
# Check current authentication status
sonar auth status

# Purge all tokens and re-login
sonar auth purge
sonar auth login
```

### Secrets scanner not found
```bash
# Check installation status
sonar install secrets --status

# Reinstall
sonar install secrets --force
```

### Project not found
```bash
# List all available projects
sonar list projects

# Search for specific project
sonar list projects -q "project-name"
```

## Common Options

Many commands accept these standard flags:
- `-s, --server <server>` - SonarQube URL (default: https://sonarcloud.io)
- `-o, --org <org>` - Organization key (required for SonarQube Cloud)
- `-p, --project <project>` - Project key

## Environment Variables

The CLI may respect standard SonarQube environment variables:
- `SONAR_TOKEN` - Authentication token
- `SONAR_HOST_URL` - Server URL

## Exit Codes
- **0** - Success
- **1** - Error (validation, execution, or other failure)
