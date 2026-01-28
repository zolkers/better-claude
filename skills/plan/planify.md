# Planify

## Overview

The plan creation phase. Takes the outputs from think (understanding) and analyze (project analysis) and produces a concrete, step-by-step implementation plan.

## Process

### Step 1: Gather Inputs

1. Read the understanding output from the think phase
2. Read the analysis output from the analyze phase
3. Load `.b-claude/preferences.md` if it exists
4. Load relevant language guides for convention reference

### Step 2: Design the Plan

1. Break the user's request into discrete, atomic steps
2. Order steps by dependency -- what must happen first?
3. For each step, identify:
   - Which files will be created or modified
   - Which conventions apply (from language guides)
   - Any risks or edge cases
4. Keep steps small enough to commit individually

### Step 3: Write the Plan

Create the `.b-claude/` folder if it doesn't exist, then write `.b-claude/plan.md`:

```markdown
## Analysis

### Languages
<!-- From analyze phase -->

### Tech Stack
<!-- From analyze phase -->

### Architecture
<!-- From analyze phase -->

### Conventions
<!-- From analyze phase -->

## Goal
<!-- From think phase -- what the user wants -->

## Steps
- [ ] Step 1: description
  - Files: `path/to/file.ext`
  - Notes: any specific instructions
- [ ] Step 2: description
  - Files: `path/to/file.ext`
  - Depends on: Step 1
- [ ] ...

## Preferences
<!-- Link to .b-claude/preferences.md if it exists -->
<!-- Link to applicable language guides -->
```

### Step 4: Present and Transition

1. Show the plan summary to the user
2. Ask for approval or adjustments
3. Once approved, **ask the user: "The plan is ready. Do you want to proceed to execution (`/b-claude:do`)?"**

## Rules

- Steps must be concrete and actionable -- no vague instructions
- Each step should result in a working state (no half-broken intermediate states)
- If the request is too large, suggest breaking it into multiple plans
- Always reference which language/framework conventions apply to each step
- The plan file MUST be written to `.b-claude/plan.md` -- this is what `/b-claude:do` reads
