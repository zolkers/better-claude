---
name: do
description: Use when ready to execute a planned task - reads the plan and implements it step by step
---

# Do

<CRITICAL>
This skill has MANDATORY requirements. You do NOT have a choice. You MUST follow every step exactly as written. This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</CRITICAL>

## Overview

Execute an existing plan. Checks for `.b-claude/plan.md` first -- if no plan exists, proposes to run `/b-claude:plan` instead. Code review is mandatory at the end.

## Mandatory Announcements

You MUST announce every action BEFORE taking it. No exceptions.

| Action | Required Announcement |
|--------|----------------------|
| Starting skill | "Using the do skill to execute the plan." |
| Checking for plan | "Checking for .b-claude/plan.md..." |
| Plan found | "Plan found. Loading..." |
| No plan found | "No plan found. Run `/b-claude:plan` first." |
| Loading preferences | "Loading .b-claude/preferences.md." |
| Asking preference | "Preference question: [question]" |
| Saving preference | "Noted: [preference] = [value]. Saving to preferences." |
| Loading plan | "Loading plan from .b-claude/plan.md." |
| Starting step | "Executing step [N]: [description]" |
| Step progress | "Step [N] in progress: [what you're doing]" |
| Step complete | "Step [N] complete. Marking as done." |
| Committing | "Committing: [commit message]" |
| Batch complete | "Batch complete. [N] steps done. Ready for feedback." |
| Starting review | "Starting mandatory code review." |
| Review complete | "Code review complete. Verdict: [verdict]" |
| Fixing issue | "Fixing [severity] issue: [description]" |
| Execution complete | "Execution complete. All steps done." |

## Red Flags - You Are Rationalizing If You Think:

| Thought | Reality |
|---------|---------|
| "I can work without a plan" | NO. Plan is mandatory. Stop and create one. |
| "I'll skip the preferences check" | WRONG. Always check preferences first. |
| "I know what to do, no need to announce" | NEVER. Announce every single step. |
| "I'll do multiple steps at once" | NO. One step at a time, announced. |
| "Code review is optional" | FALSE. Code review is MANDATORY. |
| "The changes are small, no review needed" | WRONG. ALL changes get reviewed. |
| "I'll commit everything at the end" | NO. Commit each meaningful change. |
| "I'll skip the checkpoint" | NEVER. Every batch needs user feedback. |

## Process

### Step 1: Check for Plan

**Step 1.1: Announce**
```
"Using the do skill to execute the plan."
"Checking for .b-claude/plan.md..."
```

**Step 1.2: Check**
- Look for `.b-claude/plan.md` in the project root
- If found: "Plan found. Loading..."
- If NOT found:
  ```
  "No plan found."
  "Run `/b-claude:plan` first to create one."
  ```
  **STOP HERE. Do not proceed.**

### Step 2: Initial Preferences

**Step 2.1: Check Preferences**
```
"Checking for .b-claude/preferences.md..."
```

**Step 2.2: If No Preferences Exist**
Ask these questions ONE AT A TIME. Wait for each answer.

```
"Preference question: Do you want emojis in outputs and comments? (yes/no)"
```
Wait. Then:
```
"Noted: emojis = [answer]. Saving to preferences."
```

```
"Preference question: Do you want Claude to commit for you? (yes/no)"
```
If yes:
```
"Preference question: What is your Git username?"
```
```
"Preference question: What is your Git email?"
```

```
"Preference question: Do you want Claude to manage branches for you? (yes/no)"
```
If yes:
```
"Preference question: Which branch strategy? (feature/, fix/, chore/)"
```

**Step 2.3: Save**
```
"Saving preferences to .b-claude/preferences.md."
```

### Step 3: Load Context

**Step 3.1: Announce**
```
"Loading context for execution."
```

**Step 3.2: Load Files**
```
"Loading plan from .b-claude/plan.md."
```
Read the plan.

```
"Loading preferences from .b-claude/preferences.md."
```
Read preferences.

**Step 3.3: Load Language Module**
```
"Loading internal/language module for convention detection."
```
Follow the language module's Automatic Application process to detect and load all conventions.

### Step 4: Execute

**Step 4.1: Branch (if enabled)**
If branch management is enabled:
```
"Creating branch: [branch-name]"
```

**Step 4.2: Execute Steps in Batches**

Default batch size: **3 steps**. After each batch, STOP for feedback.

For EACH step:

```
"Executing step [N]: [step description from plan]"
```

While working:
```
"Step [N] in progress: [specific action being taken]"
```

When done:
```
"Step [N] complete. Marking as done in plan."
```
Update the checkbox in `.b-claude/plan.md`.

If auto-commit is enabled:
```
"Committing: [descriptive commit message]"
```

**CHECKPOINT after each batch:**
```
"Batch complete. Steps [X-Y] done."
"Ready for feedback. Continue with next batch?"
```

**STOP and wait for user confirmation before continuing.**

**Step 4.3: Handle Blockers**

If a step is unclear or blocked:
```
"BLOCKED at step [N]: [reason]"
"Need clarification: [specific question]"
```
**STOP and wait for user response.**

### Step 5: Code Review (MANDATORY)

**This step is NOT optional. You MUST run code review.**

**Step 5.1: Announce**
```
"All steps complete. Starting mandatory code review."
"Running /b-claude:code-review on all changes."
```

**Step 5.2: Execute Review**
Run the code-review skill on all changes made during execution.

**Step 5.3: Handle Results**
```
"Code review complete. Verdict: [APPROVE/REQUEST_CHANGES/NEEDS_DISCUSSION]"
```

If issues found:
- CRITICAL issues: "Fixing CRITICAL issue: [description]" -- fix immediately
- MAJOR issues: "Fixing MAJOR issue: [description]" -- fix immediately
- MINOR issues: "MINOR issue found: [description]. Fix now or defer?"

**Step 5.4: Re-commit Fixes**
If auto-commit enabled and fixes were made:
```
"Committing fixes: [description]"
```

### Step 6: Completion

```
"Execution complete."
"Summary:"
"- Steps completed: [N]"
"- Commits made: [N]"
"- Code review: [verdict]"
"- Issues fixed: [N]"
```

## STOP Conditions

STOP immediately and ask the user if:
- A step is unclear or ambiguous
- A step requires destructive action (delete, overwrite, force)
- The code review finds CRITICAL issues
- You encounter an unexpected error
- The step seems to contradict the plan or requirements

## Batch Execution Rules

- Default batch: 3 steps
- ALWAYS stop after each batch for feedback
- NEVER execute more than one batch without user confirmation
- If a step fails, STOP the entire batch

<REMINDER>
You MUST announce EVERY step. You MUST stop at EVERY checkpoint. Code review is MANDATORY. No silent actions. No skipped reviews. No exceptions.
</REMINDER>
