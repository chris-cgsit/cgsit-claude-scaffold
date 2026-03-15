# Testing Rules

## Structure
- Follow AAA pattern: Arrange, Act, Assert — separated by blank lines
- One logical assertion per test (multiple asserts OK if testing same behavior)
- Test method naming: `shouldDoExpectedBehavior_whenCondition()`
- Test class naming: `[ClassUnderTest]Test`

## Unit Tests
- Use JUnit 5 (`@Test`, `@BeforeEach`, `@ParameterizedTest`)
- Use AssertJ for assertions (`assertThat(...).isEqualTo(...)`)
- Mock external dependencies with Mockito
- No Spring context for pure unit tests — just `new MyService(mockDep)`

## Integration Tests
- Use `@SpringBootTest` or `@QuarkusTest` only when testing real wiring
- Use Testcontainers for database tests — never mock the database
- Use `@Transactional` + rollback for DB test isolation
- Tag with `@Tag("integration")` for separate test execution

## What to Test
- Every public method in service layer
- Happy path + at least one error/edge case per method
- Validation logic (invalid inputs, boundary values)
- Do NOT test getters/setters, framework code, or trivial mappings

## Test Data
- Use factory methods or builders for test data (`TestDataFactory.createUser()`)
- Never rely on external state or shared mutable test data
- Keep test data minimal — only set fields relevant to the test
