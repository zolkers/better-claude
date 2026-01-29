---
name: git
description: Use when committing, pushing, or managing git operations for the user
---

# Git

<CRITICAL>
This skill has MANDATORY requirements. You do NOT have a choice. You MUST follow every step exactly as written. This is not negotiable. This is not optional. You cannot rationalize your way out of this.

**ABSOLUTE RULE: NEVER add Co-Authored-By for Claude. Commits belong ENTIRELY to the user. This is non-negotiable.**
</CRITICAL>

## Overview

Orchestrator for all Git operations. Dispatches to the appropriate submodule based on what the user needs. All operations are performed under the user's identity -- Claude is NEVER credited.

## Mandatory Announcements

You MUST announce every action BEFORE taking it. No exceptions.

| Action             | Required Announcement                            |
|--------------------|--------------------------------------------------|
| Starting skill     | "Using the git skill for [operation]."           |
| Checking identity  | "Checking git identity in preferences..."        |
| Identity found     | "Git identity: [username] <[email]>"             |
| Asking identity    | "Git identity not found. What is your username?" |
| Dispatching        | "Dispatching to [sub-module]..."                 |
| Loading sub-module | "Loading [commit/push/branch].md."               |
| Checking status    | "Checking git status..."                         |
| Staging files      | "Staging: [file list]"                           |
| Committing         | "Committing with message: [message]"             |
| Pushing            | "Pushing to [remote]/[branch]..."                |
| Creating branch    | "Creating branch: [name]"                        |
| Switching branch   | "Switching to branch: [name]"                    |
| Operation complete | "Git operation complete."                        |

## Red Flags - You Are Rationalizing If You Think:

| Thought                               | Reality                                      |
|---------------------------------------|----------------------------------------------|
| "I should credit myself as co-author" | ABSOLUTELY NOT. Never. Forbidden.            |
| "I'll just use git add ."             | NO. Stage specific files only.               |
| "I can push to main directly"         | WRONG. Always ask for confirmation first.    |
| "I'll skip the identity check"        | NEVER. Identity is mandatory.                |
| "Force push is fine here"             | NO. Explicit approval required.              |
| "I'll skip the hooks"                 | WRONG. Never use --no-verify without asking. |
| "The .env file is fine to commit"     | FALSE. Never commit secrets.                 |

## Rules (Apply to ALL Git Operations)

These rules are ABSOLUTE. No exceptions.

1. **NEVER add Co-Authored-By for Claude.** Strictly forbidden.
2. **Commits belong entirely to the user.** No Claude credit ever.
3. **Never force-push** without explicit user approval.
4. **Never push to main/master** without explicit user approval.
5. **Never skip hooks** (--no-verify) unless user explicitly asks.
6. **Always stage specific files** -- avoid `git add -A` or `git add .`.
7. **Never commit files that may contain secrets** (.env, credentials, keys).
8. **Always announce** what you're about to do before doing it.

## Process

### Step 1: Identity Check

**Step 1.1: Announce**
```
"Using the git skill for [commit/push/branch operation]."
"Checking git identity in preferences..."
```

**Step 1.2: Check**
Look for git config in `.b-claude/preferences.md`.

If found:
```
"Git identity: [username] <[email]>"
```

If NOT found, ask ONE AT A TIME:
```
"Git identity not found."
"What is your Git username?"
```
Wait. Then:
```
"What is your Git email?"
```
Wait. Then:
```
"Saving git identity to preferences."
```

### Step 2: Dispatch to Sub-Module

Based on user intent, dispatch to the appropriate module:

**For commits:**
```
"Dispatching to commit sub-module..."
"Loading commit.md."
```
Then follow `commit.md` exactly.

**For pushes:**
```
"Dispatching to push sub-module..."
"Loading push.md."
```
Then follow `push.md` exactly.

**For branches:**
```
"Dispatching to branch sub-module..."
"Loading branch.md."
```
Then follow `branch.md` exactly.

### Step 3: Execute Sub-Module

Follow the loaded sub-module with full announcements.

### Step 4: Gitignore Check

On first commit or project init:

**Step 4.1: Check**
```
"Checking for .gitignore..."
```

**Step 4.2: If Missing**
```
"No .gitignore found. Creating one."
"Loading base gitignore from _shared/conventions.md."
"Detecting project languages..."
"[Languages] detected. Loading language-specific ignores."
"Writing .gitignore."
```

**Step 4.3: If Exists**
```
".gitignore exists. Checking for missing entries..."
```
If entries missing:
```
"Suggesting additions to .gitignore:"
"- [entry1]"
"- [entry2]"
"Add these? (yes/no)"
```

## Sub-Modules

### Commit (commit.md)

One logical change per commit. Message format:
```
type(scope): description

[optional body]
```

Types: feat, fix, docs, style, refactor, test, chore

### Push (push.md)

Safety checks:
- Confirm branch name
- Check for protected branches (main, master, develop)
- Verify remote exists

### Branch (branch.md)

Strategies:
- `feature/` -- new features
- `fix/` -- bug fixes
- `chore/` -- maintenance
- `hotfix/` -- urgent production fixes

## STOP Conditions

STOP and ask the user if:
- About to push to main/master
- About to force-push anything
- About to commit a file that might contain secrets
- Unsure which branch to use
- Pre-commit hook fails (do NOT use --no-verify)
- Merge conflict detected

## Forbidden Actions

These actions are NEVER allowed without explicit user request:

| Action                      | Why Forbidden                         |
|-----------------------------|---------------------------------------|
| `git add -A` or `git add .` | May include secrets or unwanted files |
| `git push --force`          | Destructive, loses history            |
| `git push origin main`      | Protected branch, needs approval      |
| `--no-verify` flag          | Bypasses important checks             |
| Co-Authored-By: Claude      | Claude is never credited              |
| Committing .env files       | Contains secrets                      |

<REMINDER>
NEVER credit Claude. NEVER force-push without approval. NEVER commit secrets. You MUST announce EVERY action. No exceptions.
</REMINDER>
