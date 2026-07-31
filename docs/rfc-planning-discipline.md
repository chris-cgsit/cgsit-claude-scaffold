# RFC Planning Discipline

> **Anchor:** this doc is the detailed spec behind the short reference in
> [`CLAUDE.md` → "RFC Planning Discipline"](../CLAUDE.md). The *mechanics* (ask the mandatory
> fields, check the gate, trigger reviews) are operationalized in the `rfcs` skill +
> `docs/rfcs/TEMPLATE.md` — this doc explains the *why* and the rules in detail.
> **Read before drafting OR splitting any RFC.**

**Origin (a generic war story).** These rules exist because of a recurring failure mode: a
structural refactor was planned, then broken into "MVP now + cleanup follow-up". The half-built
architecture that remained produced — days later — exactly the bug the refactor was meant to prevent.
It was not a code bug; it was a **planning-process bug**. The half-architecture carried the risks of
both the old and the new design at once. The rules below prevent the repeat.

---

## 1. RFC-Type as a mandatory field

Every RFC gets a header field **`RFC-Type`** (Bug-Fix · Feature · Refactor · Migration · Cleanup ·
Infra/Incident). The classification decides whether phase-splitting is OK:

| Type | What it is | Phase-split deploys |
|---|---|---|
| **Feature** | new user-visible function | ✅ each phase delivers user value in isolation |
| **Bug-Fix** | eliminate a symptom | ✅ usually isolated (one fix, one deploy) |
| **Refactor** | structural property / bug-type elimination | ❌ **Atomic Deploy mandatory** (see §3) |
| **Migration** | data/API change with BC guarantee | ⚠ phases allowed with a rollback plan + documented intermediate states |
| **Cleanup** | dead code, docs, tests | ✅ trivial |
| **Infra/Incident** | infrastructure / outage | case by case (incident = speed first, RFC retrofitted if needed) |

## 2. Done-Definition as a mandatory field

Done is **not** "checklist ticked". Done is **"the property/function the RFC promised exists in
prod"**.

| Type | Done-Definition example |
|---|---|
| Feature | "user can do X" + an E2E test that verifies X |
| Bug-Fix | "symptom X no longer occurs" + a test/repro that actively excludes X |
| Refactor | "bug-type Y is structurally impossible" + a test that actively excludes the bug-type |
| Migration | "old state gone, new state has 100% coverage" + a query that proves it |
| Cleanup | "grep shows 0 hits on the deleted symbol" |

In the template, `## Done-Definition` is mandatory. If the author cannot fill it clearly, the RFC is
not spec-ready.

## 3. Refactor RFCs: Atomic-Deploy mandate

For `RFC-Type: Refactor` the following patterns are **forbidden**:
- "Backend phase first, frontend phase later" (separate deploys)
- "MVP shipped, cleanup follow-up" — if the cleanup is what creates the architectural value, it is
  not a cleanup, it is the main work
- "Phase 1 deployed, Phase 2 as another ticket"

**Rationale:** for architecture refactors the half-architecture is **strictly worse** than the old
complete one — it carries the risks of both worlds at once (backend split + frontend not split = the
bug-type still lives in the mediation layer).

**Exception:** Migration RFCs with an explicitly documented expand/contract pattern + BC guarantee
between the phases. This must be justified in the RFC.

## 4. Closeout Check (before Status "Done")

Before an RFC status goes to **Done**:

1. **Is the `Done-Definition` met in prod?** For a refactor, concretely: is the original bug-type now
   structurally impossible? For larger/sensitive RFCs, a **context-isolated check** is worth it (a
   fresh `Explore`/`general-purpose` subagent: "Read RFC-NNN + the codebase state — is the
   Done-Definition met?"). If the project wires up a dedicated `architecture-reviewer` skill, use its
   closeout phase-template instead.
2. **Truth↔Delta check:** read the `Reference Documentation (truth spec)` header field — do the named
   baseline docs (`docs/architecture.md`, `docs/*.md`) now reflect the shipped state? The delta must be
   merged back into the current-truth docs, not left only in the RFC. If a truth-spec still describes
   the old state → **not Done yet**.
3. **On YES** → Status Done (RFC header **and** `README.md`).
   ⚠️ **What "in prod" means depends on `[DEPLOY_MODEL]`:**
   - **CI/CD:** merge triggers the pipeline; Done follows the successful deploy.
   - **Manual ops deploy:** the push/merge triggers **nothing** — the prod deploy is a **separate,
     manual step**. A merged/pushed RFC is **not yet live**: it stays **"In Review"** until the prod
     deploy is done and verified, only then **"Done"**. Never flip to Done just because the code merged.
4. **On NO** → Status stays "In Progress" and the open points are named **inside the RFC** — **not**
   moved into a new "follow-up" ticket (otherwise the RFC lives on invisibly with open items — exactly
   the half-refactor failure).

**Recommended for:** all `Refactor`/`Migration` RFCs + all RFCs touching customer-data/privacy, money,
or a **load-bearing invariant** of the system.

## 5. Design Gate — "Brain Surgery Moment" before code

**Symmetric to §4:** the closeout check (§4) verifies *after* the work "is the property in prod?". The
design gate verifies **before** the code "is the design approved before a single line is written?".
Together they form a closed loop (gate before · check after).

**Why:** the most expensive thinking error is the one you only notice as a half-built architecture. At
the "brain surgery moment" — design done, **0 code** — the correction is almost free; afterwards it
gets expensive.

**Flow:**
1. The RFC has the section `## Design Approval` (checkboxes **technical** / **domain** + date).
2. **Before** implementation code begins, the required approval is set ("technically/domain approved —
   code allowed from here").
3. The status change to "In Progress" checks whether the approval is set; if not → warn + ask (no
   silent code start).

**Mandatory for:** the same triggers as §4 — all `Refactor`/`Migration` RFCs + customer-data/privacy,
money, a **load-bearing invariant**. For `Feature`/`Bug-Fix`/`Cleanup` without those triggers:
recommended, not mandatory (then a short "n/a — <reason>").

**Skip-out:** starting deliberately without the gate (e.g. a trivial feature) → justify briefly in the
RFC/commit.

## 6. Context-Isolation for complex RFCs (`Complexity: High`)

**Why:** the most expensive mistake is building the *obvious* solution before the system is understood
("completion bleed"). The countermeasure: **isolate the analysis/research phase from the solution
framing.**

**Flow** (header field `Complexity: Standard | High`):
- On **`High`** (large/unclear/cross-module RFCs): **before** the design is written, run a **research
  step in a subagent with a clean context** — brief it deliberately WITHOUT "here is how to solve it":
  *"Understand subsystem X. List facts, constraints, invariants, options — do not propose a solution."*
  Use `Explore` or a fresh `general-purpose` subagent (not the main session, which already carries the
  ticket framing). The output = a research note that feeds `## Proposed Solution`.
- On **`Standard`**: no extra step (avoid overhead).

## Anti-Patterns

- ❌ "MVP shipped + cleanup follow-up" — if the cleanup makes the value, it is not a cleanup
- ❌ "Backend is done, frontend comes later" — half-architecture
- ❌ "Status = Done because the checklist is 100%" — the checklist can be complete while the
  architectural promise stays unmet
- ❌ "We had X hours, so package Y had to be cut" — when time is short: **shrink the RFC** (scope-cut +
  status back to "Draft"), do NOT half-implement

## When you are about to split an RFC

Stop. Ask yourself:
1. Which `RFC-Type`? Refactor → splitting is suspicious (§3).
2. What is the Done-Definition? If it says "property X exists in prod" and phase 1 alone does NOT
   deliver that → the phase is not an MVP, it is half-work.
3. If you still must split: **scope-cut the RFC** instead of deploying only parts. A small RFC that is
   FULLY done beats a large RFC that is HALF live.

---

## Implementation Discipline (read before writing code)

**Cleaner, more conscientious work — split before scale.** When a task is larger than ~1–2 well-scoped
files of changes, STOP and plan before coding. Symptoms that you are already too big:

- You are modifying 5+ files and haven't sketched the data flow yet
- You are touching cross-module API surfaces without a plan
- You catch yourself thinking "I'll just fix this related thing while I'm here"
- The next test you'd write needs >20 lines of setup
- The change introduces a new dependency between modules

**When that happens, stop and:**

1. **Architecture check** — does this change touch a load-bearing invariant? If yes, read the relevant
   `docs/architecture.md` / `docs/*.md` BEFORE touching the code, and confirm the change doesn't
   violate the invariants documented there.
2. **Decompose** — split into 2–3 packages, each independently deployable + reversible. (Section
   `## Work Packages`, P1/P2 …)
3. **Plan the workflow** — per package: which files change, which tests prove it in isolation, what
   depends on it, is it deployable on its own?
4. **Write into the RFC** — the decomposition belongs in `## Work Packages` (numbered, each
   independently deployable/verifiable).
5. **Ship the smallest stable package first** — green, deployed, smoke-tested. THEN start the next.
   Don't pipeline.

**Anti-patterns to refuse:**
- "Bundle these 3 fixes since they touch the same area" — bundle only when splitting would be more
  work than coupling
- "Quick fix without an RFC because it's just X" — if X touches more than 2 modules, it's an RFC
- "Skip tests for the cleanup script, it's one-shot" — one-shot scripts that touch production data
  need an idempotency assertion or a dry-run mode
- "Deploy now and write the doc later" — the doc IS the alignment; without it the next deploy hits the
  same unwritten assumptions

**When something IS small enough** (1–2 files, clear scope, no cross-module impact): do the work and
commit. The discipline here is about catching when you've crossed the line — not turning every
typo-fix into a multi-week project.
