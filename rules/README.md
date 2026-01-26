# Cursor Rules

This directory contains rules and guidelines for Cursor AI to follow when working on this project.

## Rule Files

### `conventional-commits.md`
Complete specification of the Conventional Commits pattern used in the project. Contains:
- Commit format specification
- Allowed commit types
- Examples of valid and invalid commits
- Pull Request format and rules
- Breaking changes format
- Semantic-release integration details
- Validation checklists

### `ai-commit-guidelines.md`
Specific behavior guidelines for Cursor AI when creating commits and PRs:
- When and how to suggest commits/PRs
- Validation process before suggesting
- How to remind users about the format
- Special cases and scenarios
- Practical interaction examples

> **Note**: `ai-commit-guidelines.md` references `conventional-commits.md` for the complete specification. The separation allows the specification to be a reference document while the guidelines focus on AI behavior.

## Why These Rules Exist

This project uses **semantic-release** for automatic versioning and publishing. Semantic-release analyzes commits following the [Conventional Commits](https://www.conventionalcommits.org/) pattern to determine:

- Whether to generate a new version
- What type of version (MAJOR, MINOR, PATCH)
- What to include in the changelog

**Commits that don't follow the pattern are ignored** by semantic-release, resulting in:
- ❌ Versions not automatically generated
- ❌ Outdated changelog
- ❌ Publication not performed

## How to Use

Cursor AI automatically reads these rules and applies them when:
- You request commit creation
- You request Pull Request creation
- You make code changes

If you create a commit or PR that doesn't follow the pattern, Cursor AI will:
1. Alert about the problem
2. Suggest the correct format
3. Explain why it's important

## References

- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [Semantic Release Documentation](https://semantic-release.gitbook.io/)
- [Angular Commit Message Guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit)
