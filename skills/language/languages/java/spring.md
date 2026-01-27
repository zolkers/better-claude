# Spring

## Detection

This guide applies when `spring-boot`, `spring-framework`, or `org.springframework` is found in `pom.xml` or `build.gradle` dependencies.

## Annotations

- `@Service`, `@Repository`, `@Controller`, `@RestController` -- use the right stereotype.
- `@Transactional` on service methods that modify data, not on repository layer.
- `@Value` or `@ConfigurationProperties` for config -- never hardcode values.
- `@Autowired` via constructor injection (implicit with single constructor), not field injection.

## JPA

- `@Entity`, `@Table`, `@Column` -- always explicit table/column names, don't rely on defaults.
- `@Enumerated(EnumType.STRING)` -- never store enums as ordinals.
- Use `@ManyToOne(fetch = FetchType.LAZY)` by default -- eager loading only when justified.
- Always define `@JoinColumn` explicitly on relationship mappings.
- Use `@CreatedDate` / `@LastModifiedDate` with Spring Data auditing for timestamps.

## Pagination & Sorting

- Never return a full `List` from an API if the table can grow beyond 100 entries: use `Page<T>` or `Slice<T>`.
- Accept the `Pageable` argument directly in `@RestController` methods to leverage Spring Data's automatic binding.
- Use `Slice` instead of `Page` if calculating the total element count (`count` query) is not necessary (improves performance).
- Always define a default `Sort` to ensure consistent result ordering.
- Repository methods for large datasets must return `Page<T>` or `Slice<T>` instead of `List<T>`.

## REST Conventions

- Use proper HTTP methods: `GET` for reads, `POST` for creates, `PUT` for full updates, `PATCH` for partial, `DELETE` for removal.
- Return proper status codes: `201` for created, `204` for no content, `400` for bad request, `404` for not found.
- Use `@Valid` on request body parameters to trigger Bean Validation.
- Use `@ExceptionHandler` or `@ControllerAdvice` for centralized error handling -- never let raw exceptions leak to the client.
- Use DTOs for request/response -- never expose entities directly.

## Configuration

- Use `application.yml` over `application.properties` for readability.
- Separate configs per environment: `application-dev.yml`, `application-prod.yml`.
- Externalize secrets -- never commit credentials, use environment variables or a vault.

## Security (Spring Security)

- Never store passwords in plain text -- use `BCryptPasswordEncoder`.
- Use method-level security (`@PreAuthorize`, `@Secured`) over URL-based security when possible.
- Always configure CORS explicitly -- never use `permitAll()` in production.
- Disable CSRF only for stateless APIs (JWT), never for session-based apps.
- Use `SecurityFilterChain` bean (modern) over `WebSecurityConfigurerAdapter` (deprecated).

## Scheduling & Async

- Use `@Async` for fire-and-forget tasks -- always return `void` or `CompletableFuture`.
- Use `@Scheduled` with cron expressions for recurring tasks.
- Always configure a custom `TaskExecutor` -- never rely on the default single-thread executor.

## Testing

- Use `@SpringBootTest` sparingly -- prefer `@WebMvcTest`, `@DataJpaTest`, or plain unit tests.
- Use `@MockBean` to mock dependencies in integration tests.
- Test slices are faster and more focused than full context boot.
