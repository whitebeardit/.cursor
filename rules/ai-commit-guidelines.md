# AI Commit Guidelines - Cursor Rules

> **Reference**: For the complete Conventional Commits specification, see [conventional-commits.md](./conventional-commits.md).

## Main Rule

**ALWAYS** when the user requests commits, PRs, or code changes, you MUST:

1. **Suggest commits in Conventional Commits format**
2. **Suggest PR titles in Conventional Commits format**
3. **Remind the user about the format if they forget**

## When Creating Commits

When creating commits, ALWAYS use the format specified in [conventional-commits.md](./conventional-commits.md):

```
<type>(<scope>): <subject>
```

### Type Prioritization

When determining the commit type, prioritize in this order:

1. **`feat`** - For new features
2. **`fix`** - For bug fixes
3. **`refactor`** - For refactoring without behavior changes
4. **`docs`** - For documentation changes
5. **`test`** - For adding/fixing tests
6. **`chore`** - For maintenance tasks

### Common Scopes in the Project

Use these scopes when appropriate:
- `logging` - Changes in logging, enrichers, formatters
- `http` - Changes in HttpClient, handlers
- `middleware` - Changes in middleware (CorrelationIdMiddleware, etc.)
- `mvc` - Changes related to MVC (RouteExtractor, etc.)
- `webapi` - Changes in Web API
- `config` - Changes in configuration (TraceabilityOptions, etc.)
- `core` - Changes in core (CorrelationContext, etc.)
- `tests` - Changes in tests

### Commit Suggestion Examples

**Scenario 1: New Feature**
- **Change:** Adds support for enriching logs with TraceId
- **Suggested commit:** `feat(logging): add TraceContextEnricher for OpenTelemetry support`

**Scenario 2: Bug Fix**
- **Change:** Fixes race condition in middleware
- **Suggested commit:** `fix(middleware): ensure Activity is available in PreSendRequestHeaders`

**Scenario 3: Multiple Changes**
- **Change:** Adds feature X and fixes bug Y
- **Suggested commits:**
  ```
  feat(scope): add feature X
  fix(scope): fix bug Y
  ```
- **NEVER combine multiple changes in a single commit** unless they are related and make sense together.

## When Creating Pull Requests

### Situations That Require PR

**ALWAYS** create/suggest a PR when:
- The user explicitly requests PR creation
- The user mentions "pull request", "PR", "merge request"
- The user asks to "open PR", "create PR", "make PR"
- The user completes a feature or fix and wants code review
- The user wants semantic-release to analyze the changes

**DO NOT** create/suggest PR for:
- Release commits (automatically generated)
- Local configuration commits
- Temporary debug commits

### PR Creation Process

**ALWAYS** when the user requests PR creation or mentions Pull Request, you MUST:

1. **Suggest a title in Conventional Commits format** (see [conventional-commits.md](./conventional-commits.md))
2. **Validate the title before suggesting** (use validation checklist below)
3. **Suggest a complete PR description** (use template from [conventional-commits.md](./conventional-commits.md))
4. **Remind about the importance of the format for semantic-release**

### PR Title Validation

Before suggesting a PR title, verify:

- [ ] Type is correct (`feat`, `fix`, etc.)
- [ ] Scope is appropriate (or omitted if it doesn't make sense)
- [ ] Title has colon (`:`) after scope/type
- [ ] Subject is in lowercase (except proper nouns)
- [ ] Subject is in imperative mood
- [ ] Subject has less than 72 characters
- [ ] Subject doesn't end with period
- [ ] Title doesn't include PR number
- [ ] Format is correct: `<type>(<scope>): <subject>`

**If any validation fails, fix it before suggesting.**

## Validation Before Suggesting

### For Commits

Before suggesting a commit, verify:

- [ ] Type is correct (`feat`, `fix`, etc.)
- [ ] Scope is appropriate (or omitted if it doesn't make sense)
- [ ] Subject is in lowercase
- [ ] Subject is in imperative mood
- [ ] Subject has less than 72 characters
- [ ] Subject doesn't end with period
- [ ] Format is correct: `<type>(<scope>): <subject>`

### For Pull Requests

Before suggesting a PR title, verify:

- [ ] Type is correct (`feat`, `fix`, etc.)
- [ ] Scope is appropriate (or omitted if it doesn't make sense)
- [ ] Title has colon (`:`) after scope/type
- [ ] Subject is in lowercase (except proper nouns)
- [ ] Subject is in imperative mood
- [ ] Subject has less than 72 characters
- [ ] Subject doesn't end with period
- [ ] Title doesn't include PR number
- [ ] Format is correct: `<type>(<scope>): <subject>`

**If any validation fails, fix it before suggesting.**

## Reminders for the User

If the user creates a commit or PR that doesn't follow the pattern, you MUST:

1. **Inform** that the format is incorrect
2. **Suggest** the correct format
3. **Explain** why it's important (semantic-release won't detect it)
4. **Show** examples of correct format

**Example response for commit:**
```
⚠️ The commit title doesn't follow the Conventional Commits pattern.

Current title: "Add new feature"
Suggested title: "feat(logging): add TraceContextEnricher"

This is important because semantic-release analyzes commits to automatically generate versions. Commits that don't follow the pattern are ignored.
```

**Example response for PR:**
```
⚠️ The PR title doesn't follow the Conventional Commits pattern.

Current title: "Logging: enrich trace context + JSON trace fields"
Suggested title: "feat(logging): enrich trace context + JSON trace fields"

IMPORTANT: Semantic-release analyzes the merge commit title to determine whether to generate a new version. PRs with titles that don't follow the pattern will result in "no relevant changes" and no version will be generated.

Correct format: <type>(<scope>): <subject>
Examples:
- feat(logging): enrich trace context + JSON trace fields
- fix(middleware): ensure Activity is available in debug mode
- feat(mvc): add Attribute Routing support
```

## Special Cases

### Breaking Changes

If the change is incompatible, always include in the commit:
```
feat(api): change method signature

BREAKING CHANGE: Method X now requires parameter Y
```

### Release Commits

Commits generated by semantic-release already follow the pattern:
```
chore(release): 1.2.3 [skip ci]
```
**DO NOT modify** these commits.

### Merge Commits

If the merge commit doesn't follow the pattern, suggest editing the title:
```
# Incorrect title
Merge pull request #29 from branch-name

# Correct title
feat(logging): enrich trace context + JSON trace fields (#29)
```

## Integration with Semantic Release

Remember that:
- `feat:` → MINOR version (1.0.0 → 1.1.0)
- `fix:` → PATCH version (1.0.0 → 1.0.1)
- `BREAKING CHANGE:` → MAJOR version (1.0.0 → 2.0.0)
- Other types (`docs`, `chore`, etc.) → No release (unless configured)

**Commits that don't follow the pattern are ignored by semantic-release.**

## Practical PR Creation Examples

### Scenario 1: User Requests PR
**User:** "Create a PR for these changes"
**Cursor Response:**
```
I'll create a PR with title and description in Conventional Commits format.

**Suggested title:**
feat(logging): add TraceContextEnricher for OpenTelemetry support

**Suggested description:**
[Complete template according to conventional-commits.md]
```

### Scenario 2: User Mentions PR with Incorrect Title
**User:** "Create a PR with title 'Add new feature'"
**Cursor Response:**
```
⚠️ The suggested title doesn't follow the Conventional Commits pattern.

Current title: "Add new feature"
Corrected title: "feat(logging): add new feature"

I'll create the PR with the correct title to ensure semantic-release detects the changes.
```

### Scenario 3: Multiple Changes in One PR
**User:** "Create a PR with these changes: adds feature X and fixes bug Y"
**Cursor Response:**
```
To ensure semantic-release detects correctly, I suggest creating separate PRs:

**PR 1 - Feature:**
Title: feat(scope): add feature X
Description: [feature details]

**PR 2 - Bug Fix:**
Title: fix(scope): fix bug Y
Description: [fix details]

If you prefer a single PR, use the most relevant type (usually `feat` if there's a new feature):
Title: feat(scope): add feature X and fix bug Y
```

### Scenario 4: PR with Breaking Change
**User:** "Create a PR that changes method X signature"
**Cursor Response:**
```
⚠️ This change appears to be a BREAKING CHANGE.

**Suggested title:**
feat(api): change method X signature

**Suggested description:**
[Complete template + BREAKING CHANGE section]

**Important:** Include in the description footer:
BREAKING CHANGE: Method X now requires parameter Y instead of Z

This will make semantic-release generate a MAJOR version.
```
