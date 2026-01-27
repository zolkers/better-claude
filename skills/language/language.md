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
2. Check for `languages/<detected-lang>.md`
3. Check for `frameworks/<detected-framework>.md`
4. Apply conventions from all sources (language + framework)

## Language Guides

Language-specific guides are in `languages/`. Each guide defines:
- Coding conventions
- Project structure patterns
- Questions to ask the user for preferences

Currently available:
- `languages/java.md`

## Framework Guides

Framework-specific guides are in `frameworks/`. Frameworks are separate from languages because they define their own conventions, patterns, and architecture that go beyond the base language.

For example, React is not just JavaScript -- it has its own component patterns, state management, and project structure. Spring Boot is not just Java -- it has its own annotation conventions, layering, and config patterns.

Each framework guide defines:
- Architecture and project structure
- Framework-specific conventions and patterns
- Common libraries and how to use them
- Questions to ask the user for preferences

Currently available:
- *(none yet -- add guides in `frameworks/`)*

## Detection Priority

When both a language and a framework are detected:
1. Load `languages/<lang>.md` first (base conventions)
2. Load `frameworks/<framework>.md` on top (framework conventions override where they conflict)
