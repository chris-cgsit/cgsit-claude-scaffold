---
name: init-docs
description: Initialize project documentation by interviewing the user and filling in all doc templates. Use at the start of a new project or when the user says "init docs" or "set up documentation".
---

# Documentation Initializer

Fill in all documentation templates in `docs/` based on a user interview.

## Process

1. Read all template files in `docs/` to understand what needs to be filled
2. **Interview the user** about the project:
   - What is the product? Who is it for? What problem does it solve?
   - What is the high-level architecture? (Monolith, microservices, layers)
   - What technologies are used? (Framework, DB, cloud provider, etc.)
   - How is the project built, run, and deployed locally?
   - What is the testing approach? (Frameworks, types, CI integration)
3. Fill in each document:
   - `docs/product_overview_requirements.md`
   - `docs/architecture.md`
   - `docs/tech_stack.md`
   - `docs/testing.md`
   - `docs/development_guide.md`
4. Update `CLAUDE.md` with project-specific build/test/run commands

## Rules
- Keep documents concise — these are reference docs, not novels
- Use concrete details from the interview, not generic placeholders
- If the user doesn't know something yet, mark it as `<!-- TODO: define -->`
- Do not invent information — only write what the user confirmed
