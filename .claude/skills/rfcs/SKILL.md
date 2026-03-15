---
name: rfcs
description: Manage RFCs (architecture decisions and work packages). List, update status, create new RFCs, or check dependencies.
user-invocable: true
allowed-tools: Read, Edit, Write, Glob, Grep, Bash(ls *)
argument-hint: [list|status|new "title"|update NNN status]
---

# RFC Management

Manage RFCs in `docs/rfcs/`. RFCs track architecture decisions, migration plans, and work packages.

## RFC Location

- Directory: `docs/rfcs/`
- Index: `docs/rfcs/README.md`
- Template: `docs/rfcs/TEMPLATE.md`
- Format: `NNN-kebab-case-title.md`

## Status Values

| Status | Meaning |
|---|---|
| Draft | Initial draft, not yet approved |
| Accepted | Approved, ready for implementation |
| In Progress | Actively being worked on |
| In Review | Implemented, under review (e.g. open branch) |
| Done | Completed |
| Rejected | Rejected / not implemented |

## Commands

### `/rfcs` or `/rfcs list`
Show all RFCs with status from `docs/rfcs/README.md`.

### `/rfcs status`
Show only active RFCs (In Progress, In Review) and their dependencies.

### `/rfcs update NNN status`
Update the status of RFC NNN. Changes both the RFC file header and `docs/rfcs/README.md`.
Example: `/rfcs update 010 Done`

### `/rfcs new "title"`
Create a new RFC with the next available number. Use the standard template below.

## RFC Template

Use the template from `docs/rfcs/TEMPLATE.md`. When creating a new RFC with `/rfcs new`:

1. Copy `docs/rfcs/TEMPLATE.md` to `docs/rfcs/NNN-kebab-case-title.md`
2. Replace `RFC-XXX` with the correct number
3. Set status to `Draft`
4. Fill in the metadata table
5. Add entry to `docs/rfcs/README.md`

## Rules

- Always update BOTH the RFC file AND `docs/rfcs/README.md` when changing status
- When planning new work, read active RFCs first to check for dependencies and context
- New RFC numbers are sequential (find highest existing number + 1)
- Check dependency chain before marking an RFC as Done
