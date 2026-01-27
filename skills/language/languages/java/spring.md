# Spring

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

## Testing

- Use `@SpringBootTest` sparingly -- prefer `@WebMvcTest`, `@DataJpaTest`, or plain unit tests.
- Use `@MockBean` to mock dependencies in integration tests.
- Test slices are faster and more focused than full context boot.
