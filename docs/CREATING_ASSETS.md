# Creating Assets for Cursor Configuration

This guide explains how to create new subagents, skills, and commands for the Cursor configuration repository.

## Table of Contents

- [Creating Subagents](#creating-subagents)
- [Creating Skills](#creating-skills)
- [Creating Commands](#creating-commands)
- [Best Practices](#best-practices)

## Creating Subagents

Subagents are specialized AI assistants that perform specific tasks. They should be placed in the appropriate directory based on language specificity.

### Directory Structure

- **Language-specific**: `agents/dotnet/` or `agents/nodejs/`
- **Multi-language**: `agents/shared/`

### Template

```markdown
---
name: your-assistant-name
model: inherit
description: Brief description of what this subagent does. Should be clear and specific.
---

# Your Assistant Name

You are a specialized assistant that [describes the main purpose].

## Required Skill Dependency

**IMPORTANT**: This subagent MUST use the `skill-name` skill located at `.cursor/skills/[language]/skill-name/SKILL.md`

Before performing any tasks:
1. Read the skill file: `.cursor/skills/[language]/skill-name/SKILL.md`
2. Understand all patterns and best practices from the skill
3. Apply those patterns when working

## Your Mission

When invoked, you will:

1. **Step 1**: Description
2. **Step 2**: Description
3. **Step 3**: Description

## Analysis Process

### Step 1: [Step Name]

[Detailed instructions]

### Step 2: [Step Name]

[Detailed instructions]

## Patterns

### Pattern 1: [Pattern Name]

```[language]
// Example code
```

## Critical Rules

### ✅ DO:
- Rule 1
- Rule 2

### ❌ DON'T:
- Anti-pattern 1
- Anti-pattern 2

## Output Format

After completing the task, provide:

1. **Summary**: What was done
2. **Modified Files**: List of files updated
3. **Verification**: Confirmation that best practices were followed
```

### Example: Language-Specific Subagent

For `.NET` logging assistant:
- File: `agents/dotnet/logging-assistant.md`
- References: `.cursor/skills/dotnet/correlation-id-tracking/SKILL.md`

For `Node.js` logging assistant:
- File: `agents/nodejs/logging-assistant.md`
- References: `.cursor/skills/nodejs/correlation-id-tracking/SKILL.md`

### Example: Shared Subagent

For multi-language test assistant:
- File: `agents/shared/test-assistant.md`
- References: `.cursor/skills/shared/testing/SKILL.md`

## Creating Skills

Skills are reusable knowledge repositories that subagents reference. They contain patterns, best practices, and implementation details.

### Directory Structure

- **Language-specific**: `skills/dotnet/` or `skills/nodejs/`
- **Multi-language**: `skills/shared/`

Each skill should be in its own directory with a `SKILL.md` file.

### Template

```markdown
---
name: skill-name
description: Brief description of what this skill provides. Use when [use cases].
---

# Skill Name

This skill helps you [main purpose] in [language/context].

## When to Use

**✅ DO Use:**
- Use case 1
- Use case 2

**❌ DON'T Use:**
- Anti-pattern 1
- Anti-pattern 2

## Core Concepts

[Explain fundamental concepts]

## Patterns

### Pattern 1: [Pattern Name]

```[language]
// Example implementation
```

### Pattern 2: [Pattern Name]

```[language]
// Example implementation
```

## Best Practices

1. **Practice 1**: Explanation
2. **Practice 2**: Explanation

## Common Issues

**Issue 1:**
- Problem description
- Solution

**Issue 2:**
- Problem description
- Solution

## Key Principles

1. Principle 1
2. Principle 2
```

### Example Structure

```
skills/
├── dotnet/
│   └── correlation-id-tracking/
│       └── SKILL.md
├── nodejs/
│   └── correlation-id-tracking/
│       └── SKILL.md
└── shared/
    └── idempotency/
        └── SKILL.md
```

## Creating Commands

Commands are custom workflows accessible via `/command-name` in Cursor chat.

### Directory Structure

Commands go in `commands/` directory (no subdirectories needed for now).

### Template

```markdown
# command-name

## Objective

Clear description of what this command does.

---

## Rule (to paste in Cursor / Rules)

**Suggested name:** `rule-name`

### 1) [Step 1]

[Instructions]

### 2) [Step 2]

[Instructions]

## Expected Behavior

When invoked, the agent should:

1. Behavior 1
2. Behavior 2

## Examples

### Example 1: [Scenario]

**Input:**
```
/command-name
```

**Expected Output:**
[Description of expected behavior]
```

### Example

See `commands/wb-commit.md` for a complete example of a command implementation.

## Best Practices

### Naming Conventions

- **Subagents**: Use kebab-case with descriptive name (e.g., `logging-assistant`, `test-assistant`)
- **Skills**: Use kebab-case (e.g., `correlation-id-tracking`, `code-quality`)
- **Commands**: Use kebab-case with prefix if needed (e.g., `wb-commit`)

### File Organization

1. **Language-specific assets** → Place in `dotnet/` or `nodejs/` subdirectories
2. **Multi-language assets** → Place in `shared/` subdirectory
3. **Always use full paths** when referencing skills: `.cursor/skills/[language]/skill-name/SKILL.md`

### Frontmatter

Always include frontmatter in subagents and skills:

**Subagents:**
```yaml
---
name: assistant-name
model: inherit
description: Clear, specific description
---
```

**Skills:**
```yaml
---
name: skill-name
description: Clear description with use cases
---
```

### Path References

**Always use full paths from `.cursor/`:**

✅ Correct:
- `.cursor/skills/dotnet/correlation-id-tracking/SKILL.md`
- `.cursor/skills/shared/idempotency/SKILL.md`

❌ Incorrect:
- `skills/correlation-id-tracking/SKILL.md`
- `../skills/idempotency/SKILL.md`

### Skill Dependencies

When a subagent depends on a skill:

1. **Explicitly state the dependency** in the subagent
2. **Provide the full path** to the skill
3. **Instruct the subagent** to read the skill before performing tasks
4. **Reference specific patterns** from the skill

### Code Examples

- Use appropriate language tags in code blocks
- Provide complete, working examples
- Include error handling where relevant
- Show both simple and complex use cases

### Documentation

- **Clear mission**: Each subagent should have a clear mission statement
- **Step-by-step process**: Break down complex tasks into steps
- **DO/DON'T lists**: Make rules explicit
- **Output format**: Specify what the subagent should provide

## Testing Your Assets

Before committing:

1. **Verify paths**: All skill references use correct full paths
2. **Check frontmatter**: YAML frontmatter is valid
3. **Test in Cursor**: Link to a test project and verify detection
4. **Review structure**: Follows directory organization rules

## Checklist

When creating a new asset:

- [ ] Placed in correct directory (dotnet/nodejs/shared)
- [ ] Frontmatter is complete and valid
- [ ] Paths use full `.cursor/` paths
- [ ] Code examples are complete and correct
- [ ] DO/DON'T rules are explicit
- [ ] Documentation is clear
- [ ] Tested in Cursor

## Examples

See existing assets for reference:

- **Subagent (.NET)**: `agents/dotnet/logging-assistant.md`
- **Subagent (Shared)**: `agents/shared/test-assistant.md` (when created)
- **Skill (.NET)**: `skills/dotnet/correlation-id-tracking/SKILL.md`
- **Skill (Shared)**: `skills/shared/idempotency/SKILL.md` (when created)
- **Command**: `commands/wb-commit.md`
