---
name: init
description: Use when starting ANY conversation - establishes b-claude skills and requires Skill invocation before ANY response
---

# b-claude

<EXTREMELY_IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY_IMPORTANT>

## Skills

Use the `Skill` tool to invoke. Never use Read on skill files.

| Command                | What It Does                                                            |
|------------------------|-------------------------------------------------------------------------|
| `b-claude:do`          | **Main skill.** Plans → executes → reviews → commits. Use for ANY task. |
| `b-claude:code-review` | Review code changes against conventions.                                |
| `b-claude:git`         | Git operations (commit, push, branch).                                  |
| `b-claude:init`        | This skill. Establishes skill requirements.                             |

## The Rule

**Check for skills BEFORE any response or action.**

```
User message
     ↓
Could a skill apply? ──yes──→ Invoke Skill("b-claude:...")
     │                              ↓
     no                       Follow skill exactly
     ↓                              ↓
Respond                       Respond
```

## Red Flags - You Are Rationalizing If You Think:

| Thought                            | Reality                            |
|------------------------------------|------------------------------------|
| "Simple question, no skill needed" | Check anyway.                      |
| "I need context first"             | Skill check comes FIRST.           |
| "Let me explore the code"          | Skills tell you HOW. Check first.  |
| "This doesn't need a skill"        | If one exists, use it.             |
| "I'll just do this quick thing"    | Check BEFORE doing.                |
| "The skill is overkill"            | Simple things get complex. Use it. |

## When to Use Each Skill

| Task                    | Skill                  |
|-------------------------|------------------------|
| "Build feature X"       | `b-claude:do`          |
| "Fix bug Y"             | `b-claude:do`          |
| "Add endpoint Z"        | `b-claude:do`          |
| "Refactor this"         | `b-claude:do`          |
| ANY implementation task | `b-claude:do`          |
| "Review my changes"     | `b-claude:code-review` |
| "Commit this"           | `b-claude:git`         |
| "Push to remote"        | `b-claude:git`         |

## Workflow

For most tasks, just use `b-claude:do`. It handles everything:

1. **Plan** - Understands request, analyzes project, creates steps
2. **Execute** - Implements step by step with checkpoints
3. **Review** - Mandatory code review against conventions
4. **Commit** - Auto-commits if enabled

## Internal Modules

These are NOT commands. Loaded automatically by skills:

| Module              | Purpose                                         |
|---------------------|-------------------------------------------------|
| `internal/language` | Detects language, frameworks, loads conventions |

## User Instructions

User instructions say WHAT, not HOW.

- "Add login feature" → Use `b-claude:do`
- "Fix the bug" → Use `b-claude:do`
- "Commit my changes" → Use `b-claude:git`

Never skip skills just because the user didn't explicitly request them.

<REMINDER>
ALWAYS check if a skill applies. ALWAYS invoke before acting. ALWAYS follow exactly. No exceptions.
</REMINDER>
