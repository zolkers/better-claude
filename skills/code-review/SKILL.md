---
name: code-review
description: Use when reviewing code changes, after completing a task, or before committing to verify quality and conventions
---

# Code Review

<CRITICAL>
This skill has MANDATORY requirements. You do NOT have a choice. You MUST follow every step exactly as written. This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</CRITICAL>

## Overview

Review code changes against project conventions and language-specific rules. Produces a structured review with issues, suggestions, and a verdict.

## Mandatory Announcements

You MUST announce every action BEFORE taking it. No exceptions.

| Action | Required Announcement |
|--------|----------------------|
| Starting skill | "Using the code-review skill to review changes." |
| Loading preferences | "Loading .b-claude/preferences.md." |
| Detecting language | "Detecting language... [Language] detected." |
| Loading conventions | "Loading [language]/[language].md conventions." |
| Identifying changes | "Identifying changed files..." |
| Files found | "Found [N] changed files: [list]" |
| Reviewing file | "Reviewing [filename]..." |
| Issue found | "Issue found: [severity] - [description]" |
| File done | "Review of [filename] complete." |
| Compiling report | "Compiling review report..." |
| Verdict | "Verdict: [APPROVE/REQUEST_CHANGES/NEEDS_DISCUSSION]" |

## Red Flags - You Are Rationalizing If You Think:

| Thought | Reality |
|---------|---------|
| "The changes look fine at a glance" | NO. Review every file systematically. |
| "I wrote this code, I don't need to review it" | WRONG. Self-review is mandatory. |
| "These are small changes" | DOESN'T MATTER. All changes get full review. |
| "I'll just check for obvious errors" | NEVER. Check ALL criteria. |
| "Security review is overkill for this" | FALSE. Security is ALWAYS checked. |
| "I can skip the conventions check" | NO. Conventions compliance is mandatory. |
| "I'll combine the review steps" | WRONG. One file at a time, announced. |

## Process

### Step 1: Gather Context

**Step 1.1: Announce**
```
"Using the code-review skill to review changes."
```

**Step 1.2: Load Preferences**
```
"Checking for .b-claude/preferences.md..."
```
If exists: "Loading preferences."
If not: "No preferences found. Using defaults."

**Step 1.3: Load Language Module**
```
"Loading internal/language module for convention detection."
```
Follow the language module's Automatic Application process to detect and load all conventions.

**Step 1.4: Identify Changed Files**
```
"Identifying changed files..."
```
Use `git diff --name-only` or user-specified files.
```
"Found [N] changed files:"
"- [file1]"
"- [file2]"
"- ..."
```

### Step 2: Review Each File

For EACH changed file:

**Step 2.1: Announce**
```
"Reviewing [filename]..."
```

**Step 2.2: Check Conventions Compliance**
- Does the code follow rules in `languages/<lang>.md`?
- Are annotations/decorators used properly?
- Is the package/module structure respected?
- Are naming conventions followed?

**Step 2.3: Check Code Quality**
- SOLID principles applied?
- Complexity acceptable? (nesting, method length)
- No magic numbers?
- No dead code or commented-out code?
- Proper error handling?
- Proper logging (no raw print/console)?
- Null safety respected?

**Step 2.4: Check Architecture**
- Code in the right layer/package?
- Responsibilities properly separated?
- No circular dependencies?

**Step 2.5: Check Security**
- No hardcoded secrets or credentials?
- Input validation on entry points?
- No SQL injection risk?
- No XSS risk?
- No other OWASP top 10 vulnerabilities?

**Step 2.6: Report Issues**
For each issue found:
```
"Issue found: [SEVERITY] - [file:line] - [description]"
```

**Step 2.7: Complete File**
```
"Review of [filename] complete."
```

### Step 3: Compile Report

**Step 3.1: Announce**
```
"Compiling review report..."
```

**Step 3.2: Generate Report**

Output this exact format:

```markdown
## Code Review

### Files Reviewed
- [file1]
- [file2]

### Issues
<!-- Blocking problems that must be fixed -->
- [ ] **[CRITICAL]** file:line -- description
- [ ] **[MAJOR]** file:line -- description
- [ ] **[MINOR]** file:line -- description

### Suggestions
<!-- Non-blocking improvements -->
- file:line -- description

### Verdict
[APPROVE / REQUEST_CHANGES / NEEDS_DISCUSSION]

### Summary
- Files reviewed: [N]
- Critical issues: [N]
- Major issues: [N]
- Minor issues: [N]
- Suggestions: [N]
```

**Step 3.3: Announce Verdict**
```
"Verdict: [APPROVE/REQUEST_CHANGES/NEEDS_DISCUSSION]"
```

## Severity Definitions

| Severity | Definition | Action |
|----------|------------|--------|
| **CRITICAL** | Security flaw, data loss risk, crash, production blocker | MUST fix before merge |
| **MAJOR** | Convention violation, bad pattern, missing error handling, bug | SHOULD fix before merge |
| **MINOR** | Naming, style, small improvements | CAN defer |

## Verdict Rules

- **APPROVE**: Zero CRITICAL, zero MAJOR issues
- **REQUEST_CHANGES**: Any CRITICAL or MAJOR issue exists
- **NEEDS_DISCUSSION**: Architectural concerns or trade-off decisions needed

## STOP Conditions

STOP and ask the user if:
- A file is too large to review effectively (>500 lines changed)
- You're unsure if something is a convention violation
- You find a potential security issue but need more context
- The changes seem to conflict with the project architecture

<REMINDER>
You MUST review EVERY file. You MUST check ALL criteria. You MUST announce each step. No shortcuts. No assumptions. No exceptions.
</REMINDER>
