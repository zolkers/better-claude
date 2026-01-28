# Branch

## Overview

Manage git branches according to the user's configured branching strategy. Create feature branches, switch between branches, close features, and handle branch lifecycle.

## Strategies

On first run (or if not configured), ask the user which strategy they prefer. Save to `.b-claude/preferences.md`.

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

## Feature Lifecycle

### Opening a Feature

1. Ensure the working tree is clean (no uncommitted changes)
2. Pull latest from the base branch:
   - Gitflow: `develop`
   - Trunk-based: `main`
   - Simple: `dev`
3. Create the feature branch: `feature/<name>`
4. Switch to the new branch
5. Announce: "Feature branch `feature/<name>` created from `<base>`"

### Working on a Feature

- All commits happen on the feature branch
- Regularly pull from the base branch to stay up to date (rebase or merge depending on team preference)
- Each commit follows the rules from `commit.md`

### Closing a Feature

1. Ensure all changes are committed
2. **Run `/b-claude:code-review` before closing** -- this is mandatory
3. If code review passes (APPROVE verdict):
   - Merge the feature branch into the base branch:
     - Gitflow: merge into `develop`
     - Trunk-based: merge into `main`
     - Simple: merge into `dev`
   - Use `--no-ff` merge to preserve branch history (unless the user prefers squash)
   - Delete the feature branch after merge
   - Announce: "Feature `<name>` merged into `<base>` and branch deleted"
4. If code review fails (REQUEST_CHANGES):
   - Fix the issues on the feature branch
   - Re-run code review
   - Only merge once approved

### Closing a Hotfix

1. Merge into `main` (all strategies)
2. If Gitflow: also merge into `develop`
3. Tag the release if appropriate
4. Delete the hotfix branch

### Closing a Release (Gitflow only)

1. Merge into `main`
2. Tag with version number
3. Merge back into `develop`
4. Delete the release branch

## Branch Operations

### Creating a Branch

1. Determine the correct base branch from the strategy
2. Propose a branch name following conventions
3. Wait for user approval
4. Create and switch to the new branch

### Switching Branches

1. Check for uncommitted changes
2. If changes exist, propose: stash, commit, or abort
3. Switch to the target branch

### Deleting a Branch

1. Confirm with the user before deleting
2. Never delete `main`, `master`, or `develop` without explicit approval
3. Use `git branch -d` (safe delete) -- only `-D` if the user explicitly asks

## Branch Naming

- Always use lowercase, hyphen-separated names
- Always include the type prefix: `feature/`, `bugfix/`, `hotfix/`, `release/`
- Propose the branch name to the user before creating it
- Examples: `feature/user-authentication`, `bugfix/login-redirect`, `hotfix/security-patch`
