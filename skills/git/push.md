# Push

## Overview

Push local commits to the remote. Includes safety checks for protected branches.

## Rules

- Never force-push without explicit user approval.
- Never push to main/master without explicit user approval.
- Always confirm the target branch and remote before pushing.

## Process

### Step 1: Pre-flight

1. Check `.b-claude/preferences.md` for remote URL
2. Run `git status` to confirm there are commits to push
3. Run `git log --oneline @{u}..HEAD` to show what will be pushed (if tracking exists)
4. Identify the current branch

### Step 2: Safety Check

1. If the branch is `main` or `master`:
   - Warn the user explicitly
   - Require confirmation before proceeding
2. If the branch has no upstream tracking:
   - Propose `git push -u origin <branch>`
3. If force-push is requested:
   - Warn the user about potential data loss
   - Require explicit confirmation

### Step 3: Push

1. Confirm the remote and branch with the user
2. Push with `-u` flag if tracking is not set up
3. Report success or failure
