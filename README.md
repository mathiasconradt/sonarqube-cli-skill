# SonarQube CLI Skill

A Claude Code skill for working with the SonarQube CLI - analyze code, scan for secrets, manage authentication, list issues/projects, and integrate with Claude Code.

## Installation

Install the SonarQube CLI first:

**Linux/macOS:**
```bash
curl -o- https://raw.githubusercontent.com/SonarSource/sonarqube-cli/refs/heads/master/user-scripts/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://raw.githubusercontent.com/SonarSource/sonarqube-cli/refs/heads/master/user-scripts/install.ps1 | iex
```

## Quick Start

```bash
# Login to SonarQube Cloud
sonar auth login

# Integrate with Claude Code (global setup)
sonar integrate claude -g
```

## Documentation

- [SKILL.md](SKILL.md) - Quick reference and common commands
- [REFERENCE.md](REFERENCE.md) - Complete command reference and advanced workflows

## Features

- **Authentication Management** - Login, logout, and manage SonarQube tokens
- **Secrets Scanning** - Scan files and code for hardcoded secrets
- **Code Analysis** - Verify files and run SQAA analysis
- **Issue Management** - List and filter SonarQube issues
- **Project Management** - Search and list SonarQube projects
- **Integration** - Setup hooks and MCP server for Claude Code and Git

## Version

Compatible with SonarQube CLI v0.7.0
