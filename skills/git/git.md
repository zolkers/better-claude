---
name: git
description: Use when committing, pushing, or managing git operations for the user
---

# Git

## Overview

Handle all Git operations on behalf of the user. All commits are made under the user's identity -- Claude is never credited.

## Rules

- **NEVER add Co-Authored-By for Claude. Strictly forbidden.**
- Commits belong entirely to the user.
- Never force-push without explicit user approval.
- Never push to main/master without explicit user approval.
- Never skip hooks (--no-verify) unless the user explicitly asks.
- Always stage specific files -- avoid `git add -A` or `git add .`.
- Never commit files that may contain secrets (.env, credentials, keys).

## Branch Strategy

On first run, ask the user which strategy they prefer:

**Gitflow (default):**
```
main            -- production-ready code
  └── develop   -- integration branch
       ├── feature/<name>   -- new features
       ├── bugfix/<name>    -- bug fixes
       ├── hotfix/<name>    -- urgent production fixes
       └── release/<name>   -- release preparation
```

**Trunk-based:**
```
main                        -- single source of truth
  ├── feature/<name>        -- short-lived feature branches
  └── hotfix/<name>         -- urgent fixes
```

**Simple:**
```
main
  └── dev
       └── <name>           -- any work branch
```

When creating branches:
- Always branch from the correct base (`develop` for gitflow, `main` for trunk-based).
- Use lowercase, hyphen-separated names: `feature/user-authentication`, not `feature/UserAuthentication`.
- Propose the branch name to the user before creating it.

## Commit Rules

- **Every change that has a reason to exist gets its own commit.**
- Commits must be highly relevant and focused -- one logical change per commit.
- Never bundle unrelated changes into a single commit.
- Commit messages are concise and focused on the "why", not the "what".
- Use conventional commit format if the project already uses it, otherwise keep it simple.

## Process

### Step 1: Identity

1. Check `.b-claude/preferences.md` for Git config (username, email, remote)
2. If not found, ask the user:
   - Git username
   - Git email
   - Remote URL (optional -- default to `origin`)
3. Save to `.b-claude/preferences.md`

### Step 2: Commit

1. Run `git status` to see changes
2. Run `git diff` to review what will be committed
3. Propose a commit message to the user (concise, focused on "why")
4. Wait for user approval or edits
5. Stage relevant files by name
6. Commit using the user's identity

### Step 3: Push

1. Only push if the user asks
2. Confirm the target branch and remote
3. Warn if pushing to main/master
4. Push with `-u` flag if tracking is not set up
