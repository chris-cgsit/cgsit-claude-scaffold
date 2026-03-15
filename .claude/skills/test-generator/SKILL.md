---
name: generate-tests
description: Generate tests for existing code following the project's testing strategy. Use when the user asks to "write tests", "add tests", "test this", or "cover this with tests".
---

# Test Generator

Generate tests for specified classes or features.

## Process

1. Read `docs/testing.md` for the project's test strategy
2. Read `.claude/rules/testing-rules.md` for conventions
3. Examine existing tests in `src/test/` for patterns and style
4. Analyze the target class(es):
   - Identify all public methods
   - Determine dependencies that need mocking
   - List happy paths, error paths, and edge cases
5. Generate test class(es) following project conventions
6. Run the tests to verify they compile and pass
7. Intentionally break one assertion to verify the test actually catches failures

## Test Case Selection
For each public method, generate at minimum:
- One happy path test
- One test for invalid/null input
- One test for error/exception scenarios
- Parameterized tests for methods with multiple valid input patterns

## Output
- Test files in the correct `src/test/java/` package
- Test data factories if needed (in a `testutil` package)
- Summary of what was tested and what was intentionally skipped (with reason)
