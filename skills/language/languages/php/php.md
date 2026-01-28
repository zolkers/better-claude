# PHP

## Conventions

Apply PSR standards (PSR-1, PSR-4, PSR-12). Use strict typing. Follow modern PHP practices (8.0+). Leverage Composer for dependency management.

### Strict Typing

- Always declare `declare(strict_types=1);` at the top of every PHP file.
- Always type parameters, return types, and properties.
- Use union types (`string|int`), nullable types (`?string`), and `mixed` when appropriate.
- Use `never` return type for functions that always throw or exit.

```php
<?php

declare(strict_types=1);

final class UserService
{
    public function __construct(
        private readonly UserRepository $repository,
    ) {}

    public function findById(int $id): ?User
    {
        return $this->repository->find($id);
    }
}
```

### Naming Conventions

- **Classes**: `PascalCase` (e.g., `UserController`, `OrderService`).
- **Methods & Functions**: `camelCase` (e.g., `getUserById`, `calculateTotal`).
- **Variables**: `camelCase` (e.g., `$userName`, `$orderItems`).
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `MAX_RETRY_COUNT`).
- **Properties**: `camelCase`, prefer `private` or `protected` with getters/setters or constructor promotion.
- **Interfaces**: `PascalCase`, suffix with `Interface` (e.g., `UserRepositoryInterface`).
- **Traits**: `PascalCase`, suffix with `Trait` (e.g., `TimestampableTrait`).
- **Enums**: `PascalCase` for enum name and cases (e.g., `Status::Pending`).

### Classes & OOP

- Use `final` by default -- only remove when inheritance is explicitly needed.
- Use `readonly` properties (PHP 8.1+) for immutable data.
- Use constructor property promotion to reduce boilerplate.
- Prefer composition over inheritance.
- One class per file, file name matches class name.
- Use enums instead of class constants for fixed sets of values.

```php
final readonly class CreateUserDto
{
    public function __construct(
        public string $email,
        public string $name,
        public ?string $phone = null,
    ) {}
}

enum OrderStatus: string
{
    case Pending = 'pending';
    case Confirmed = 'confirmed';
    case Shipped = 'shipped';
    case Delivered = 'delivered';
}
```

### Functions & Methods

- Max 15 lines per method (excluding blank lines and braces).
- Max 4 parameters -- use a DTO or value object beyond that.
- Use named arguments for optional parameters and better readability.
- Use arrow functions for simple callbacks: `fn($x) => $x * 2`.
- Return early -- avoid deep nesting with guard clauses.

```php
public function processOrder(Order $order): void
{
    if ($order->isPaid()) {
        return;
    }

    if (!$order->hasStock()) {
        throw new OutOfStockException($order->getId());
    }

    $this->paymentService->charge($order);
    $this->notificationService->sendConfirmation($order);
}
```

### Error Handling

- Use exceptions for exceptional cases, not control flow.
- Create custom exception classes per domain: `UserNotFoundException`, `InvalidOrderException`.
- Catch specific exceptions -- never empty catch blocks.
- Use `finally` for cleanup code.
- Log exceptions with context (user ID, request ID, etc.).

```php
final class UserNotFoundException extends DomainException
{
    public static function withId(int $id): self
    {
        return new self(sprintf('User with ID %d not found', $id));
    }
}
```

### Arrays & Collections

- Use short array syntax `[]` -- never `array()`.
- Use array destructuring: `[$first, $second] = $array`.
- Use spread operator for merging: `[...$array1, ...$array2]`.
- Use `array_map`, `array_filter`, `array_reduce` over foreach when appropriate.
- Consider using Collection classes (Laravel Collections, Doctrine Collections) for complex operations.

### Null Safety

- Use null coalescing: `$value ?? 'default'`.
- Use nullsafe operator: `$user?->getAddress()?->getCity()`.
- Never return `null` for collections -- return empty array.
- Type nullable explicitly: `?Type` or `Type|null`.

### Dependency Injection

- Use constructor injection -- never `new` inside business logic.
- Depend on interfaces, not concrete implementations.
- Use a DI container (Symfony DI, Laravel Container, PHP-DI).
- Avoid service locators and static calls to containers.

### Database & Doctrine/Eloquent

- Use an ORM (Doctrine, Eloquent) or query builder -- avoid raw SQL unless performance-critical.
- Use migrations for schema changes -- never modify production schemas manually.
- Use repositories to encapsulate queries -- controllers don't query directly.
- Use transactions for multi-step operations.
- Always use prepared statements / parameter binding.

### Security

- Never trust user input -- validate and sanitize everything.
- Use prepared statements -- never concatenate SQL.
- Escape output: `htmlspecialchars()` for HTML, JSON encode for JSON.
- Use CSRF tokens for forms.
- Hash passwords with `password_hash()` and verify with `password_verify()`.
- Store secrets in environment variables -- never in code.

### Autoloading & Namespaces

- Use PSR-4 autoloading via Composer.
- Namespace matches directory structure: `App\Services\UserService` → `src/Services/UserService.php`.
- One namespace declaration per file, at the top after `declare(strict_types=1)`.
- Group `use` statements: PHP classes, then external packages, then internal classes.

### Testing

- Use PHPUnit or Pest for unit/integration tests.
- One test class per production class.
- Test method names describe behavior: `test_user_can_be_created_with_valid_email`.
- Use data providers for testing multiple scenarios.
- Mock external dependencies -- don't hit real databases/APIs in unit tests.

### Code Style

- Follow PSR-12 coding style.
- Use PHP-CS-Fixer or PHP_CodeSniffer for automatic formatting.
- Max 120 characters per line.
- Use PHPStan or Psalm for static analysis (level 8+ recommended).

## Project Structure

```
project/
├── src/                        # Application code (PSR-4: App\)
│   ├── Controller/
│   ├── Service/
│   ├── Repository/
│   ├── Entity/                 # Domain models
│   ├── Dto/                    # Data Transfer Objects
│   ├── Exception/
│   └── ValueObject/
├── tests/
│   ├── Unit/
│   └── Integration/
├── config/
├── public/
│   └── index.php               # Entry point
├── var/                        # Cache, logs (gitignored)
├── vendor/                     # Composer dependencies (gitignored)
├── .env                        # Environment variables (gitignored)
├── .env.example
├── composer.json
├── phpunit.xml
└── phpstan.neon
```

## Questions to Ask

- PHP version (8.1, 8.2, 8.3, 8.4)?
- Framework: Laravel, Symfony, slim, or none?
- Database: MySQL, PostgreSQL, SQLite?
- ORM: Doctrine, Eloquent, or query builder only?
- Testing: PHPUnit or Pest?
- Static analysis: PHPStan, Psalm, or none?
- API only or with views (Blade, Twig)?

## Gitignore (PHP)

```
/vendor/
/var/
/.env
/.env.local
/.phpunit.result.cache
/phpstan.neon.local
```
