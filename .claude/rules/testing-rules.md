---
globs: ["**/*Test.java", "**/*Tests.java", "**/e2e/*.spec.ts", "**/test/**/*.java"]
---

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

## E2E Tests (Playwright) — Phase 1: Smoke Tests

We are in **Phase 1** (App im aktiven Umbau). Rules:

- **Only smoke tests** — test that core flows work, not layout/styling
- **No CSS class selectors** — never use `.summary-card`, `.state-bar` etc.
- Use `getByText()`, `getByRole()`, `getByPlaceholder()`, `getByTitle()`, or `data-testid`
- **No layout assertions** — don't test position, size, or visual appearance
- Each test max 10-15 lines, testing ONE flow
- Tests live in `cgsit-finance-web/e2e/smoke.spec.ts`
- Run: `npm run e2e` from `cgsit-finance-web/`

### What to test (Phase 1)
- App loads, navigation works (sidebar links reach correct URLs)
- Portfolio dashboard shows portfolios, can open one
- Portfolio detail shows KPIs and positions
- Securities page loads with search
- Admin pages (users, groups) load

### What NOT to test (Phase 1)
- Specific CSS classes, DOM structure, or element counts
- Dark mode styling, responsive layout, animations
- Exact text formatting (numbers, dates)
- Complex multi-step workflows (create → edit → delete)

### Later phases
- **Phase 2** (UI stable): detailed tests with `data-testid` attributes
- **Phase 3** (production): regression, cross-browser, visual comparison
- See `docs/testing.md` for full strategy
