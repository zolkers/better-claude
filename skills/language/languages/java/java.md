# Java

## Conventions

Apply SOLID principles. Follow SonarQube conventions strictly.

### Optionnal (depends if the user accepts or not)
- Declare explicit types and avoid `var` unless the user says otherwise; 


### Structure & Complexity

- Max 3 nested statements of any type.
- Max 2 nested `if` statements.
- Max 15 lines per method.
- Max 30 methods per class.
- Max 4 parameters per method -- use an object/DTO beyond that.
- Avoid ternary nesting (`a ? b ? c : d : e`) -- one level max.
- Avoid boolean parameters that switch behavior -- use two methods or an enum instead.

### Imports & Qualifiers

- Always import classes explicitly -- never use fully qualified names inline.
- Exception: fully qualified names are allowed when two classes share the same name.

```java
// Allowed: disambiguating same-name classes
package com.someproject;
public class Screen extends com.example.Screen {
}
```

### Streams & Modern Java

- Prefer streams over `for` loops when they improve readability.
- Use try-with-resources for any `AutoCloseable` (streams, connections, readers).
- Use `Optional<T>` for methods that may not return a value. Never call `.get()` without `.isPresent()` -- use `.orElse()` / `.orElseThrow()`.

### Logging & Exceptions

- Always use a logger -- never `exception.printStackTrace()`.
- Catch specific exceptions -- never generic `Exception` or `Throwable`.
- Never leave catch blocks empty -- at minimum log the exception.
- Don't use exceptions for control flow.

### Null Safety

- Never return `null` for a collection -- return `Collections.emptyList()`, `List.of()`, etc.
- Never pass `null` as a method argument unless the API explicitly expects it.

### Strings

- Use `StringBuilder` for concatenation in loops, not `+`.
- Compare with `.equals()`, never `==`.
- Use `String.format()` or `MessageFormat` for complex string building.

### Immutability & Constants

- Constants: `static final`, named `UPPER_SNAKE_CASE`.
- Prefer immutable objects -- `final` fields, no setters.
- Use `List.of()`, `Map.of()`, or `Collections.unmodifiableList()` for immutable collections.

### Records (Java 16+)

- Use `record` for DTOs, command objects, and any data carrier.
- Prefer `record` over Lombok `@Value` classes if the project uses Java 16 or higher.
- Use **compact constructors** for validation or data normalization (e.g., `public UserRecord { Objects.requireNonNull(email); }`).
- Avoid putting complex business logic inside a record; keep them focused on data.

### Code Clarity

- No magic numbers -- extract to named constants.
- Use early returns for guard clauses -- avoid `else` after `return`.
- Boolean method names read as questions: `isEmpty()`, `hasPermission()`, `canExecute()`.
- Split long regex -- cognitive complexity matters.
- prefer descriptive names over one-letter identifiers.

### Reuse & Patterns

- Before writing new code, check for existing util classes or methods that already do the job.
- Use patterns (builder, factory, etc.) when they fit -- but avoid over-engineering.
- Method parameters and return types should favor interfaces (`List`, `Map`) over implementations (`ArrayList`, `HashMap`).

### Concurrency

- Never expose internal mutable state from a thread-safe class.
- Use `ConcurrentHashMap` over `Hashtable` or synchronized `HashMap`.
- Prefer `java.util.concurrent` utilities over manual `synchronized`/`wait`/`notify`.

### Annotations

Use annotations wherever they add clarity or enforce contracts. Don't create custom annotations unless genuinely reusable.

**Null contracts:**
- `@NonNull` / `@Nullable` on parameters and return types to make intent explicit.

**Override & deprecation:**
- Always use `@Override` when overriding a method -- never skip it.
- Use `@Deprecated` with a `@deprecated` Javadoc tag explaining the replacement.

**Custom annotations:**
- Create custom annotations when a cross-cutting concern repeats (logging, auth checks, rate limiting, auditing).
- Always pair with a processor (Spring AOP, annotation processor, or reflection-based handler).
- Name them as adjectives or intent: `@Auditable`, `@RateLimited`, `@RequiresRole("ADMIN")`.

**Suppression:**
- `@SuppressWarnings` only with a specific reason in a comment. Never blanket-suppress.

**Validation (Bean Validation / Jakarta):**
- Use `@NotNull`, `@NotBlank`, `@Size`, `@Min`, `@Max`, `@Pattern`, `@Email`, `@Valid` on DTOs and request objects instead of manual validation.

```java
public class CreateUserDto {
    @NotBlank
    private String name;

    @Email
    @NotNull
    private String email;

    @Size(min = 8, max = 64)
    private String password;
}
```

**Lombok, Spring, JPA:** See dedicated guides in `languages/java/`.

### Testing

- Every public method should be testable in isolation.

## Questions to Ask

- Do you want code comments?
- Do you want Javadoc on public methods/classes?
- Naming convention: camelCase for methods/variables, PascalCase for classes?
- Package structure: feature-by-layer (default), package-by-layer, hexagonal, or modular monolith?
- Framework-specific conventions (Spring, etc.)?

## Project Structure

Default structure is **feature-by-layer** (hybrid). Each feature gets its own package, with layers inside.

```
com.project/
├── user/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── model/
│   └── dto/
├── auth/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── model/
│   └── dto/
└── common/
    ├── config/
    ├── exception/
    └── util/
```

If the user prefers a **modular monolith** (large-scale projects with independent modules):

```
project/
├── module-user/
│   └── src/main/java/com/project/user/
│       ├── controller/
│       ├── service/
│       ├── repository/
│       └── model/
├── module-auth/
│   └── src/main/java/com/project/auth/
│       └── ...
├── module-common/
│   └── src/main/java/com/project/common/
│       └── ...
```

Each module has its own build config, tests, and dependencies. Use this for projects with multiple teams or 50k+ lines.

If the user prefers **package-by-layer** (small projects, prototypes):

```
com.project/
├── controller/
├── service/
├── repository/
├── model/
├── dto/
├── config/
├── exception/
└── util/
```

Simple and flat. All controllers together, all services together. Works for small projects but doesn't scale -- becomes a mess past 20+ classes per package.

If the user prefers **hexagonal / clean architecture** (domain-driven, strict separation):

```
com.project/
├── domain/
│   ├── model/
│   ├── port/
│   │   ├── in/          # Use cases (interfaces)
│   │   └── out/         # Repository interfaces
│   └── service/
├── application/
│   └── usecase/         # Use case implementations
└── infrastructure/
    ├── adapter/
    │   ├── in/
    │   │   └── web/     # Controllers
    │   └── out/
    │       └── persistence/  # Repository implementations
    └── config/
```

Business logic in `domain/` has zero dependency on frameworks. All external concerns (DB, web, etc.) live in `infrastructure/`. Use this when domain logic is complex and must stay framework-agnostic.

## Testing

- Every utility function should have a unit test.
- Every public API/Service method should be testable in isolation.
- Use Mocks/Spies for network requests or external dependencies.
- Maybe for environments like modding ( like minecraft modding ) Tests wont be doable on minecraft's bootstrap, only test class with no direct depedencies