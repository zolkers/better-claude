# Shared Conventions

These rules apply to **all languages**. Each language guide inherits these and adds language-specific rules on top. Language-specific overrides take priority where they conflict.
if a language overrides one of these rules, keep the language specifics.

## Structure & Complexity

- Max 3 nested statements of any type.
- Max 2 nested `if` statements.
- Max 15 lines per function/method (language guides may adjust).
- Max 30 functions/methods per file or class.
- Max 4 parameters per function -- use an object, DTO, or dataclass beyond that.
- Avoid ternary nesting -- one level max.
- Avoid boolean parameters that switch behavior -- use two functions or an enum instead.
- A function / method is expected to have at most 7 parameters, avoid more
- Cast one of the expression in equations when necessary
- In for loops, only one break / continue statement is usually allowed
- Switch case should always have a default unless it covers entirely a whole enumeration, if the default is empty always comment why (even if the user doesn't want comments)
- When a method/function is empty always comment why (even if the user doesn't want comments)

## Code Clarity

- No magic numbers -- extract to named constants.
- Use early returns for guard clauses -- avoid `else` after `return`.
- Boolean names read as questions: `isEmpty`, `hasPermission`, `canExecute`.
- Prefer descriptive names to abbreviations -- clarity beats brevity. (for instance: avoid x1 + x2 + x3 + x4 * t * t * t)
- Split complex regex -- cognitive complexity matters.

## Error Handling

- Catch specific exceptions -- never generic catch-all.
- Never leave catch blocks empty -- at minimum log the error.
- Don't use exceptions for control flow.

## Null Safety

- Never return null/None for a collection -- return an empty collection instead.
- Never pass null as a method argument unless the API explicitly expects it.

## Immutability

- Prefer immutable data structures where possible.
- Constants at module/class level in `UPPER_SNAKE_CASE`.
- Favor value objects over mutable state.

## Reuse & Patterns

- Before writing new code, check for existing utilities that already do the job.
- Use patterns (builder, factory, etc.) when they fit -- avoid over-engineering.
- Prefer composition to deep inheritance.
- Apply SOLID principles.

## Testing

- Every public function/method should be testable in isolation.
- Every utility function should have a unit test.
- Use mocks/spies for network requests or external dependencies.
- Test behavior, not implementation details.

## Documentation

- Document the "why" for complex logic, not the "how".
- Use the language's standard documentation format (Javadoc, JSDoc, docstrings, etc.).

## Logging

- Use a proper logging library -- never raw print/console.log/printStackTrace in production.
- Log with levels: error, warn, info, debug.
- Don't log sensitive data (passwords, tokens, PII).

## Gitignore (base)

Every project should ignore at minimum:

```
# IDE
.idea/
*.iml
.vscode/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
desktop.ini

# Plugin
.b-claude/

# Dependencies
node_modules/
vendor/
```

Each language guide adds its own entries on top of this base.
