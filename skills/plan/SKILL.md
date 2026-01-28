---
name: plan
description: Use when starting a new task or feature - analyzes the project and creates a step-by-step implementation plan
---

# Plan

## Overview

Full planning pipeline. Chains three phases in order to produce a complete implementation plan saved to `.b-claude/plan.md`.

## Pipeline

Execute these phases in order:

### Phase 1: Think

Load and follow `think.md`.

Deeply understand the user's request. Ask targeted questions from language guides. Clarify ambiguities. Produce a structured understanding.

### Phase 2: Analyze

Load and follow `analyze.md`.

Scan the project structure. Detect languages, frameworks, architecture, and conventions. Build a complete picture of the codebase.

### Phase 3: Planify

Load and follow `planify.md`.

Combine the outputs from think and analyze. Create a concrete, step-by-step implementation plan. Save to `.b-claude/plan.md`.

## After Planning

Once the plan is written to `.b-claude/plan.md` and the user approves it, **ask: "The plan is ready. Do you want to proceed to execution (`/b-claude:do`)?"**

## Notes

- If `.b-claude/preferences.md` exists, load it at the start and skip already-answered questions
- If a matching `languages/<lang>/<lang>.md` exists, consult it throughout all phases
