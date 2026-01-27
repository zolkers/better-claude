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
   - Code comments: yes/no, style?
   - Documentation (javadoc, docstrings, etc.): yes/no, level of detail?
   - Naming conventions
   - Any other language-specific preferences
3. Save to `.b-claude/preferences.md`

## Mode 2: Automatic Application

When writing code and `.b-claude/preferences.md` exists:

1. Read preferences
2. Check for `languages/<lang>/<lang>.md` (base language conventions)
3. Detect frameworks/libraries in the project (e.g. Spring, Lombok, React, etc.)
4. Load matching guides from `languages/<lang>/` (e.g. `spring.md`, `lombok.md`)
5. Apply conventions from all sources

## Language Guides

Each language has its own folder in `languages/`. The folder contains:
- `<lang>.md` -- base language conventions, project structure, questions to ask
- Additional `.md` files for frameworks/libraries specific to that language

```
languages/
└── java/
    ├── java.md       # Base Java conventions
    ├── spring.md     # Spring / JPA conventions
    └── lombok.md     # Lombok conventions
```

Currently available:
- `languages/java/` -- Java, Spring, Lombok
- `languages/html/` -- Html


## Detection Priority

When loading conventions:
1. Load `languages/<lang>/<lang>.md` first (base conventions)
2. Detect which frameworks/libraries are present in the project
3. Load matching guides on top (framework conventions override where they conflict)
