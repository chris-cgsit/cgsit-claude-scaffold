---
name: review-architecture
description: Review code changes against the project's architecture definition. Use when reviewing PRs, after implementing a feature, or when the user asks to "check architecture" or "review against architecture".
---

# Architecture Reviewer

Verify that code changes align with the documented architecture.

## Process

1. Read `docs/architecture.md` for the defined architecture
2. Read `docs/tech_stack.md` for approved technologies
3. Analyze the changed/added files:
   - Are they in the correct layer/package?
   - Do dependencies flow in the right direction?
   - Are there forbidden cross-layer dependencies?
   - Are new technologies introduced that aren't in tech_stack.md?
4. Check the relevant RFC in `docs/rfcs/` for the feature's architecture impact
5. Report findings

## Architecture Checks
- **Layer violations**: Controller → Service → Repository (never skip layers)
- **Package structure**: Follows the defined module/package organization
- **Dependency direction**: Upper layers depend on lower layers, never reverse
- **No circular dependencies** between packages/modules
- **New dependencies**: Any new Maven/Gradle dependency must be justified
- **Infrastructure**: Cloud resources match `docs/architecture.md` definitions

## Output Format
- ✅ **Compliant**: [what aligns well]
- ⚠️ **Warning**: [minor deviations, suggest improvement]
- ❌ **Violation**: [what breaks the architecture, must be fixed]
