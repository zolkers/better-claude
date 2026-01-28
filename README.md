# b-claude

A Claude Code plugin for composable project workflows -- planning, execution, code review, language conventions, and git management.

## Skills

### Orchestrators (chain sub-skills)

| Command | Description |
|---------|-------------|
| `/b-claude:plan` | Full planning pipeline: think + analyze + planify |
| `/b-claude:git` | Git operations dispatcher: commit, push, branch |

### Standalone Skills

| Command | Description |
|---------|-------------|
| `/b-claude:do` | Execute an existing plan step by step |
| `/b-claude:language` | Configure coding preferences per language |
| `/b-claude:code-review` | Review code changes against conventions and best practices |

## How It Works

```
/b-claude:plan "add user authentication"
  Phase 1: think    -> Ask questions, consult language guides
  Phase 2: analyze  -> Scan project structure, detect stack
  Phase 3: planify  -> Create .b-claude/plan.md

/b-claude:do
  -> Reads the plan or creates if no plan exist
  -> Asks initial preferences (emojis, git, branch strategy)
  -> Executes step by step
  -> Commits via /b-claude:git if enabled

/b-claude:language
  -> Interactive setup for language-specific conventions
  -> Saves to .b-claude/preferences.md

/b-claude:code-review
  -> Reviews changes against language conventions
  -> Reports issues (CRITICAL / MAJOR / MINOR) + verdict

/b-claude:git
  -> Dispatches to commit, push, or branch
  -> Claude is never credited as co-author
```

## Languages Supported

All languages inherit shared conventions from `_shared/conventions.md` (complexity limits, error handling, null safety, testing, logging, etc.).

- **Java** -- SOLID, SonarQube, annotations, Records, 4 package structure options
  - Spring, Lombok, Minecraft modding environment
- **JavaScript** -- ESLint, ES6+, async, DOM, modules
- **TypeScript** -- Strict typing, generics, utility types (inherits JS)
- **Python** -- PEP 8, Ruff, type hints, dataclasses, scripts
  - Turtle graphics
- **HTML** -- W3C, WCAG accessibility, semantics, SEO
- **Cross-language frameworks** -- React, Node.js, Vue.js (JS or TS)
- **CSS** -- BEM (Block__Element--Modifier) UX/UI responsibility
  - Tailwind, Boostrap

## Preferences

On first run, `/b-claude:do` asks the user for:
- Emojis in outputs (yes/no)
- Auto-commit (yes/no) + git identity
- Git branch strategy (gitflow, trunk-based, simple)

Saved to `.b-claude/preferences.md` in the project root.

## Installation

Add the marketplace:

```bash
claude plugin marketplace add zolkers/better-claude
```

Install the plugin:

```bash
claude plugin install b-claude@better-claude
```

Update later:

```bash
claude plugin update b-claude@better-claude
```

Restart Claude Code. The skills will be available as `/b-claude:plan`, `/b-claude:do`, etc.

## License

MIT
