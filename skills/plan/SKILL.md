---
name: plan
description: Use when starting a new task or feature - analyzes the project and creates a step-by-step implementation plan
---

# Plan

<CRITICAL>
This skill has MANDATORY requirements. You do NOT have a choice. You MUST follow every step exactly as written. This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</CRITICAL>

## Overview

Full planning pipeline. Chains three phases in order to produce a complete implementation plan saved to `.b-claude/plan.md`.

## Mandatory Announcements

You MUST announce every action BEFORE taking it. No exceptions.

| Action | Required Announcement |
|--------|----------------------|
| Starting skill | "Using the plan skill to create an implementation plan." |
| Starting Phase 1 | "Phase 1: Think -- Understanding the request." |
| Loading think.md | "Loading think.md." |
| Asking question | "Question: [question text]" |
| Noting answer | "Noted: [summary of answer]." |
| Phase 1 complete | "Phase 1 complete. Understanding established." |
| Starting Phase 2 | "Phase 2: Analyze -- Scanning the project." |
| Loading analyze.md | "Loading analyze.md." |
| Detecting language | "Detecting language... [Language] detected." |
| Detecting framework | "Detecting framework... [Framework] detected." |
| Scanning structure | "Scanning project structure..." |
| Phase 2 complete | "Phase 2 complete. Project analysis done." |
| Starting Phase 3 | "Phase 3: Planify -- Creating the plan." |
| Loading planify.md | "Loading planify.md." |
| Writing plan | "Writing plan to .b-claude/plan.md." |
| Phase 3 complete | "Phase 3 complete. Plan saved." |
| Completion | "Planning complete. Ready for review." |

## Red Flags - You Are Rationalizing If You Think:

| Thought | Reality |
|---------|---------|
| "I already understand the request" | NO. Run Phase 1 anyway. Ask questions. |
| "I know this codebase" | WRONG. Run Phase 2 anyway. Scan everything. |
| "I can skip straight to planning" | NEVER. All three phases are mandatory. |
| "The user gave enough detail" | FALSE. Always ask clarifying questions. |
| "I'll combine the phases" | NO. One phase at a time, fully completed. |
| "This is a simple task" | DOESN'T MATTER. Full pipeline every time. |
| "I'll announce at the end" | WRONG. Announce BEFORE each action. |

## Pipeline

Execute these phases in order. NO SKIPPING. NO COMBINING.

### Phase 1: Think

**Step 1.1: Announce**
```
"Phase 1: Think -- Understanding the request."
"Loading think.md."
```

**Step 1.2: Load and Execute**
Load and follow `think.md` exactly.

**Step 1.3: Ask Questions**
- Ask questions ONE AT A TIME
- Wait for each answer before proceeding
- Announce each answer: "Noted: [summary]."

**Step 1.4: Confirm Understanding**
```
"Phase 1 complete. Understanding established:
- [Key point 1]
- [Key point 2]
- [Key point 3]"
```

**CHECKPOINT: Do NOT proceed to Phase 2 until the user confirms the understanding is correct.**

### Phase 2: Analyze

**Step 2.1: Announce**
```
"Phase 2: Analyze -- Scanning the project."
"Loading analyze.md."
```

**Step 2.2: Load and Execute**
Load and follow `analyze.md` exactly.

**Step 2.3: Detect and Announce**
For each detection:
```
"Detecting language... [Language] detected from [source]."
"Detecting framework... [Framework] detected from [source]."
"Scanning project structure..."
```

**Step 2.4: Summarize Analysis**
```
"Phase 2 complete. Project analysis:
- Language: [language]
- Frameworks: [list]
- Architecture: [pattern]
- Key files: [list]"
```

**CHECKPOINT: Do NOT proceed to Phase 3 until the user confirms the analysis is correct.**

### Phase 3: Planify

**Step 3.1: Announce**
```
"Phase 3: Planify -- Creating the plan."
"Loading planify.md."
```

**Step 3.2: Load and Execute**
Load and follow `planify.md` exactly.

**Step 3.3: Write Plan**
```
"Writing plan to .b-claude/plan.md."
```
Create the plan file with checkboxes for each step.

**Step 3.4: Present Plan**
```
"Phase 3 complete. Plan saved to .b-claude/plan.md."
```
Display the plan summary to the user.

**CHECKPOINT: Do NOT proceed until the user approves the plan.**

## After Planning

Once the plan is written and approved:
```
"Planning complete. The plan is ready."
"Do you want to proceed to execution (`/b-claude:do`)?"
```

## STOP Conditions

STOP and ask the user if:
- The request is ambiguous and Phase 1 questions didn't clarify
- Multiple architectural approaches are possible
- The detected stack doesn't match the user's description
- A step in the plan seems risky or destructive
- You're unsure about any requirement

## Internal Module: Language

This skill automatically loads the `internal/language` module during Phase 2 (Analyze).

When loading:
```
"Loading internal/language module for convention detection."
```

Then follow the language module's Automatic Application process to detect and announce:
- Universal conventions
- Language base conventions
- Runtimes
- Frameworks
- Environments

## Notes

- If `.b-claude/preferences.md` exists, load it at the start
- The language module is loaded automatically during Phase 2
- Each phase MUST be completed fully before starting the next
- Each CHECKPOINT requires user confirmation before proceeding

<REMINDER>
You MUST announce EVERY step. You MUST wait at EVERY checkpoint. No silent actions. No skipped phases. No exceptions.
</REMINDER>
