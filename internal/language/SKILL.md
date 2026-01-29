---
name: language
type: internal
description: Internal module for language detection and convention loading. Auto-loaded by other skills. NOT a user command.
---

# Language (Internal Module)

<CRITICAL>
This is an INTERNAL MODULE, not a user-facing command. It is automatically loaded by other skills (plan, do, code-review) when they need language conventions. You do NOT invoke this directly -- it is loaded as part of other skill workflows.

This module has MANDATORY requirements. You do NOT have a choice. You MUST follow every step exactly as written. This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</CRITICAL>

## Overview

Internal module for language detection and convention loading. Automatically loaded by skills that need coding conventions (plan, do, code-review). NOT a user command.

## Mandatory Announcements

You MUST announce every action BEFORE taking it. No exceptions.

| Action                    | Required Announcement                                             |
|---------------------------|-------------------------------------------------------------------|
| Starting skill            | "Using the language skill to [configure/apply] conventions."      |
| Detecting language        | "Detecting language... [Language] detected from [source]."        |
| Loading base conventions  | "Loading _shared/conventions.md (universal rules)."               |
| Loading language guide    | "Loading [language]/[language].md (base conventions)."            |
| Detecting runtime         | "Detecting runtimes... [Runtime] detected from [source]."         |
| Loading runtime guide     | "Loading _shared/runtimes/[runtime].md."                          |
| Detecting framework       | "Detecting frameworks... [Framework] detected from [source]."     |
| Loading framework guide   | "Loading [path]/[framework].md."                                  |
| Detecting environment     | "Detecting environments... [Environment] detected from [source]." |
| Loading environment guide | "Loading [language]/environments/[env].md."                       |
| Applying conventions      | "Applying conventions: [list what's being applied]."              |
| Saving preferences        | "Saving preferences to .b-claude/preferences.md."                 |
| Completion                | "Convention loading complete. Active: [summary list]."            |

## Red Flags - You Are Rationalizing If You Think:

| Thought                                           | Reality                                   |
|---------------------------------------------------|-------------------------------------------|
| "I'll just apply the conventions silently"        | NO. You MUST announce each step.          |
| "The user doesn't need to know what I'm loading"  | WRONG. Transparency is mandatory.         |
| "I'll skip the announcement for speed"            | NEVER. Announcements are non-negotiable.  |
| "I already know these conventions"                | IRRELEVANT. Load and announce anyway.     |
| "This is a simple project, I don't need all this" | FALSE. Every project gets full detection. |
| "I'll combine multiple announcements"             | NO. One announcement per action.          |
| "The detection is obvious"                        | DOESN'T MATTER. Announce it explicitly.   |

## How Skills Load This Module

Other skills (plan, do, code-review) load this module automatically when they need conventions. They call it like this:

```
"Loading internal/language module for convention detection."
```

Then they follow the Automatic Application process below.

## Automatic Application

When loaded by another skill:

**Step 1: Announce Start**
```
"Using the language skill to apply conventions."
```

**Step 2: Load Universal Conventions**
```
"Loading _shared/conventions.md (universal rules)."
```
Read the file.

**Step 3: Detect and Announce Language**
```
"Detecting language... [Language] detected from [source: file extension / package.json / user request / etc.]."
```

**Step 4: Load and Announce Language Guide**
```
"Loading [language]/[language].md (base conventions)."
```
Read the file.

**Step 5: Detect and Announce Runtimes**
Check for runtime indicators (e.g., Node.js backend, Deno, Bun).
- If found: "Detecting runtimes... [Runtime] detected from [package.json scripts / server files / etc.]."
- Then: "Loading _shared/runtimes/[runtime].md."
- If none: "No specific runtime detected."

**Step 6: Detect and Announce Frameworks**
Check for framework indicators in package.json, composer.json, build.gradle, etc.
For EACH framework detected:
```
"Detecting frameworks... [Framework] detected from [source]."
"Loading [path]/[framework].md."
```
If none: "No frameworks detected."

**Step 7: Detect and Announce Environments**
Check for environment indicators (e.g., fabric.mod.json for Minecraft).
- If found: "Detecting environments... [Environment] detected from [source]."
- Then: "Loading [language]/environments/[env].md."
- If none: "No specialized environments detected."

**Step 8: Summarize Active Conventions**
```
"Convention loading complete. Active conventions:
- Universal rules (_shared/conventions.md)
- [Language] base ([language]/[language].md)
- [Runtime if any] (_shared/runtimes/[runtime].md)
- [Framework 1] ([path]/[framework1].md)
- [Framework 2] ([path]/[framework2].md)
- [Environment if any] ([language]/environments/[env].md)

Applying these conventions to the code."
```

**Step 9: Write Code**
Only NOW do you write the code, following ALL loaded conventions.

## STOP Conditions

STOP and ask the user if:
- Multiple languages are detected and it's unclear which to use
- A framework is detected but you're unsure if it applies to the current task
- The detected environment seems incorrect for the user's request
- You encounter a conflict between conventions

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

<REMINDER>
You MUST announce EVERY step. No silent actions. No skipped announcements. No exceptions.
</REMINDER>
