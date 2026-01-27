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

### Option 1: Install from GitHub

In Claude Code, run:

```
/install-plugin https://github.com/zolkers/better-claude
```

### Option 2: Manual install

1. Clone the repository:

```bash
git clone https://github.com/zolkers/better-claude.git
```

2. Add the plugin to your Claude Code settings. Edit `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "b-claude@b-claude-marketplace": true
  }
}
```

3. Register the plugin in `~/.claude/plugins/installed_plugins.json`:

```json
{
  "version": 2,
  "plugins": {
    "b-claude@b-claude-marketplace": [
      {
        "scope": "user",
        "installPath": "/path/to/better-claude",
        "version": "0.1.0"
      }
    ]
  }
}
```

Replace `/path/to/better-claude` with the actual path where you cloned the repo.

4. Restart Claude Code. The skills should now be available as `/b-claude:plan`, `/b-claude:do`, etc.

## License

MIT
