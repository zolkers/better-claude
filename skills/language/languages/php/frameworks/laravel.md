# Laravel

Inherits from `php/php.md`.

## Detection

This guide applies when `laravel/framework` is found in `composer.json` or `artisan` file exists at project root.

## Conventions

### Architecture

- Use Laravel's conventions -- don't fight the framework.
- Keep controllers thin: validate, call service/action, return response.
- Use Form Requests for validation -- not in controllers.
- Use Resources/Transformers for API responses.
- Use Actions or Services for business logic -- controllers orchestrate, don't implement.

### Controllers

- One resource per controller (CRUD operations).
- Use resource controllers: `index`, `show`, `store`, `update`, `destroy`.
- Use invokable controllers (`__invoke`) for single-action endpoints.
- Max 5-7 methods per controller -- split if growing.
- Return responses explicitly: `response()->json()`, `redirect()`, `view()`.

```php
final class UserController extends Controller
{
    public function __construct(
        private readonly UserService $userService,
    ) {}

    public function index(): JsonResponse
    {
        return UserResource::collection(
            $this->userService->getAllPaginated()
        )->response();
    }

    public function store(StoreUserRequest $request): JsonResponse
    {
        $user = $this->userService->create($request->validated());

        return UserResource::make($user)
            ->response()
            ->setStatusCode(Response::HTTP_CREATED);
    }
}
```

### Form Requests

- One request class per action: `StoreUserRequest`, `UpdateUserRequest`.
- Put authorization logic in `authorize()` method.
- Use custom messages in `messages()` for user-friendly errors.
- Use `prepareForValidation()` to sanitize input before validation.

```php
final class StoreUserRequest extends FormRequest
{
    public function authorize(): bool
    {
        return $this->user()->can('create', User::class);
    }

    public function rules(): array
    {
        return [
            'email' => ['required', 'email', 'unique:users,email'],
            'name' => ['required', 'string', 'max:255'],
            'password' => ['required', 'string', 'min:8', 'confirmed'],
        ];
    }
}
```

### Eloquent Models

- Define `$fillable` or `$guarded` -- prefer `$fillable` for explicit mass assignment.
- Use `$casts` for type casting: dates, enums, JSON, encrypted.
- Define relationships explicitly with return types.
- Use scopes for reusable query constraints.
- Avoid business logic in models -- models are data + relationships + scopes.

```php
final class User extends Authenticatable
{
    protected $fillable = ['name', 'email', 'password'];

    protected $hidden = ['password', 'remember_token'];

    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
            'status' => UserStatus::class,
        ];
    }

    public function orders(): HasMany
    {
        return $this->hasMany(Order::class);
    }

    public function scopeActive(Builder $query): Builder
    {
        return $query->where('status', UserStatus::Active);
    }
}
```

### Services & Actions

- Use Service classes for complex business logic spanning multiple models.
- Use Action classes for single-purpose operations: `CreateUserAction`, `SendInvoiceAction`.
- Actions are invokable: one public `__invoke()` or `execute()` method.
- Services/Actions receive dependencies via constructor, data via method parameters.

```php
final class CreateUserAction
{
    public function __construct(
        private readonly UserRepository $repository,
        private readonly Dispatcher $events,
    ) {}

    public function execute(CreateUserDto $dto): User
    {
        $user = $this->repository->create([
            'name' => $dto->name,
            'email' => $dto->email,
            'password' => Hash::make($dto->password),
        ]);

        $this->events->dispatch(new UserCreated($user));

        return $user;
    }
}
```

### API Resources

- Use API Resources for all JSON responses -- never return models directly.
- Create separate resources for list vs detail: `UserResource`, `UserDetailResource`.
- Use `whenLoaded()` for conditional relationship inclusion.
- Use Resource Collections for paginated responses.

```php
final class UserResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email,
            'created_at' => $this->created_at->toIso8601String(),
            'orders' => OrderResource::collection($this->whenLoaded('orders')),
        ];
    }
}
```

### Routes

- Use route groups for prefixes, middleware, and naming.
- Use `apiResource()` for REST APIs, `resource()` for web.
- Name all routes -- never use URLs directly in code.
- Use route model binding with custom keys when needed.

```php
Route::prefix('api/v1')->middleware('auth:sanctum')->group(function () {
    Route::apiResource('users', UserController::class);
    Route::post('users/{user}/activate', ActivateUserController::class)->name('users.activate');
});
```

### Middleware

- Use middleware for cross-cutting concerns: auth, rate limiting, CORS, logging.
- Create custom middleware for domain-specific checks.
- Register middleware in `bootstrap/app.php` (Laravel 11+) or `app/Http/Kernel.php`.

### Events & Listeners

- Use events for decoupling: `UserCreated`, `OrderShipped`.
- One listener per side effect -- don't put multiple actions in one listener.
- Queue listeners for slow operations: emails, webhooks, reports.
- Use event subscribers to group related listeners.

### Jobs & Queues

- Queue slow operations: emails, file processing, API calls to external services.
- Implement `ShouldQueue` interface.
- Set `$tries`, `$backoff`, `$timeout` for reliability.
- Use `$uniqueFor` to prevent duplicate jobs.
- Handle failures in `failed()` method.

```php
final class SendWelcomeEmail implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public int $tries = 3;
    public int $backoff = 60;

    public function __construct(
        public readonly User $user,
    ) {}

    public function handle(Mailer $mailer): void
    {
        $mailer->to($this->user)->send(new WelcomeMail($this->user));
    }
}
```

### Database

- Use migrations for all schema changes.
- Use seeders and factories for test data.
- Use database transactions for multi-model operations.
- Avoid N+1: use `with()` for eager loading.
- Use chunking for large datasets: `chunk()`, `chunkById()`, `lazy()`.

### Testing

- Use `RefreshDatabase` trait for integration tests.
- Use factories for test data: `User::factory()->create()`.
- Use `actingAs()` for authenticated requests.
- Test HTTP endpoints with `$this->getJson()`, `$this->postJson()`, etc.
- Use Pest or PHPUnit -- Pest is more concise.

```php
it('creates a user with valid data', function () {
    $response = $this->postJson('/api/users', [
        'name' => 'John Doe',
        'email' => 'john@example.com',
        'password' => 'password123',
        'password_confirmation' => 'password123',
    ]);

    $response->assertCreated()
        ->assertJsonPath('data.email', 'john@example.com');

    $this->assertDatabaseHas('users', ['email' => 'john@example.com']);
});
```

### Configuration

- Use `config()` helper -- never `env()` outside config files.
- Cache config in production: `php artisan config:cache`.
- Use `.env` for environment-specific values, config files for defaults.

## Project Structure (Laravel 11+)

```
app/
├── Actions/                    # Single-purpose action classes
├── Console/Commands/
├── Dto/                        # Data Transfer Objects
├── Enums/
├── Events/
├── Exceptions/
├── Http/
│   ├── Controllers/
│   ├── Middleware/
│   ├── Requests/               # Form Requests
│   └── Resources/              # API Resources
├── Jobs/
├── Listeners/
├── Mail/
├── Models/
├── Notifications/
├── Policies/
├── Providers/
├── Repositories/               # Optional: data access layer
└── Services/                   # Business logic services
bootstrap/
config/
database/
├── factories/
├── migrations/
└── seeders/
public/
resources/
routes/
storage/
tests/
├── Feature/
└── Unit/
```

## Questions to Ask

- Laravel version (10, 11)?
- API only or with Blade views?
- Auth: Sanctum, Passport, or Fortify?
- Admin panel: Filament, Nova, or custom?
- Testing: Pest or PHPUnit?
- Queue driver: Redis, database, SQS?
- Using Livewire or Inertia.js?
