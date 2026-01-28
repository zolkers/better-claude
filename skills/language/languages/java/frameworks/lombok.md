# Lombok

## Detection

This guide applies when `lombok` is found in `pom.xml` or `build.gradle` dependencies.

## Conventions

- Prefer `@RequiredArgsConstructor` over `@AllArgsConstructor`.
- Use `@Getter` / `@Setter` instead of boilerplate. `@Data` for DTOs.
- Use `@Builder` for classes with 3+ fields.
- Use `@Slf4j` for logger injection instead of manual `LoggerFactory.getLogger()`.
- Use `@EqualsAndHashCode(callSuper = true)` when extending a class -- never rely on the default.
- Avoid `@ToString` on entities with lazy-loaded relations -- it triggers unnecessary queries.
- Use `@Value` (Lombok) for immutable classes -- generates `final` fields, all-args constructor, getters, `equals`, `hashCode`, `toString`.
- Prefer `@NoArgsConstructor(access = AccessLevel.PROTECTED)` on JPA entities over public no-arg constructors.
