---
name: plan
description: Use when starting a new task or feature - analyzes the project and creates a step-by-step implementation plan
---

# Plan

## Overview

Analyze the current project and create an implementation plan for the user's request. Saves results to `.b-claude/plan.md`.

## Process

### Phase 1: Project Analysis

1. Scan the project structure (directories, files)
2. Detect languages used (by extensions, config files) and ask questions from the language sections if deemed necessary
3. Read config files: `pom.xml`, `build.gradle`, `package.json`, `Dockerfile`, etc.
4. Identify architecture, frameworks, dependencies
5. Check for `.b-claude/preferences.md` -- if it exists, load language preferences

### Phase 2: Planning

1. Read the user's request (passed as argument or asked interactively)
2. Ask clarifying questions if the request is ambiguous (max 3-4 targeted questions)
3. Build a step-by-step plan with:
   - What needs to be done
   - Which files are involved
   - Dependencies between steps

### Phase 3: Save

Write `.b-claude/plan.md` with:

```markdown
## Analysis

### Languages
<!-- Detected languages and proportions -->
ensure questions from the question section are asked if necessary

### Tech Stack
<!-- Frameworks, key dependencies -->

### Architecture
<!-- Detected patterns, module structure -->

### Conventions
<!-- Existing conventions in the codebase -->

## Goal
<!-- What the user wants to achieve -->

## Steps
<!-- Numbered implementation checklist -->
- [ ] Step 1
- [ ] Step 2

## Preferences
<!-- Link to .b-claude/preferences.md if it exists -->
<!-- Link to applicable languages/<lang>.md -->
```

If a matching `languages/<lang>.md` exists for the detected language, consult it and apply its conventions to the plan.
