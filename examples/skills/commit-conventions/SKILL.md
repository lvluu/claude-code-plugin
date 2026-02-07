---
name: commit-conventions
description: Auto-invoked when user is making git commits. Enforces conventional commit format and ensures commit messages are descriptive.
version: 0.1.0
---

# Commit Conventions

## When This Skill Applies

- User asks to commit changes
- User asks to create a PR
- Any git commit operation

## Instructions

Enforce conventional commit format:

```
<type>(<scope>): <description>

[optional body]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `ci`

Rules:
- Description must be lowercase, no period at end
- Body wraps at 72 characters
- Reference issues with `#123` format
