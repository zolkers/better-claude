# b-claude

A Claude Code plugin for structured project workflows -- analysis, planning, execution, code review, and git management.

## Skills

| Command | Description |
|---------|-------------|
| `/b-claude:plan` | Analyze the project and create a step-by-step implementation plan |
| `/b-claude:do` | Execute an existing plan step by step |
| `/b-claude:language` | Configure coding preferences per language |
| `/b-claude:code-review` | Review code changes against conventions and best practices |
| `/b-claude:git` | Commit and push under the user's identity |

## How It Works

```
/b-claude:plan "add user authentication"
  -> Analyzes project (languages, stack, architecture)
  -> Creates .b-claude/plan.md

/b-claude:do
  -> Reads the plan
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
  -> Commits and pushes under the user's identity
  -> Claude is never credited as co-author
```

## Structure

```
claude-better/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   ├── plan/
│   │   └── plan.md
│   ├── do/
│   │   └── do.md
│   ├── language/
│   │   ├── language.md
│   │   └── languages/
│   │       └── java.md
│   ├── code-review/
│   │   └── code-review.md
│   └── git/
│       └── git.md
├── config/
│   └── preferences.md
├── package.json
└── README.md
```

## Languages Supported

- **Java** -- SOLID, SonarQube rules, annotations, 4 package structure options

More languages can be added in `skills/language/languages/`.

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
