---
name: rfcs
description: Manage RFCs (architecture decisions and work packages) and — if the project uses a ticket system — the linked tickets. Decide RFC vs ticket-only, create/update specs in docs/rfcs/, run the interactive intake flow, and enforce the planning-discipline gates.
user-invocable: true
allowed-tools: Read, Edit, Write, Glob, Grep, Bash(ls *), Bash(gh issue *), Bash(gh label *)
argument-hint: [list|status|new "title"|ticket "title"|subticket NNN "title"|update NNN status|close NNN]
---

# RFC Management

Manage RFCs in `docs/rfcs/`. An RFC is a **specification document** that tracks an architecture
decision, migration plan, or work package. It lives in the repo, committed alongside the code.

If the project has a ticket/issue system (`[TICKET_SYSTEM]`), each non-trivial RFC is paired with a
**ticket** that tracks the work (open/in-progress/closed, discussion). The RFC documents the design,
the ticket tracks the work, and they reference each other. If `[TICKET_SYSTEM]` is "none", an RFC
lives purely as a file + a row in `docs/rfcs/README.md` — skip every ticket step below.

## Reference docs (read these)

- **Template (the one true source):** `.claude/skills/rfcs/TEMPLATE.md` — read it before creating an
  RFC. It lives *inside the skill* so the skill is self-contained: copy the directory into another
  repository and the template comes with it. Do
  NOT hand-roll a header or write an empty file.
- **Planning discipline (the WHY + the rules):** `docs/rfc-planning-discipline.md` — Atomic-Deploy,
  Design-Gate ↔ Closeout loop, Context-Isolation against "completion bleed", Split/Implementation
  discipline.

### Header mandatory fields
`Status`, `RFC-Type` (Bug-Fix · Feature · Refactor · Migration · Cleanup · Infra/Incident),
`Complexity` (Standard | High), `Priority` (Critical | High | Medium | Low), `Author`, `Created`,
`Affects`, `Branch`, `Reference Documentation (truth spec)`.

### Mandatory sections
`Done-Definition`, `Summary`, `Current State`, `Target State`, `Proposed Solution`, `Test Strategy`.
Sections marked *(when needed)* (Acceptance Criteria, Architecture-Impact, Security/Privacy,
Performance, Alternatives) may be dropped on small Bug-Fix/Cleanup RFCs. `Bug Details` is filled only
when `RFC-Type = Bug-Fix`. `Design Approval` is mandatory for Refactor/Migration + any
Privacy/Money/Cross-Tenant/load-bearing-invariant RFC; otherwise "n/a — <reason>".

---

## Decision: RFC (+ Ticket) vs. Ticket-only

Use this rule of thumb before starting work:

| Situation | Artifact(s) | Why |
|-----------|-------------|-----|
| New feature, new module, architectural change | **RFC (+ Ticket)** | Decisions need to be documented and reviewable |
| Multi-step implementation (>1 day, multiple commits) | **RFC (+ sub-Tickets)** | One RFC, several implementation steps |
| Cross-cutting refactoring | **RFC (+ Ticket)** | Affects many files, design rationale matters |
| New API endpoint with non-trivial contract | **RFC (+ Ticket)** | Contract worth documenting before coding |
| DB migration with data shape change | **RFC (+ Ticket)** | Migration plan must be reviewable |
| Bug-fix (clear scope, < few hours) | **Ticket only** | Title + description is enough |
| UI polish, copy change, small CSS tweak | **Ticket only** | Not worth a spec document |
| Doc-only update | **No artifact** | Just commit |
| Hotfix during incident | **Ticket only** (RFC retrofitted later if needed) | Speed first |

When in doubt: **prefer RFC**. It's cheap to write a 30-line RFC and expensive to reverse-engineer the
design intent later from commit history.

## RFC Files

- Directory: `docs/rfcs/`
- Index: `docs/rfcs/README.md`
- Template: `.claude/skills/rfcs/TEMPLATE.md` (ships with the skill, not with the docs)
- Filename format: `NNN-kebab-case-title.md` (3-digit number, hyphenated title)
- Numbers are sequential (find highest existing + 1)

## Status Values

| Status | Meaning | Linked ticket state (if `[TICKET_SYSTEM]`) |
|---|---|---|
| Draft | Initial draft, not yet approved | Open + `draft` |
| Accepted | Approved, ready for implementation | Open + `accepted` |
| In Progress | Actively being worked on | Open + `in-progress` |
| In Review | Implemented, awaiting review (e.g. open branch) | Open + `in-progress` |
| Done | Completed | **Closed** |
| Rejected | Rejected / not implemented | **Closed** |

---

## Commands

### `/rfcs` or `/rfcs list`
Show all RFCs from `docs/rfcs/README.md` with status, author, and (if any) linked ticket.

### `/rfcs status`
Show only active items (Draft / Accepted / In Progress / In Review) with their dependencies. Useful at
the start of a session to find what's pending.

### `/rfcs new "<title>"`
Create a new RFC (+ linked ticket if `[TICKET_SYSTEM]`) **via the interactive Intake-Flow below** —
never write an empty file. It runs the intake, then generates from `.claude/skills/rfcs/TEMPLATE.md`,
then adds
the README row and (optionally) creates the ticket.

### `/rfcs ticket "<title>"`  *(only if `[TICKET_SYSTEM]`)*
Create a **ticket-only** item (no RFC, no spec file). Use for small, self-contained changes — bugs,
polish, copy fixes. The ticket is the only artifact; reference it in commit messages.

### `/rfcs subticket NNN "<title>"`  *(only if `[TICKET_SYSTEM]`)*
Create a **sub-ticket** under an existing RFC NNN, add it to the RFC's Implementation Checklist with a
link. The parent RFC stays open until ALL sub-tickets are closed.

### `/rfcs update NNN <status>`
Update RFC NNN status. Touches:
1. **RFC file header** (`Status` field)
2. **`docs/rfcs/README.md`** (status column)
3. **the ticket** (relabel or close) — only if `[TICKET_SYSTEM]`

Gate handling on status transitions is described in "Gates on status change" below.

### `/rfcs close NNN`
Convenience for `/rfcs update NNN Done`. Same closeout trigger. Also closes any open sub-tickets under
the RFC (if `[TICKET_SYSTEM]`) after confirming with the user.

---

## Interactive intake (`/rfcs new`)

When creating an RFC, **do not** immediately write an empty file — collect the fields like a form.

**Step 1 — base choices via `AskUserQuestion`** (ONE call, rendered as clickable options):
- **RFC-Type**: `Bug-Fix` · `Feature` · `Refactor` · `Migration` (the "Other" option catches
  `Cleanup`/`Infra-Incident`)
- **Complexity**: `Standard` · `High` (High ⇒ research/design first, then code — §6)
- **Priority**: `Critical` · `High` · `Medium` · `Low`
- **Technical/domain approval needed?**: `Yes — Refactor/Migration/Privacy/Money/Cross-Tenant/invariant`
  · `No → n/a`

(`Status` defaults to `Draft`; only ask separately if needed.)

When **RFC-Type = Refactor**, warn explicitly: *"Atomic Deploy is mandatory — separate backend/frontend
deploys are forbidden. If the RFC gets too big: scope-cut, don't half-implement"* (see
`docs/rfc-planning-discipline.md` §3).

**Step 1b — type-adaptive (ONLY when RFC-Type = Bug-Fix):** a second `AskUserQuestion` for the bug
details:
- **Severity/impact**: `S1 outage/data-loss` · `S2 core function broken` · `S3 degraded` · `S4 cosmetic`
- **Reproducible**: `always` · `sometimes` · `observed once`
- **Environment**: `Prod` · `Dev` · `both`

(Repro steps / expected-vs-actual / evidence follow as text in Step 2.)

**Step 2 — content (guided, not an empty form), in three sub-steps:**

- **2a — ask for the description:** "**What is it about?**" → derive **Summary** + **Motivation /
  Problem**; for a feature the **Target State**, for a bug the **Current State / root cause** (with
  `file:line` / log evidence). Plus the **Done-Definition** (mandatory) + optional **Acceptance
  Criteria** (Given-When-Then). For a bug also the **Bug Details** text (repro / expected-vs-actual /
  evidence).
- **2b — analyze the affected modules YOURSELF** (don't just ask the user): from the description + the
  codebase (grep/Read), **derive** the affected modules/files and **propose** them as **`Affects`** +
  optionally Architecture-Impact; have the user confirm/correct.
- **2c — check the work-package split:** for larger/multi-part efforts, **ask whether a split into
  Work Packages (P1, P2, …)** is wanted — each package a cuttable, independently deployable step
  (section `## Work Packages`). Small/isolated ⇒ no split, note it briefly.

Sections marked *(when needed)* are only filled when the type/topic demands it (Refactor →
Architecture-Impact; Privacy/Money → Security; etc.).

**Complexity = High → Context-Isolation (`docs/rfc-planning-discipline.md` §6):** **before** writing
the design, run a research step in a subagent with a clean context — brief it WITHOUT solution framing:
*"Understand subsystem X for RFC-<title>. List facts, constraints, invariants, options — do NOT propose
a solution."* Use `Explore` or a fresh `general-purpose` subagent (not the main session, which already
carries the ticket framing). The result feeds `## Proposed Solution`. On **Standard**: skip.

**Step 3 — generate:** copy `.claude/skills/rfcs/TEMPLATE.md` → `docs/rfcs/NNN-kebab-case-title.md`
(next number =
highest + 1), replace `RFC-NNN`, set `Status: Draft`, fill the header fields with the user's choices +
metadata, fill the Done-Definition and the `Reference Documentation (truth spec)` field, add a row to
`docs/rfcs/README.md`, then show the finished RFC for approval before committing.

**Short mode:** if the user says "no questions" or already provides everything in the prompt → skip the
intake and generate directly.

**`[TICKET_SYSTEM]` ticket creation (optional):** if the project uses a ticket system, after
generating the file also create the linked ticket and record its id in the RFC header + README row.
For a GitHub-based `[TICKET_SYSTEM]` this is, for example:
```bash
gh issue create --repo [REPO_SLUG] \
  --title "RFC-NNN: <title>" \
  --label rfc,draft \
  --body "Specification: https://github.com/[REPO_SLUG]/blob/[DEFAULT_BRANCH]/docs/rfcs/NNN-...md"
```
Then set the header row `| [TICKET_SYSTEM] Issue | [REPO_SLUG]#NNN |`. Adapt the command to the
actual ticket tool; if `[TICKET_SYSTEM]` is "none", omit this entirely.

---

## Gates on status change (`/rfcs update`)

The planning discipline is a closed loop (gate before code · check after). Enforce it on transitions:

**→ In Progress — Design-Gate** (`docs/rfc-planning-discipline.md` §5, "Brain Surgery Moment"):
- Read the RFC's `## Design Approval` section.
- For `Refactor` / `Migration` / RFCs touching Privacy/Money/Cross-Tenant/a load-bearing invariant, the
  approval checkbox MUST be ticked (design approved, 0 code) before status goes "In Progress".
- If the gate is NOT set → **warn + ask** before proceeding: "Design gate for RFC-NNN not set — approve
  the design first, or deliberately skip with a reason?" Don't silently flip to In Progress.
- Skip-out: an explicit user override documented as `[design-gate skip: <reason>]` in the RFC/commit.
- For sensitive RFCs, the *active* form of the gate is an `architecture-reviewer` subagent run (design
  viable before code? baseline fit? invariants held?) if the project wires that skill up; otherwise an
  ad-hoc `Explore`/`general-purpose` subagent check.

**→ Done — Closeout Check** (`docs/rfc-planning-discipline.md` §4, run FIRST):
- Read the RFC's `RFC-Type` and `Done-Definition`. Is it met in prod? For a refactor: is the original
  bug-type now structurally impossible? For sensitive RFCs, run the check in a context-isolated
  subagent (`architecture-reviewer` if available, else `Explore`/`general-purpose`).
- **Truth↔Delta check:** read the `Reference Documentation (truth spec)` header. Confirm each named
  baseline doc now **reflects the shipped state** — the delta must be merged back into the current-truth
  doc, not left only in the RFC. If a truth-spec still describes the old state → **unmet criterion**.
- ⚠️ **"in prod" depends on `[DEPLOY_MODEL]`:** with **CI/CD**, Done follows the pipeline deploy after
  merge; with a **manual ops deploy**, the push triggers nothing — a merged RFC stays **"In Review"**
  until the separate manual prod deploy is verified, only then **"Done"**.
- If a criterion is unmet → **refuse the status change**; recommend either (a) keep "In Progress" + add
  the gaps to the RFC's Implementation Checklist (NOT a new follow-up ticket), or (b) an explicit user
  override `[closeout-review override: <reason>]` in the closing commit.

---

## After writing — the RFC is the cut

Once the RFC stands and its status is set, **the document** carries the state, not the conversation.
That makes the phase boundary a *lossless* context cut:

- **After `new` and after approval → `/clear`.** The next phase starts from the RFC as the entry point,
  not the full research history. Working on without cutting drags the analysis phase through design and
  implementation — and pays for it in *every* further step (the API is stateless, the history rides
  along).
- **Within a phase** — if writing the RFC itself gets long: `/compact`, not `/clear`. `/compact`
  condenses, `/clear` discards. Discarding is only allowed once the result is written.
- **Don't cut earlier.** As long as something lives only in the conversation, the reset is lossy. Only
  the artifact makes it lossless — that is the underrated point of the RFC requirement: it is the
  **permission to throw away your own context**.
- **Exception:** if a review subagent should run right after approval, review first, then cut — the
  review reads the RFC, not the history, but its result still belongs to the same round.

---

## Rules

- **New RFCs start from `.claude/skills/rfcs/TEMPLATE.md`** (single source — update it in place when
  conventions
  change). Never hand-roll a header.
- **Every RFC needs a filled `Done-Definition`** before it leaves Draft.
- **Always update BOTH** the RFC file header AND `docs/rfcs/README.md` when changing status (+ the
  ticket, if `[TICKET_SYSTEM]`).
- **Sequential RFC numbers** — find the highest existing in `docs/rfcs/` + 1.
- **Read active RFCs first** when planning new work to check for dependencies and avoid duplicate
  effort.
- **Check the dependency chain** before marking an RFC as Done.
- **Cut after approval** — `/clear` before the next phase, once the RFC carries the state (see "the RFC
  is the cut").
- **Hotfixes:** ticket-only is fine during an incident; if the change has lasting design implications,
  retrofit an RFC after the dust settles.
- **`[TICKET_SYSTEM]` (if used):** an item is either `rfc` or `ticket`, never both; sub-tickets close
  before the parent RFC closes; close the ticket when the RFC is Done or Rejected.
