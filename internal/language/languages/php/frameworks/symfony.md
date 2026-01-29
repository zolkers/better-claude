# Symfony

Inherits from `php/php.md`.

## Detection

This guide applies when `symfony/framework-bundle` is found in `composer.json` or `symfony.lock` file exists.

## Conventions

### Architecture

- Follow Symfony's conventions and best practices.
- Use autowiring and autoconfiguration -- minimal manual service config.
- Keep controllers thin: validate, call service, return response.
- Use DTOs for request/response data -- not entities.
- Use services for business logic -- controllers orchestrate.

### Controllers

- Extend `AbstractController` for convenience methods.
- Use attributes for routing: `#[Route]`.
- One action per method, or use invokable controllers.
- Return `Response` objects explicitly.
- Use `#[MapRequestPayload]` and `#[MapQueryString]` for automatic DTO mapping (Symfony 6.3+).

```php
#[Route('/api/users', name: 'api_users_')]
final class UserController extends AbstractController
{
    public function __construct(
        private readonly UserService $userService,
    ) {}

    #[Route('', name: 'index', methods: ['GET'])]
    public function index(): JsonResponse
    {
        $users = $this->userService->findAll();

        return $this->json($users);
    }

    #[Route('', name: 'create', methods: ['POST'])]
    public function create(#[MapRequestPayload] CreateUserDto $dto): JsonResponse
    {
        $user = $this->userService->create($dto);

        return $this->json($user, Response::HTTP_CREATED);
    }

    #[Route('/{id}', name: 'show', methods: ['GET'])]
    public function show(User $user): JsonResponse
    {
        return $this->json($user);
    }
}
```

### DTOs & Validation

- Use DTOs for all input data -- never bind forms directly to entities.
- Use Symfony Validator constraints as attributes.
- Use `#[MapRequestPayload]` for automatic JSON to DTO mapping.
- Use `#[Assert\Valid]` for nested DTO validation.

```php
final readonly class CreateUserDto
{
    public function __construct(
        #[Assert\NotBlank]
        #[Assert\Email]
        public string $email,

        #[Assert\NotBlank]
        #[Assert\Length(min: 2, max: 255)]
        public string $name,

        #[Assert\NotBlank]
        #[Assert\Length(min: 8)]
        public string $password,
    ) {}
}
```

### Services

- One service per domain concept: `UserService`, `OrderService`.
- Use constructor injection -- autowiring handles the rest.
- Services are `final` by default.
- Use interfaces for services that might have multiple implementations.
- Tag services with attributes when needed: `#[AsEventListener]`, `#[AsCommand]`.

```php
#[Autoconfigure(lazy: true)]
final class UserService
{
    public function __construct(
        private readonly UserRepository $repository,
        private readonly EventDispatcherInterface $dispatcher,
        private readonly ValidatorInterface $validator,
    ) {}

    public function create(CreateUserDto $dto): User
    {
        $errors = $this->validator->validate($dto);
        if (count($errors) > 0) {
            throw new ValidationException($errors);
        }

        $user = new User(
            email: $dto->email,
            name: $dto->name,
        );
        $user->setPassword($dto->password); // Hashed in entity or listener

        $this->repository->save($user, flush: true);
        $this->dispatcher->dispatch(new UserCreatedEvent($user));

        return $user;
    }
}
```

### Doctrine Entities

- Use attributes for mapping -- not XML or YAML.
- Entities are not DTOs -- don't expose them directly in APIs.
- Use repository pattern -- custom repository per entity.
- Encapsulate state changes in entity methods.
- Use value objects for complex types: `Email`, `Money`, `Address`.

```php
#[ORM\Entity(repositoryClass: UserRepository::class)]
#[ORM\Table(name: 'users')]
class User
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column]
    private ?int $id = null;

    #[ORM\Column(length: 180, unique: true)]
    private string $email;

    #[ORM\Column]
    private string $name;

    #[ORM\Column]
    private string $password;

    #[ORM\Column(enumType: UserStatus::class)]
    private UserStatus $status = UserStatus::Pending;

    #[ORM\OneToMany(mappedBy: 'user', targetEntity: Order::class)]
    private Collection $orders;

    public function __construct(string $email, string $name)
    {
        $this->email = $email;
        $this->name = $name;
        $this->orders = new ArrayCollection();
    }

    public function activate(): void
    {
        if ($this->status !== UserStatus::Pending) {
            throw new \DomainException('User is already activated');
        }
        $this->status = UserStatus::Active;
    }

    // Getters...
}
```

### Repositories

- Extend `ServiceEntityRepository` for type safety.
- Create custom query methods -- don't use QueryBuilder in services.
- Use `find()`, `findOneBy()` for simple queries.
- Return `null` or throw custom exceptions for not found cases.
- Use pagination for list queries.

```php
/**
 * @extends ServiceEntityRepository<User>
 */
final class UserRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, User::class);
    }

    public function save(User $user, bool $flush = false): void
    {
        $this->getEntityManager()->persist($user);
        if ($flush) {
            $this->getEntityManager()->flush();
        }
    }

    public function findByStatus(UserStatus $status): array
    {
        return $this->createQueryBuilder('u')
            ->andWhere('u.status = :status')
            ->setParameter('status', $status)
            ->orderBy('u.createdAt', 'DESC')
            ->getQuery()
            ->getResult();
    }
}
```

### Events & Listeners

- Use event dispatcher for decoupling.
- Create domain events: `UserCreatedEvent`, `OrderShippedEvent`.
- Use `#[AsEventListener]` attribute for listeners.
- One listener per side effect.

```php
final readonly class UserCreatedEvent
{
    public function __construct(
        public User $user,
    ) {}
}

#[AsEventListener(event: UserCreatedEvent::class)]
final class SendWelcomeEmailListener
{
    public function __construct(
        private readonly MailerInterface $mailer,
    ) {}

    public function __invoke(UserCreatedEvent $event): void
    {
        $email = (new Email())
            ->to($event->user->getEmail())
            ->subject('Welcome!')
            ->html($this->renderTemplate($event->user));

        $this->mailer->send($email);
    }
}
```

### Messenger (Async)

- Use Messenger for async operations: emails, reports, external API calls.
- Create message + handler pairs.
- Configure transports in `messenger.yaml`.
- Use stamps for metadata: delay, priority.

```php
final readonly class SendWelcomeEmailMessage
{
    public function __construct(
        public int $userId,
    ) {}
}

#[AsMessageHandler]
final class SendWelcomeEmailHandler
{
    public function __construct(
        private readonly UserRepository $repository,
        private readonly MailerInterface $mailer,
    ) {}

    public function __invoke(SendWelcomeEmailMessage $message): void
    {
        $user = $this->repository->find($message->userId);
        if ($user === null) {
            return;
        }

        // Send email...
    }
}
```

### Security

- Use Security bundle for authentication and authorization.
- Use voters for complex authorization logic.
- Use `#[IsGranted]` attribute on controllers.
- Use firewalls for different auth strategies (API vs web).

```php
#[Route('/api/users/{id}', methods: ['DELETE'])]
#[IsGranted('ROLE_ADMIN')]
public function delete(User $user): JsonResponse
{
    $this->userService->delete($user);

    return $this->json(null, Response::HTTP_NO_CONTENT);
}

// Custom voter
final class UserVoter extends Voter
{
    protected function supports(string $attribute, mixed $subject): bool
    {
        return $subject instanceof User && in_array($attribute, ['VIEW', 'EDIT', 'DELETE']);
    }

    protected function voteOnAttribute(string $attribute, mixed $subject, TokenInterface $token): bool
    {
        $currentUser = $token->getUser();

        return match ($attribute) {
            'VIEW' => true,
            'EDIT', 'DELETE' => $currentUser === $subject || $this->isAdmin($currentUser),
            default => false,
        };
    }
}
```

### Forms (Web)

- Use Form component for web forms.
- Create form types: `UserType`, `OrderType`.
- Bind forms to DTOs, not entities.
- Use form events for dynamic fields.

### API Platform (Optional)

- Consider API Platform for REST/GraphQL APIs.
- Use attributes for resource configuration.
- Customize with state providers/processors.

### Testing

- Use WebTestCase for functional tests.
- Use KernelTestCase for integration tests.
- Use `->getContainer()` for service access in tests.
- Use Foundry or Doctrine Fixtures for test data.

```php
final class UserControllerTest extends WebTestCase
{
    public function testCreateUser(): void
    {
        $client = static::createClient();

        $client->request('POST', '/api/users', [], [], [
            'CONTENT_TYPE' => 'application/json',
        ], json_encode([
            'email' => 'test@example.com',
            'name' => 'Test User',
            'password' => 'password123',
        ]));

        $this->assertResponseStatusCodeSame(201);
        $this->assertJsonContains(['email' => 'test@example.com']);
    }
}
```

### Configuration

- Use YAML for config, attributes for code.
- Use environment variables for secrets: `%env(DATABASE_URL)%`.
- Use `config/packages/` for bundle configuration.
- Use `services.yaml` only for special cases -- autowiring handles most.

## Project Structure

```
config/
├── packages/                   # Bundle configuration
├── routes/
├── services.yaml
migrations/
public/
├── index.php
src/
├── Command/                    # Console commands
├── Controller/
├── Dto/
│   ├── Request/                # Input DTOs
│   └── Response/               # Output DTOs
├── Entity/
├── Enum/
├── Event/
├── EventListener/
├── Exception/
├── Message/                    # Messenger messages
├── MessageHandler/
├── Repository/
├── Security/
│   └── Voter/
├── Service/
├── Validator/                  # Custom constraints
└── Kernel.php
templates/                      # Twig templates (if web)
tests/
├── Functional/
├── Integration/
└── Unit/
translations/
var/                            # Cache, logs (gitignored)
vendor/                         # (gitignored)
```

## Questions to Ask

- Symfony version (6.4 LTS, 7.x)?
- API only or with Twig templates?
- Using API Platform?
- Database: PostgreSQL, MySQL, SQLite?
- Async: Messenger with which transport (Redis, RabbitMQ, Doctrine)?
- Auth: Security bundle, LexikJWT, custom?
- Admin: EasyAdmin, Sonata, or custom?
- Testing: PHPUnit, Pest?
