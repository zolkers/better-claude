---
name: do
description: Use for ANY task - automatically plans if needed, then executes step by step with mandatory code review
---

# Do

<CRITICAL>
This skill has MANDATORY requirements. You do NOT have a choice. You MUST follow every step exactly as written. This is not negotiable. This is not optional. You cannot rationalize your way out of this.

This skill handles EVERYTHING: planning, execution, review, and git. One command to rule them all.
</CRITICAL>

## Overview

The main workflow skill. Automatically:
1. Creates a plan if none exists (or resumes existing plan)
2. Executes the plan step by step
3. Runs mandatory code review
4. Handles git operations if enabled

## Mandatory Announcements

You MUST announce every action BEFORE taking it. No exceptions.

| Action          | Required Announcement                                  |
|-----------------|--------------------------------------------------------|
| Starting skill  | "Using the do skill."                                  |
| Checking plan   | "Checking for existing plan..."                        |
| No plan found   | "No plan found. Starting planning phase."              |
| Plan found      | "Plan found. Resuming execution."                      |
| Starting phase  | "Phase [N]: [name]"                                    |
| Loading module  | "Loading [module]."                                    |
| Asking question | "Question: [text]"                                     |
| Noting answer   | "Noted: [summary]."                                    |
| Detecting       | "Detecting [what]... [Result] detected from [source]." |
| Writing plan    | "Writing plan to .b-claude/plan.md."                   |
| Executing step  | "Executing step [N]: [description]"                    |
| Step complete   | "Step [N] complete."                                   |
| Batch complete  | "Batch complete. Ready for feedback."                  |
| Starting review | "Starting mandatory code review."                      |
| Issue found     | "Issue found: [severity] - [description]"              |
| Committing      | "Committing: [message]"                                |
| Complete        | "Task complete."                                       |

## Red Flags - You Are Rationalizing If You Think:

| Thought                            | Reality                                  |
|------------------------------------|------------------------------------------|
| "I can skip planning"              | NO. Planning is mandatory for new tasks. |
| "I already understand the request" | WRONG. Ask clarifying questions anyway.  |
| "I know this codebase"             | FALSE. Analyze it anyway.                |
| "I'll combine steps"               | NO. One step at a time.                  |
| "Code review is optional"          | NEVER. Review is mandatory.              |
| "This is simple"                   | DOESN'T MATTER. Full process every time. |
| "I'll announce at the end"         | WRONG. Announce BEFORE each action.      |
| "I'll skip the checkpoint"         | NO. Every checkpoint is mandatory.       |

## Process

### Step 0: Initialize

**Step 0.1: Announce**
```
"Using the do skill."
"Checking for .b-claude/preferences.md..."
```

**Step 0.2: Preferences**
If no preferences exist, ask ONE AT A TIME:
```
"Preference: Do you want emojis? (yes/no)"
"Preference: Auto-commit enabled? (yes/no)"
"Preference: Branch management? (yes/no)"
```
Save to `.b-claude/preferences.md`.

### Step 1: Check for Plan

```
"Checking for existing plan in .b-claude/plan.md..."
```

- If plan exists with uncompleted steps → **Go to Step 3 (Execute)**
- If plan exists but all steps done → **Ask: "Plan complete. New task?"**
- If no plan exists → **Go to Step 2 (Plan)**

### Step 2: Planning Phase

Only runs if no plan exists.

#### Phase 2.1: Think

```
"Phase 2.1: Think -- Understanding the request."
```

Ask clarifying questions ONE AT A TIME:
- What is the goal?
- What are the constraints?
- What are the acceptance criteria?

```
"Noted: [summary of answer]."
```

After all questions:
```
"Understanding established:
- Goal: [goal]
- Constraints: [constraints]
- Criteria: [criteria]"
```

**CHECKPOINT: "Is this understanding correct?"**

#### Phase 2.2: Analyze

```
"Phase 2.2: Analyze -- Scanning the project."
"Loading internal/language module."
```

Follow the language module to detect and announce:
- Language
- Frameworks
- Runtimes
- Environments

```
"Project analysis:
- Language: [lang]
- Frameworks: [list]
- Architecture: [pattern]
- Key files: [list]"
```

**CHECKPOINT: "Is this analysis correct?"**

#### Phase 2.3: Planify

```
"Phase 2.3: Planify -- Creating the plan."
```

Create a step-by-step plan with checkboxes:

```markdown
# Plan: [task name]

## Context
- Language: [lang]
- Frameworks: [list]

## Steps
- [ ] Step 1: [description]
- [ ] Step 2: [description]
- [ ] Step 3: [description]
...
```

```
"Writing plan to .b-claude/plan.md."
"Plan created with [N] steps."
```

**CHECKPOINT: "Approve this plan to proceed?"**

### Step 3: Execution Phase

```
"Starting execution phase."
"Loading plan from .b-claude/plan.md."
```

#### Step 3.1: Branch (if enabled)

If branch management enabled:
```
"Creating branch: [branch-name]"
```

#### Step 3.2: Execute Steps

Batch size: **3 steps** max. Stop after each batch.

For EACH step:
```
"Executing step [N]: [description]"
```

While working:
```
"Step [N] in progress: [action]"
```

When done:
```
"Step [N] complete. Marking as done."
```
Update checkbox in `.b-claude/plan.md`.

If auto-commit enabled:
```
"Committing: [message]"
```

**CHECKPOINT after each batch:**
```
"Batch complete. Steps [X-Y] done."
"Continue with next batch?"
```

**STOP and wait.**

#### Step 3.3: Handle Blockers

If blocked:
```
"BLOCKED at step [N]: [reason]"
"Need clarification: [question]"
```
**STOP and wait.**

### Step 4: Code Review (MANDATORY)

```
"All steps complete. Starting mandatory code review."
```

Review all changes against:
- Language conventions
- Code quality (SOLID, complexity, etc.)
- Security (no secrets, injection, XSS)

For each issue:
```
"Issue found: [SEVERITY] - [file:line] - [description]"
```

```
"Code review complete. Verdict: [APPROVE/REQUEST_CHANGES]"
```

If issues found:
- CRITICAL/MAJOR: Fix immediately, re-commit
- MINOR: Ask user "Fix now or defer?"

### Step 5: Completion

```
"Task complete."
"Summary:"
"- Steps completed: [N]"
"- Commits made: [N]"
"- Code review: [verdict]"
```

If git push enabled:
```
"Push to remote? (yes/no)"
```

## STOP Conditions

STOP immediately and ask if:
- Request is ambiguous
- Multiple approaches possible
- Destructive action required
- CRITICAL issue found
- Step unclear
- Conflict with requirements

## Internal Modules

Automatically loaded during analysis:
- `internal/language` - Convention detection

<REMINDER>
You MUST announce EVERY step. You MUST wait at EVERY checkpoint. Code review is MANDATORY. No shortcuts. No exceptions.
</REMINDER>
