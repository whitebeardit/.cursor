# Cursor Configuration Repository

Comprehensive configuration for Cursor IDE to maximize development productivity. This repository provides specialized subagents, reusable skills, custom commands, and development rules organized by technology stack.

## Overview

This configuration repository is designed to be linked/synchronized to `.cursor/` directories in your projects. It provides:

- **Subagents**: Specialized AI assistants for specific tasks (logging, testing, security, etc.)
- **Skills**: Reusable knowledge and patterns organized by language
- **Commands**: Custom commands for development workflows
- **Rules**: Development guidelines and conventions

## Structure

```
.cursor-config/
├── AGENTS.md                    # Minimal root configuration
├── README.md                     # This file
│
├── agents/                      # Subagents organized by language
│   ├── dotnet/                  # .NET specific subagents
│   ├── nodejs/                  # Node.js specific subagents
│   └── shared/                  # Multi-language subagents
│
├── skills/                      # Reusable skills organized by language
│   ├── dotnet/                  # .NET specific skills
│   ├── nodejs/                  # Node.js specific skills
│   └── shared/                  # Multi-language skills
│
├── commands/                    # Custom commands
│   └── wb-commit.md             # Commit workflow command
│
├── rules/                       # Development rules and guidelines
│   ├── conventional-commits.md  # Commit format specification
│   ├── ai-commit-guidelines.md  # AI behavior for commits
│   └── development-guidelines.md # General development practices
│
└── docs/                        # Documentation
    └── CREATING_ASSETS.md       # Guide for creating new assets
```

## Quick Start

### 1. Link to Your Project

Link or copy this repository to your project's `.cursor/` directory:

```bash
# Option 1: Symlink (recommended)
ln -s /path/to/.cursor-config .cursor

# Option 2: Copy
cp -r /path/to/.cursor-config/* .cursor/
```

### 2. Available Subagents

#### .NET Specific
- **logging-assistant**: Adds structured logging with correlation-ID tracking
- **opentelemetry-instrumentation-assistant**: Adds OpenTelemetry instrumentation

#### Node.js Specific
- **logging-assistant**: Adds structured logging with correlation-ID tracking (Node.js)
- **opentelemetry-instrumentation-assistant**: Adds OpenTelemetry instrumentation (Node.js)

#### Shared (Multi-language)
- **test-assistant**: Analyzes code and suggests/creates tests
- **security-assistant**: Identifies security vulnerabilities
- **refactor-assistant**: Suggests code improvements and refactorings
- **documentation-assistant**: Generates and improves code documentation
- **idempotency-assistant**: Ensures operations are idempotent

### 3. Using Subagents

Subagents are automatically available when this configuration is linked to `.cursor/`. You can invoke them in Cursor chat:

```
@logging-assistant Add logging to this code
@test-assistant Create tests for this function
@security-assistant Review this code for security issues
```

### 4. Available Skills

Skills are reusable knowledge that subagents reference. They're organized by language:

- **dotnet/**: .NET specific patterns (correlation-ID tracking, OpenTelemetry)
- **nodejs/**: Node.js specific patterns (correlation-ID tracking, OpenTelemetry)
- **shared/**: Multi-language patterns (testing, security, performance, code quality, idempotency)

### 5. Custom Commands

- **wb-commit**: Ensures commits follow Conventional Commits pattern with context-based grouping

Use in Cursor chat:
```
/wb-commit
```

## How It Works

### Cursor Detection

Cursor automatically detects:
- Subagents in `.cursor/agents/` (including subdirectories)
- Skills in `.cursor/skills/` (including subdirectories)
- Commands in `.cursor/commands/`
- Rules in `.cursor/rules/`

The recursive search means subdirectories like `dotnet/`, `nodejs/`, and `shared/` are automatically detected.

### Path References

All paths in subagents and skills use the full path from `.cursor/`:

- `.cursor/skills/dotnet/correlation-id-tracking/SKILL.md`
- `.cursor/skills/nodejs/correlation-id-tracking/SKILL.md`
- `.cursor/skills/shared/idempotency/SKILL.md`

## Creating New Assets

See [docs/CREATING_ASSETS.md](docs/CREATING_ASSETS.md) for detailed guides on:
- Creating new subagents
- Creating new skills
- Creating new commands
- Best practices and templates

## Language Support

Currently supported:
- **.NET** (C#): Full support with specialized subagents and skills
- **Node.js** (TypeScript/JavaScript): Full support with specialized subagents and skills
- **Multi-language**: Shared subagents and skills work across languages

## Contributing

When adding new assets:
1. Follow the structure: place language-specific assets in `dotnet/` or `nodejs/`, generic in `shared/`
2. Use proper frontmatter in subagents and skills
3. Reference skills using full paths (`.cursor/skills/...`)
4. Update this README if adding major features

## References

- [Cursor Documentation](https://docs.cursor.com)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [AGENTS.md](AGENTS.md) - Minimal root configuration
