# Testing Strategy

## Overview
<!-- What is the testing philosophy for this project? -->
[TODO: Brief testing philosophy — e.g., "Test behavior, not implementation"]

## Test Pyramid

### Unit Tests
- **Scope**: Individual classes, business logic
- **Framework**: JUnit 5 + AssertJ + Mockito
- **Location**: `src/test/java/` mirroring main structure
- **Run**: `[TODO: mvn test / ./gradlew test]`
- **Coverage target**: [TODO: e.g., 80% line coverage on service layer]

### Integration Tests
- **Scope**: Component interaction, database queries, API endpoints
- **Framework**: Spring Boot Test + Testcontainers
- **Location**: `src/test/java/` with `@Tag("integration")`
- **Run**: `[TODO: mvn verify / ./gradlew integrationTest]`
- **Database**: Testcontainers PostgreSQL — no H2, no mocks

### E2E Tests
- **Scope**: [TODO: Full user flows? API-level only?]
- **Framework**: [TODO: if applicable]
- **Run**: [TODO]

## Test Conventions
- Naming: `shouldDoExpectedBehavior_whenCondition()`
- Pattern: Arrange → Act → Assert (separated by blank lines)
- One logical assertion per test
- Test data via factory methods (`TestDataFactory.createXyz()`)

## What Must Be Tested
- All service layer public methods (happy + error path)
- All REST endpoints (status codes, validation, error responses)
- Database migrations (must run cleanly on empty DB)
- Security rules (authenticated vs. unauthenticated access)

## What Should NOT Be Tested
- Getters, setters, trivial mappings
- Framework internals (Spring, Hibernate)
- Third-party library behavior

## CI Integration
<!-- How do tests run in CI? -->
[TODO: Describe CI pipeline test stages]
