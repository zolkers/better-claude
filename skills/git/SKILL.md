---
name: git
description: Use when committing, pushing, or managing git operations for the user
---

# Git

## Overview

Orchestrator for all Git operations. Dispatches to the appropriate sub-module based on what the user needs. All operations are performed under the user's identity -- Claude is never credited.

## Rules (apply to ALL git operations)

- **NEVER add Co-Authored-By for Claude. Strictly forbidden.**
- Commits belong entirely to the user.
- Never force-push without explicit user approval.
- Never push to main/master without explicit user approval.
- Never skip hooks (--no-verify) unless the user explicitly asks.
- Always stage specific files -- avoid `git add -A` or `git add .`.
- Never commit files that may contain secrets (.env, credentials, keys).

## Sub-Modules

Dispatch to the right sub-module based on the user's intent:

### Commit

Load and follow `commit.md`.

Stage and commit changes. One logical change per commit.

### Push

Load and follow `push.md`.

Push commits to the remote. Safety checks for protected branches.

### Branch

Load and follow `branch.md`.

Create, switch, and manage branches following the configured strategy.

## Gitignore

On project init or first commit, ensure a `.gitignore` exists. If not, build one by:

1. Start with the **base gitignore** from `_shared/conventions.md`
2. Detect project languages
3. Load the **Gitignore** section from each detected language guide (e.g. `java.md`, `javascript.md`, `python.md`)
4. Merge all entries into a single `.gitignore`

If the project already has a `.gitignore`, review it and suggest missing entries based on the detected stack. Never overwrite an existing `.gitignore` without user approval.

## Identity

1. Check `.b-claude/preferences.md` for Git config (username, email, remote)
2. If not found, ask the user and save to `.b-claude/preferences.md`
