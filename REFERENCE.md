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
sonar auth login --with-token <token> -o my-org
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
- `-o, --org <org>` - SonarQube Cloud organization key (required for SonarQube Cloud)

### Status
Show active authentication connection with token verification.

```bash
sonar auth status
```

**Options:**
- None - displays information about the current authentication status

### Purge
Remove all authentication tokens from keychain (interactive).

```bash
sonar auth purge
```

**Options:**
- None - prompts for confirmation before removing all tokens

## Integration (`sonar integrate`)

### Claude Code Integration
Setup SonarQube integration for Claude Code with secrets scanning hooks and MCP server.

```bash
# Interactive setup (will prompt for project details)
sonar integrate claude

# Global installation (installs to ~/.claude for all projects)
sonar integrate claude -g

# Non-interactive with project key
sonar integrate claude -p my-project --non-interactive

# Project-specific installation
sonar integrate claude -p my-project
```

**Options:**
- `-p, --project <project>` - Project key
- `--non-interactive` - Non-interactive mode (no prompts)
- `-g, --global` - Install to ~/.claude instead of project directory

**Note:** Authentication must be set up separately using `sonar auth login` before running integration.

### Git Integration
Install git hooks that scan for secrets before commits or pushes.

```bash
# Install pre-commit hook (scans staged files)
sonar integrate git --hook pre-commit

# Install pre-push hook (scans unpushed commits)
sonar integrate git --hook pre-push

# Force overwrite existing hook
sonar integrate git --hook pre-commit --force

# Install globally for all repositories
sonar integrate git --hook pre-commit --global

# Non-interactive mode
sonar integrate git --hook pre-commit --non-interactive
```

**Options:**
- `--hook <type>` - Hook type: `pre-commit` (scan staged files) or `pre-push` (scan files in unpushed commits)
- `--force` - Overwrite existing hook even if not created by sonar
- `--non-interactive` - Non-interactive mode (no prompts)
- `--global` - Install hook globally for all repositories (sets `git config --global core.hooksPath`)

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

### Verify File
Analyze a file for issues using SonarQube Cloud's server-side analysis.

```bash
# Analyze a file with auto-detected project
sonar verify --file ./src/app.js

# Analyze with specific branch context
sonar verify --file ./src/app.js --branch main

# Specify project key explicitly
sonar verify --file ./src/app.js --project my-project
```

**Options:**
- `--file <file>` - File path to analyze (required)
- `--branch <branch>` - Branch name for analysis context
- `--project <project>` - SonarQube Cloud project key (overrides auto-detection)

### SQAA Analysis
Run SQAA (SonarQube AI Agent) server-side analysis on a file. SonarQube Cloud only.

```bash
# Analyze a file with SQAA
sonar analyze sqaa --file ./src/app.js

# Analyze with branch context
sonar analyze sqaa --file ./src/app.js --branch main

# Specify project explicitly
sonar analyze sqaa --file ./src/app.js --project my-project
```

**Options:**
- `--file <file>` - File path to analyze (required)
- `--branch <branch>` - Branch name for analysis context
- `--project <project>` - SonarQube Cloud project key (overrides auto-detection)

### Secrets Scanning
Scan files or stdin for hardcoded secrets.

```bash
# Scan a specific file
sonar analyze secrets ./src/config.js

# Scan multiple files or directories
sonar analyze secrets ./src ./tests

# Scan with glob pattern (requires shell expansion)
sonar analyze secrets ./src/**/*.js

# Scan from stdin
echo "API_KEY=sk_test_12345" | sonar analyze secrets --stdin
```

**Arguments:**
- `[paths...]` - File or directory paths to scan for secrets

**Options:**
- `--stdin` - Read from standard input instead of file paths

**Note:** Provide either file paths or use `--stdin`, not both.

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

## Self-Update (`sonar self-update`)

Update the SonarQube CLI to the latest version.

```bash
# Update to latest version
sonar self-update

# Check for updates without installing
sonar self-update --status

# Force reinstall even if up to date
sonar self-update --force
```

**Options:**
- `--status` - Check for a newer version without installing
- `--force` - Install the latest version even if already up to date

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
The secrets scanner is automatically downloaded and installed when you first run `sonar analyze secrets`. No manual installation is required.

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
