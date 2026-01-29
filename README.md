# b-claude

A Claude Code plugin for disciplined project workflows. One command to plan, execute, review, and commit.

## Commands

| Command                 | Description                                                               |
|-------------------------|---------------------------------------------------------------------------|
| `/b-claude:do`          | **Main command.** Plans → executes → reviews → commits. Use for ANY task. |
| `/b-claude:code-review` | Review code changes against conventions.                                  |
| `/b-claude:git`         | Git operations (commit, push, branch).                                    |
| `/b-claude:init`        | Initialize skill discovery (usually auto-loaded).                         |

## How It Works

```
/b-claude:do "add user authentication"

  Step 0: Preferences
    -> Asks emojis, auto-commit, branch management (first run only)
    -> Saves to .b-claude/preferences.md

  Step 1: Check for Plan
    -> Resumes existing plan if found
    -> Otherwise starts planning phase

  Step 2: Planning Phase (if no plan)
    Phase 2.1: Think    -> Clarifying questions, one at a time
    Phase 2.2: Analyze  -> Detect language, frameworks, scan project
    Phase 2.3: Planify  -> Create .b-claude/plan.md with steps

  Step 3: Execution Phase
    -> Execute steps in batches of 3
    -> Checkpoint after each batch
    -> Auto-commit if enabled

  Step 4: Code Review (mandatory)
    -> Review all changes against conventions
    -> Fix CRITICAL/MAJOR issues immediately

  Step 5: Completion
    -> Summary of work done
    -> Push to remote if requested
```

## Enforcement

All skills follow strict enforcement patterns:

- **Mandatory announcements** before every action
- **Checkpoints** requiring user confirmation
- **Red flags table** preventing rationalization
- **STOP conditions** for ambiguous situations

Claude cannot skip steps, combine phases, or work silently.

## Languages Supported

All languages inherit shared conventions (complexity limits, error handling, null safety, etc.).

| Language   | Frameworks/Libs                   |
|------------|-----------------------------------|
| Java       | Spring, Lombok, Minecraft modding |
| JavaScript | React, Vue.js, Node.js            |
| TypeScript | React, Vue.js, Angular, Node.js   |
| Python     | Turtle graphics                   |
| PHP        | Laravel, Symfony                  |
| CSS        | Tailwind, Bootstrap               |
| HTML       | WCAG accessibility                |

## Project Structure

```
.b-claude/
├── preferences.md    # User preferences (emojis, git config)
└── plan.md           # Current task plan with checkboxes
```

## Installation

```bash
# Add marketplace
claude plugin marketplace add zolkers/better-claude

# Install plugin
claude plugin install b-claude@better-claude

# Update later
claude plugin update b-claude@better-claude
```

## License

MIT
