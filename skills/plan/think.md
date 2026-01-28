# Think

## Overview

The thinking phase of planning. Deeply understand the user's request, ask targeted questions, and consult language guide question sections to gather all necessary context before analysis or implementation.

## Process

### Step 1: Understand the Request

1. Read the user's request carefully
2. Identify ambiguities, implicit assumptions, and missing details
3. Determine the scope: is this a new feature, bug fix, refactor, or something else?

### Step 2: Consult Language Guides

1. Detect the project's language(s) from file extensions and config files
2. Load the matching language guide(s) from `languages/<lang>/<lang>.md`
3. Read the **Questions to Ask** section of each relevant guide
4. Filter questions that are relevant to the current request -- don't ask everything, only what matters

### Step 3: Ask Questions

Ask the user targeted questions (max 5-6 total) covering:

1. **From language guides**: relevant questions from the Questions to Ask sections
2. **From the request**: clarifying questions about scope, behavior, edge cases
3. **From architecture**: questions about where things should live, how they should interact

Rules:
- Never ask questions you can answer by reading the project
- Group related questions together
- Propose sensible defaults for each question so the user can just say "yes"
- If the user has `.b-claude/preferences.md`, check it first -- don't re-ask answered questions

### Step 4: Output

Produce a structured summary of what was learned:

```markdown
## Understanding

### Request
<!-- What the user wants, in your own words -->

### Decisions
<!-- Answers to all questions asked -->

### Constraints
<!-- Any limitations or requirements discovered -->

### Language Context
<!-- Which language guides apply, key conventions to follow -->
```

This output feeds into the next phase (`b-claude:analyze`).
