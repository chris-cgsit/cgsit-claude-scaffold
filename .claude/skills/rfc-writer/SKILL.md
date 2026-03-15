---
name: create-rfc
description: Create a new RFC feature specification. Use when planning a new feature, writing a feature spec, or when the user says "new RFC" or "plan feature".
---

# RFC Writer

Create a new RFC document following the project's template.

## Process

1. Read `docs/rfcs/TEMPLATE.md` for the expected format
2. Read `docs/architecture.md` to understand the current system
3. Read `docs/tech_stack.md` to know available technologies
4. **Interview the user** using the AskUserQuestion tool:
   - What problem does this feature solve?
   - Who is the target user?
   - What are the acceptance criteria?
   - What are the edge cases and error scenarios?
   - Are there performance or security concerns?
   - What existing components are affected?
   - Keep interviewing until all sections of the template can be filled
5. Determine the next RFC number by checking existing files in `docs/rfcs/`
6. Write the RFC to `docs/rfcs/[NNN]-[short-name].md`
7. Set status to `Draft`

## Quality Checks
- Every section from the template must be filled
- Architecture impact must reference specific components from `docs/architecture.md`
- Test strategy must be concrete, not generic
- Implementation checklist must be included (empty, to be filled during implementation)
