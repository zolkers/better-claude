---
name: code-review
description: Use when reviewing code changes, after completing a task, or before committing to verify quality and conventions
---

# Code Review

## Overview

Review code changes against project conventions and language-specific rules. Produces a structured review with issues, suggestions, and a verdict.

## Process

### Step 1: Gather Context

1. Check for `.b-claude/preferences.md` -- load user preferences
2. Detect project language(s) and ask questions from the language sections if deemed necessary
3. Load matching `languages/<lang>.md` conventions
4. Identify changed files (via `git diff`, staged changes, or user-specified files)

### Step 2: Review

Analyze each changed file against:

**Conventions compliance:**
- Does the code follow the rules defined in `languages/<lang>.md`?
- Are annotations used properly?
- Is the package structure respected?

**Code quality:**
- SOLID principles
- SonarQube rules (complexity, nesting, method length, etc.)
- No magic numbers, no dead code, no commented-out code
- Proper error handling and logging
- Null safety

**Architecture:**
- Is the code in the right layer/package?
- Are responsibilities properly separated?
- No circular dependencies between packages

**Security:**
- No hardcoded secrets or credentials
- Input validation on entry points
- No SQL injection, XSS, or other OWASP top 10 vulnerabilities

### Step 3: Report

Output a structured review:

```markdown
## Code Review

### Issues
<!-- Blocking problems that must be fixed -->
- [ ] **[SEVERITY]** file:line -- description

### Suggestions
<!-- Non-blocking improvements -->
- file:line -- description

### Verdict
<!-- APPROVE, REQUEST_CHANGES, or NEEDS_DISCUSSION -->
```

Severity levels:
- **CRITICAL** -- security flaw, data loss risk, crash
- **MAJOR** -- convention violation, bad pattern, missing error handling
- **MINOR** -- naming, style, small improvements
