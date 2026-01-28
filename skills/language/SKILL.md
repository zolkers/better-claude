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
│   ├── runtimes/                  # Execution environments (cross-language)
│   │   └── nodejs.md              # Node.js runtime (JS or TS backend)
│   └── frameworks/                # UI/application frameworks (cross-language)
│       ├── angular.md             # Angular (TS-first)
│       ├── react.md               # React (JS or TS)
│       └── vuejs.md               # Vue.js (JS or TS)
├── java/
│   ├── java.md                    # Base Java conventions
│   ├── frameworks/                # Java-only frameworks
│   │   ├── spring.md              # Spring / JPA conventions
│   │   └── lombok.md              # Lombok conventions
│   └── environments/
│       └── minecraft-modding.md   # Minecraft modding (Fabric/Forge/NeoForge)
├── css/
│   ├── css.md                     # Base CSS conventions, UX/UI standards
│   └── frameworks/
│       ├── tailwind.md            # Tailwind CSS
│       └── bootstrap.md           # Bootstrap
├── php/
│   ├── php.md                     # Base PHP conventions (PSR-12, strict types)
│   └── frameworks/
│       ├── laravel.md             # Laravel
│       └── symfony.md             # Symfony
└── ...
```

Currently available (files md always are in lowercase):

**Shared (cross-language)**
- `_shared/conventions.md` -- Universal rules (loaded first for all languages)
- `_shared/runtimes/` -- Execution environments: Node.js
- `_shared/frameworks/` -- UI/application frameworks: Angular, React, Vue.js

**Languages**
- `css/` -- CSS base conventions, UX/UI standards
- `css/frameworks/` -- Tailwind CSS, Bootstrap
- `html/` -- HTML base conventions
- `java/` -- Java base conventions
- `java/frameworks/` -- Spring, Lombok
- `java/environments/` -- Minecraft modding
- `javascript/` -- JavaScript base conventions
- `php/` -- PHP base conventions (PSR-12, strict typing)
- `php/frameworks/` -- Laravel, Symfony
- `python/` -- Python base conventions
- `python/libs/` -- Turtle graphics
- `typescript/` -- TypeScript base conventions

## Environments

Environments are specialized contexts that combine a language, specific libraries, and domain-specific conventions. They live in `languages/<lang>/environments/`.

Each environment guide has a **Detection** section listing files or dependencies that trigger it automatically. For example, `fabric.mod.json` in the project = Minecraft modding detected.

When an environment is detected, load it on top of the base language and any matching framework guides.

## Shared Conventions

All languages inherit from `languages/_shared/conventions.md`. This file contains universal rules (complexity limits, error handling, null safety, testing, logging, etc.) that apply to every language. Always load it first.

## Detection Priority

When loading conventions:
1. Load `_shared/conventions.md` first (universal rules)
2. Load `<lang>/<lang>.md` (base language conventions)
3. Detect runtimes (e.g., Node.js for JS/TS backend projects)
4. Load matching runtime guides from `_shared/runtimes/`
5. Detect frameworks/libraries in the project (React, Vue, Spring, Tailwind, etc.)
6. Load matching framework guides from `_shared/frameworks/` or `<lang>/frameworks/`
7. Detect environments from `<lang>/environments/`
8. Load matching environment guides (highest priority)
9. Announce what was detected so the user understands which conventions apply
