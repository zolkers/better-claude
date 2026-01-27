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
3. Apply conventions from both sources

## Language Guides

Language-specific guides are in `languages/`. Each guide defines:
- Coding conventions
- Project structure patterns
- Common frameworks and how to use them
- Questions to ask the user for preferences

Currently available:
- `languages/java.md`
