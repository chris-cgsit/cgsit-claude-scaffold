# Project: [PROJECT_NAME]

## Quick Reference
- Build: `[BUILD_COMMAND]`
- Test: `[TEST_COMMAND]`
- Run: `[RUN_COMMAND]`
- Lint: `[LINT_COMMAND]`

## Code Conventions
- Java 21+, use records over POJOs where possible
- Constructor injection only, no field injection
- Prefer `Optional` return types over null
- Use `var` for local variables when type is obvious
- German comments are OK, code and identifiers always English

## Git Conventions
- Commit messages in English, imperative mood
- Branch naming: `feature/rfc-XXX-short-description`
- Always create a branch per RFC, never work on main directly

## Project Documentation
Before implementing features, read the relevant docs:
- **Product scope & requirements:** `docs/product_overview_requirements.md`
- **Architecture decisions:** `docs/architecture.md`
- **Tech stack:** `docs/tech_stack.md`
- **Test strategy:** `docs/testing.md`
- **Local setup & build:** `docs/development_guide.md`
- **Feature specs:** `docs/rfcs/` (read the RFC relevant to your current task)

## Rules
Path-specific rules are in `.claude/rules/` — they load automatically based on context.

## Lessons Learned
Check `tasks/lessons.md` before starting work — it contains corrections from previous sessions.

## Compaction Instructions
When compacting, always preserve:
- The current RFC being implemented and its checklist status
- All modified file paths
- Any failing test details
- The current branch name
