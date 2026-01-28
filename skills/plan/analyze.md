# Analyze

## Overview

The analysis phase of planning. Scan the project structure, detect technologies, and build a complete picture of the codebase. Runs after `b-claude:think` when used as part of `b-claude:plan`.

## Process

### Step 1: Scan Project Structure

1. List the project's directory structure (top 3-4 levels)
2. Identify key directories (src, tests, config, docs, etc.)
3. Count files by extension to understand the language mix
4. Note any monorepo or multi-module structure

### Step 2: Detect Languages & Frameworks

1. Check file extensions for languages (`.java`, `.py`, `.js`, `.ts`, `.vue`, etc.)
2. Read config files for framework detection:
   - `pom.xml` / `build.gradle` -- Java dependencies (Spring, Lombok, etc.)
   - `package.json` -- JS/TS dependencies (React, Vue, Express, etc.)
   - `pyproject.toml` / `requirements.txt` -- Python dependencies
   - `tsconfig.json` -- TypeScript presence
3. Load matching language guides via `b-claude:language` (automatic mode)
4. Check for environment triggers (e.g., `fabric.mod.json` = Minecraft modding)

### Step 3: Identify Architecture

1. Detect the package/folder structure pattern:
   - Feature-by-layer, package-by-layer, hexagonal, modular monolith, etc.
2. Identify entry points (main class, app.ts, index.html, etc.)
3. Note existing conventions:
   - Naming patterns (camelCase, snake_case, etc.)
   - Import style
   - Test structure
4. Check for CI/CD files (`.github/workflows`, `Jenkinsfile`, etc.)

### Step 4: Check Existing Context

1. Read `.b-claude/preferences.md` if it exists
2. Check for existing documentation (README, CONTRIBUTING, etc.)
3. Check git history for commit style and branch strategy
4. Note any `.editorconfig`, linter configs, or formatter configs

### Step 5: Output

Produce a structured analysis:

```markdown
## Analysis

### Languages
<!-- Detected languages with proportions -->

### Tech Stack
<!-- Frameworks, key dependencies, build tools -->

### Architecture
<!-- Detected patterns, module structure, entry points -->

### Conventions
<!-- Existing code style, naming, imports, test patterns -->

### Environment
<!-- Any specialized environments detected (e.g., Minecraft modding) -->
```

This output feeds into the next phase (`b-claude:planify`).
