---
name: do
description: Use when ready to execute a planned task - reads the plan and implements it step by step
---

# Do

## Overview

Execute an existing plan. Checks for `.b-claude/plan.md` first -- if no plan exists, proposes to run `/b-claude:plan` instead. Code review is mandatory at the end.

## Process

- If requirements are unclear or infeasible, request clarification before proceeding.

### Step 1: Check for Plan

1. Look for `.b-claude/plan.md` in the project root
2. If not found:
   - Inform the user: "No plan found."
   - Propose: "Run `/b-claude:plan` first to create one."
   - Stop here.

### Step 2: Initial Preferences

If `.b-claude/preferences.md` does not exist yet, ask the user:
- Do you want emojis in outputs and comments? (yes/no)
- Do you want Claude to commit for you? If yes:
  - Ask for their Git username and email
  - Save to preferences
- Do you want Claude to manage branches for you? If yes:
  - Ask which branch strategy they prefer (see `/b-claude:git` for options)
  - Save to preferences
- Save all answers to `.b-claude/preferences.md`.

If preferences already exist, skip this step.

### Step 3: Load Context

1. Read `.b-claude/plan.md`
2. Read `.b-claude/preferences.md` -- load preferences (emojis, language, etc.)
3. Check for matching `languages/<lang>/<lang>.md` based on detected project language

### Step 4: Execute

1. If branch management is enabled, create the appropriate branch before starting (see `/b-claude:git` branching strategy)
2. Follow the plan step by step
3. For each step:
   - Announce what you're doing
   - Apply language conventions from preferences/language guides
   - Mark the step as done in `.b-claude/plan.md` (check the checkbox)
   - If auto-commit is enabled: commit each meaningful change individually with a relevant message. Every change that has a reason to exist gets its own commit.
4. If a step is unclear, ask the user before proceeding

### Step 5: Code Review (mandatory)

**This step is NOT optional. Always run code review after execution.**

1. Summarize what was done
2. Note any steps that were skipped or need follow-up
3. If user opted for auto-commit in preferences, use `/b-claude:git` for all Git operations.
4. **Run `/b-claude:code-review` on all changes made during execution.**
5. If the code review finds issues:
   - Fix CRITICAL and MAJOR issues immediately
   - Present MINOR issues to the user for decision
   - Re-commit fixes if auto-commit is enabled
