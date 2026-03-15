# Lessons Learned

This file tracks corrections and patterns discovered during development.
Claude reads this before starting work to avoid repeating past mistakes.

<!-- Format:
## [Date] - [Short description]
**Context**: What was happening
**Mistake**: What went wrong
**Correction**: What to do instead
-->

<!-- Example:
## 2025-01-15 - Field injection used in UserService
**Context**: Implementing RFC-001 User Authentication
**Mistake**: Used `@Autowired` on fields instead of constructor injection
**Correction**: Always use constructor injection with `final` fields. See `.claude/rules/java-conventions.md`
-->
