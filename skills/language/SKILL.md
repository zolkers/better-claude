---
name: language
description: Use when configuring coding preferences for a language or when writing code to apply saved conventions
---

# Language

## Overview

Configure and apply language-specific coding preferences. Works in two modes: interactive configuration and automatic application.

## Mode 1: Interactive Configuration

When the user invokes `/b-claude:language`:

1. Ask which language to configure (or detect from project)
2. Ask preference questions:
   - Ask questions from the language sections if deemed necessary
   - Naming conventions
   - Any other language-specific preferences
3. Save to `.b-claude/preferences.md`

## Mode 2: Automatic Application

When writing code and `.b-claude/preferences.md` exists:

1. Read preferences
2. Check for `languages/<lang>/<lang>.md` (base language conventions)
3. Detect frameworks/libraries in the project (e.g. Spring, Lombok, React, etc.)
4. Load matching guides from `languages/<lang>/` (e.g. `spring.md`, `lombok.md`)
5. Detect environments (see below)
6. Apply conventions from all sources

## Language Guides

Each language has its own folder in `languages/`. The folder contains:
- `<lang>.md` -- base language conventions, project structure, questions to ask
- Additional `.md` files for frameworks/libraries specific to that language
- `environments/` -- specialized environments that go beyond a single framework

```
languages/
├── _shared/
│   ├── conventions.md             # Universal rules (all languages inherit)
│   └── frameworks/                # Frameworks usable across multiple languages
│       ├── react.md               # React (JS or TS)
│       ├── nodejs.md              # Node.js (JS or TS)
│       └── vuejs.md               # Vue.js (JS or TS)
├── java/
│   ├── java.md                    # Base Java conventions
│   ├── frameworks/                # Java-only frameworks
│   │   ├── spring.md              # Spring / JPA conventions
│   │   └── lombok.md              # Lombok conventions
│   └── environments/
│       └── minecraft-modding.md   # Minecraft modding (Fabric/Forge/NeoForge)
└── ...
```

Currently available (files md always are in lowercase):
- `languages/_shared/` -- Universal conventions (loaded first for all languages)
- `languages/_shared/frameworks/` -- Cross-language frameworks: React, Node.js, Vue.js
- `languages/java/` -- Java
- `languages/java/frameworks/` -- Java-only: Spring, Lombok
- `languages/java/environments/` -- Minecraft modding
- `languages/python/` -- Python, Turtle graphics
- `languages/javascript/` -- JavaScript
- `languages/typescript/` -- TypeScript
- `languages/html/` -- HTML
- `languages/css/` -- CSS base conventions, UX/UI standards
- `languages/css/frameworks/` -- CSS-only: Tailwind CSS, Bootstrap

## Environments

Environments are specialized contexts that combine a language, specific libraries, and domain-specific conventions. They live in `languages/<lang>/environments/`.

Each environment guide has a **Detection** section listing files or dependencies that trigger it automatically. For example, `fabric.mod.json` in the project = Minecraft modding detected.

When an environment is detected, load it on top of the base language and any matching framework guides.

## Shared Conventions

All languages inherit from `languages/_shared/conventions.md`. This file contains universal rules (complexity limits, error handling, null safety, testing, logging, etc.) that apply to every language. Always load it first.

## Detection Priority

When loading conventions:
1. Load `languages/_shared/conventions.md` first (universal rules)
2. Load `languages/<lang>/<lang>.md` (base language conventions)
3. Detect which frameworks/libraries are present in the project
4. Load matching framework guides (framework conventions override where they conflict)
5. Detect environments from `languages/<lang>/environments/`
6. Load matching environment guides on top (environment conventions have highest priority)
7. Please announce what you detected so the user isn't confused
