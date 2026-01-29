# TypeScript

Inherits all rules from `_shared/conventions.md` and `javascript/javascript.md`. Only TypeScript-specific conventions below.

## Detection

This guide applies when `tsconfig.json` is found in the project or `.ts`/`.tsx` files are present.

## Conventions

### Compiler Strictness

- Always enable `"strict": true` in `tsconfig.json`.
- Enable `"noUncheckedIndexedAccess": true` for safer array/object access.
- Enable `"exactOptionalProperties": true` (5.0+) to distinguish `undefined` from missing.
- Never use `// @ts-ignore` -- use `// @ts-expect-error` with a reason comment if suppression is truly needed.
- Never disable strict checks per-file.

### Types

- Prefer `interface` for object shapes that can be extended. Use `type` for unions, intersections, and computed types.
- Never use `any` -- use `unknown` and narrow with type guards.
- Use `Record<K, V>` over `{ [key: string]: V }`.
- Use `as const` for literal types and readonly tuples.
- Use `satisfies` (4.9+) to validate a value matches a type without widening.
- Prefer `readonly` on properties and parameters that shouldn't be mutated.
- Use discriminated unions for state management over optional properties.

```typescript
// Prefer discriminated unions
type Result<T> =
  | { status: "success"; data: T }
  | { status: "error"; error: string };

// Over optional chaos
// BAD: { data?: T; error?: string; status: string }
```

### Functions

- Always type parameters and return values explicitly -- never rely on inference for public APIs.
- Use overloads sparingly -- prefer union types or generics.
- Use `void` for functions that return nothing, `never` for functions that never return.
- Use generic constraints: `<T extends Base>` not unconstrained `<T>`.

### Generics

- Name generics descriptively when not obvious: `TItem`, `TResponse`, not just `T`, `U`.
- Prefer `extends` constraints over runtime checks.
- Avoid deeply nested generics -- extract intermediate types.

### Enums

- Prefer `as const` objects over `enum` for most cases -- they're more tree-shakeable.
- If using `enum`, always use string values, never numeric.

```typescript
// Preferred
const Status = {
  Active: "active",
  Inactive: "inactive",
} as const;
type Status = (typeof Status)[keyof typeof Status];

// Acceptable
enum Direction {
  Up = "UP",
  Down = "DOWN",
}
```

### Null Handling

- Use strict null checks (enabled by `strict: true`).
- Use optional chaining `?.` and nullish coalescing `??`.
- Avoid non-null assertion `!` -- narrow with type guards instead.
- Use `undefined` over `null` for consistency, unless an API requires `null`.

### Utility Types

- Use built-in utility types: `Partial<T>`, `Required<T>`, `Pick<T, K>`, `Omit<T, K>`, `Readonly<T>`.
- Use `Extract` / `Exclude` for union manipulation.
- Use `ReturnType<T>`, `Parameters<T>` for deriving types from functions.
- Create project-specific utility types when patterns repeat.

### Module Organization

- Export types alongside their implementations.
- Use `type` imports: `import type { Foo } from "./foo"` for type-only imports.
- Barrel exports (`index.ts`) only at package boundaries -- avoid deep barrel chains.

### Testing

- Type your test fixtures and mocks.
- Use `expectTypeOf` (vitest) or `tsd` for compile-time type testing.
- Don't use `any` in tests either -- use proper typing or `unknown`.

## Questions to Ask

- TypeScript version target (5.0+, 5.3+, latest)?
- Strict mode: full strict or gradual migration?
- Runtime: Node.js, Deno, Bun, browser?
- Bundler: Vite, esbuild, webpack, tsc only?
- Path aliases configured (`@/`)?

## Gitignore (TypeScript)

Add to the JavaScript gitignore:

```
*.tsbuildinfo
```
