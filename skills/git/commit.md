# Commit

## Overview

Stage and commit changes. All commits are made under the user's identity -- Claude is never credited.

## Rules

- **NEVER add Co-Authored-By for Claude. Strictly forbidden.**
- Commits belong entirely to the user.
- Never skip hooks (--no-verify) unless the user explicitly asks.
- Always stage specific files -- avoid `git add -A` or `git add .`.
- Never commit files that may contain secrets (.env, credentials, keys).

## Process

### Step 1: Identity

1. Check `.b-claude/preferences.md` for Git config (username, email)
2. If not found, ask the user for their Git username and email
3. Save to `.b-claude/preferences.md`

### Step 2: Review

1. Run `git status` to see changes
2. Run `git diff` to review what will be committed
3. Group changes by logical purpose

### Step 3: Commit

1. Propose a commit message to the user (concise, focused on "why")
2. Wait for user approval or edits
3. Stage relevant files by name
4. Commit using the user's identity

## Commit Rules

- **Every change that has a reason to exist gets its own commit.**
- Commits must be highly relevant and focused -- one logical change per commit.
- Never bundle unrelated changes into a single commit.
- Commit messages are concise and focused on the "why", not the "what".
- Use conventional commit format if the project already uses it, otherwise keep it simple.
