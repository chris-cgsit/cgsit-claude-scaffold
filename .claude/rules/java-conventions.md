---
globs: ["**/*.java"]
---

# Java Conventions

## General
- Java 21+ features: records, sealed classes, pattern matching, virtual threads where appropriate
- Use `var` for local variables when the type is obvious from context
- Prefer immutable objects — use records for DTOs and value objects
- No Lombok in new code — use records and explicit constructors instead

## Dependency Injection
- Constructor injection only, never field injection (`@Autowired` on fields)
- Use `final` fields for all injected dependencies
- For Spring: `@RequiredArgsConstructor` (Lombok) or explicit constructor
- For CDI/Quarkus: constructor injection with `@Inject`

## Naming
- Classes: `PascalCase`
- Methods/variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Packages: `com.cgsit.[project].[module]`

## Error Handling
- Use domain-specific exceptions, not generic `RuntimeException`
- Always log exceptions with context (what operation, what input)
- Return `Optional<T>` instead of null for query methods
- Use `@ControllerAdvice` / `@ExceptionHandler` for REST error responses

## Logging
- Use SLF4J (`LoggerFactory.getLogger(MyClass.class)`)
- Log levels: ERROR (failures), WARN (recoverable), INFO (business events), DEBUG (dev details)
- Never log sensitive data (passwords, tokens, PII)
