# cgsit-claude-scaffold

A project scaffold template for Claude Code projects by CGS IT Solutions GmbH.

Provides a standardized structure for documentation, skills, agents, rules, and commands
that makes Claude Code dramatically more effective on larger software projects.

## Quick Start

```bash
# 1. Copy scaffold into your new or existing project
cp -r cgsit-claude-scaffold/.claude /path/to/your-project/
cp -r cgsit-claude-scaffold/docs /path/to/your-project/
cp -r cgsit-claude-scaffold/tasks /path/to/your-project/
cp cgsit-claude-scaffold/CLAUDE.md /path/to/your-project/

# 2. Open the project in Claude Code
cd /path/to/your-project
claude

# 3. Let Claude fill in the templates based on your project briefing
/project:init-docs
```

## Structure

```
├── CLAUDE.md                              ← Slim: conventions, references to docs/
│
├── .claude/
│   ├── settings.json                      ← Permissions, allowlists
│   ├── .mcp.json                          ← MCP server integrations
│   │
│   ├── rules/                             ← Path-scoped, auto-loaded by context
│   │   ├── java-conventions.md
│   │   ├── api-rules.md
│   │   ├── testing-rules.md
│   │   └── database-rules.md
│   │
│   ├── agents/                            ← Specialized sub-agents
│   │   ├── code-reviewer.md
│   │   ├── researcher.md
│   │   └── test-engineer.md
│   │
│   ├── skills/                            ← Reusable workflows
│   │   ├── rfcs/SKILL.md                  ← RFC lifecycle (list/new/update/status)
│   │   ├── commit/SKILL.md                ← Git commit conventions
│   │   ├── doc-initializer/SKILL.md
│   │   ├── test-generator/SKILL.md
│   │   └── architecture-reviewer/SKILL.md
│   │
│   └── commands/                          ← /project:xxx slash commands
│       ├── create-rfc.md
│       ├── plan-feature.md
│       ├── init-docs.md
│       └── review.md
│
├── docs/
│   ├── product_overview_requirements.md   ← Product vision, target audience
│   ├── architecture.md                    ← Software layers, system, cloud, deployment
│   ├── tech_stack.md                      ← Technologies, versions, rationale
│   ├── testing.md                         ← Test strategy, frameworks, conventions
│   ├── development_guide.md               ← Local setup, build, run, deploy
│   └── rfcs/
│       ├── TEMPLATE.md                    ← RFC template for consistent feature specs
│       └── 001-example-feature.md         ← Example RFC
│
└── tasks/
    └── lessons.md                         ← Corrections from previous sessions
```

## Workflow

### Starting a new project
1. Copy the scaffold into your project
2. Run `/project:init-docs` — Claude interviews you and fills in all doc templates
3. Commit the docs to your repo

### Planning a feature
1. Run `/project:create-rfc` or `/rfcs new "title"` to create an RFC
2. Review and set status to `Accepted`
3. Start a fresh session with `/clear`
4. Tell Claude to implement the RFC: `Implement docs/rfcs/001-my-feature.md`

### During development
- Claude loads rules from `.claude/rules/` automatically based on what files you're editing
- `/commit` for consistent, conventional commit messages
- `/rfcs` to manage RFC lifecycle (list, status, update)
- The `code-reviewer` agent reviews changes before commit
- The `researcher` agent verifies external APIs/libs against real docs
- Corrections go into `tasks/lessons.md` so Claude doesn't repeat mistakes

## Customization

- **CLAUDE.md**: Adjust code conventions, build commands, and Git workflow to your project
- **rules/**: Add or modify path-scoped rules for your specific patterns
- **agents/**: Tune agent prompts to your team's review standards
- **skills/**: Add project-specific skills as needed
- **docs/**: The templates are starting points — expand as your project grows

## License

MIT
