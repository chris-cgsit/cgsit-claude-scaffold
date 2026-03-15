# Review Current Changes

Review the code changes in the current branch against project standards.

1. Run `git diff main --name-only` to see changed files
2. Use the code-reviewer agent (`.claude/agents/code-reviewer.md`) checklist
3. Use the architecture-reviewer skill to check architecture compliance
4. Read `.claude/rules/` for relevant coding rules
5. Check `tasks/lessons.md` for known pitfalls

Report findings grouped by severity (Critical → Warning → Suggestion).
If everything looks good, say so — don't invent issues.
