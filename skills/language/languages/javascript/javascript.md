# JavaScript

Inherits all rules from `_shared/conventions.md`. Only JavaScript-specific conventions below.

## Conventions

Follow Clean Code practices and ESLint "standard" or "Airbnb" rules. Focus on immutability and modern ES6+ syntax.

### Variables & Types

- Use `const` by default. Use `let` only if reassignment is necessary. Never use `var`.
- Use strict equality `===` and `!==` instead of `==` and `!=`.
- Use template literals `` `string ${variable}` `` instead of string concatenation.
- Use destructuring for objects and arrays to improve readability.
- Use **Optional Chaining** (`?.`) instead of nested `&&` checks for deeply nested objects.
- Use **Nullish Coalescing** (`??`) instead of `||` when you specifically want to handle `null` or `undefined` (to avoid issues with falsy values like `0` or `""`).

### Naming Conventions

- **Variables & Functions**: `camelCase`.
- **Classes & Constructors**: `PascalCase`.
- **Constants**: `UPPER_SNAKE_CASE`.
- **Booleans**: Prefix with `is`, `has`, `can`, or `should` (e.g., `isVisible`, `hasToken`).
- **Private fields**: Use the `#` prefix for truly private class fields.

### Functions & Functional Programming

- Prefer arrow functions `const suit = () => {}` for callbacks and anonymous functions.
- Prefer immutability: Use `.map()`, `.filter()`, `.reduce()`, and `.find()` instead of `for` loops.
- Use default parameters instead of manual null checks.
- Use the spread operator `...` for shallow copies of arrays and objects.
- Never mutate function arguments.
- Use the **Spread operator** (`{...obj}`, `[...arr]`) to update objects or arrays instead of direct mutation.
- Use `Object.freeze()` for static configuration objects to ensure they cannot be modified at runtime.

### Async Programming

- Always use `async/await` over raw `.then().catch()` Promises for readability.
- Always wrap `await` calls in `try/catch` blocks.
- Use `Promise.all()` for concurrent independent operations to improve performance.
- Never leave a Promise unhandled.

### Error Handling (JS-specific)

- Throw proper `Error` objects: `throw new Error("message")`.

### DOM Manipulation (Vanilla JS)

- Never use `innerHTML` for user-generated content (prevents XSS). Use `textContent` or `innerText`.
- Cache DOM selectors in variables if reused.
- Use Event Delegation (one listener on a parent) for lists or multiple similar elements.
- Use `dataset` (`data-*` attributes) to store data in the HTML.

### Modules

- Use ES Modules (`import`/`export`).
- Explicitly name exports for utility functions. Use default exports only for the main component or class of a file.
- One main feature per file.

### Performance & Security

- Use `debounce` or `throttle` for high-frequency events (scroll, resize, input).
- Avoid global variables to prevent namespace pollution.
- Always sanitize or validate user input before processing.
- Use `JSON.parse()` inside a `try/catch` to avoid crashes on malformed strings.

### Documentation (JS-specific)

- Use **JSDoc** for complex functions to define parameter types and return values.
- Use `@deprecated` to mark functions that should no longer be used.

```javascript
/**
 * Calculates the total price including tax.
 * @param {number} price - The base price.
 * @param {number} taxRate - Tax rate between 0 and 1.
 * @returns {number} The total price.
 */
const calculateTotal = (price, taxRate) => price * (1 + taxRate);
```

## Questions to Ask

- Do you want code comments ?
- Do you want documentation ?
- Is this for the Browser, Node.js, or both (Universal)?
- Do you want to use TypeScript instead of plain JavaScript?
- Which module system are we using (ESM or CommonJS)?
- Which testing framework is preferred (Vitest, Jest, Cypress)?
- Is there a specific linter/formatter config (ESLint + Prettier)?

## Project Structure

Default structure is **feature-based**.

```
project/
├── src/
│   ├── features/
│   │   ├── auth/
│   │   │   ├── auth.api.js       # API calls
│   │   │   ├── auth.service.js   # Business logic
│   │   │   └── auth.utils.js     # Helpers
│   │   └── user/
│   ├── shared/
│   │   ├── components/           # Reusable UI parts
│   │   ├── hooks/                # (If using React/Alpine)
│   │   ├── constants/            # UPPER_SNAKE_CASE constants
│   │   └── utils/                # Global helpers
│   ├── api/                      # Base API / Axios config
│   └── main.js                   # Entry point
├── tests/
├── .eslintrc.json
└── package.json
```

## Testing (JS-specific)

- Use `describe` / `it` blocks for clear test structure.
- Prefer Vitest or Jest for unit testing.

## Gitignore (JavaScript)

Add to the shared base:

```
dist/
build/
coverage/
.env
.env.*
```
