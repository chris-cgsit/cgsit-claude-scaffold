# RFC-NNN: <Title>

<!-- Copy this file to NNN-kebab-case-title.md (NNN = highest existing number + 1).
     Small Bug-Fix / Cleanup RFCs MAY drop the sections marked *(when needed)* — mandatory are only:
     header, Done-Definition, Summary, Current State, Target State, Proposed Solution, Test Strategy.
     The WHY / rules (Atomic-Deploy, Design-Gate, Context-Isolation, Split-Discipline):
     docs/rfc-planning-discipline.md -->

| Field       | Value                                                                                          |
|-------------|------------------------------------------------------------------------------------------------|
| Status      | Draft   *(Draft · In Progress · In Review · Done · Rejected · Superseded · Deferred)*           |
| RFC-Type    | *(one: Bug-Fix · Feature · Refactor · Migration · Cleanup · Infra/Incident — see `docs/rfc-planning-discipline.md` §1)* |
| Complexity  | Standard \| High   *(High ⇒ research/design first, then code — see "Design Approval" + §6)*     |
| Priority    | Critical \| High \| Medium \| Low   *(urgency — how soon must this ship)*                       |
| Author      |                                                                                                |
| Created     | YYYY-MM-DD                                                                                      |
| Affects     | *(affected modules/systems, comma-separated)*                                                   |
| Branch      | *(e.g. `feature/rfc-NNN-short-description`)*                                                     |
| Reference Documentation (truth spec) | *(standing baseline docs this RFC advances as a delta: `docs/architecture.md`, `docs/product_overview_requirements.md`, `docs/*.md` — or "n/a")* |
| [TICKET_SYSTEM] Issue | *(optional — `[REPO_SLUG]#NNN`; omit the row if the project has no ticket system)*  |

## Done-Definition
<!-- MANDATORY — see `docs/rfc-planning-discipline.md` §2.
     Done is NOT "checklist ticked". Done is "the property/function this RFC promised exists in prod".
     - Bug-Fix:   "symptom X no longer occurs" + a test/repro that actively excludes X.
     - Feature:   "user can do X" + E2E/manual acceptance verifies X.
     - Refactor:  "bug-type Y is structurally impossible" + a test that actively excludes Y.
     - Migration: "old state gone, new state has 100% coverage" + a proving query.
     - Cleanup:   "grep shows 0 hits on the deleted symbol".
     ⚠️ "in prod" depends on [DEPLOY_MODEL]: with CI/CD, Done follows the pipeline deploy; with a manual
        ops deploy, the RFC stays "In Review" after merge until the manual prod deploy is verified
        (see `docs/rfc-planning-discipline.md` §4).
     If you cannot fill this clearly, the RFC is not spec-ready yet. -->

## Acceptance Criteria  *(when needed — recommended for Bug-Fix & Feature)*
<!-- Testable Given-When-Then scenarios (Gherkin/BDD). E2E/tests derive directly from these.
     - Scenario "<name>": **Given** <state> · **When** <action> · **Then** <expected result>.
     Multiple scenarios incl. error/edge cases. -->

## Summary
<!-- 2–4 sentences: what this is about, what the result is. -->

## Motivation / Problem
<!-- Why do we need this? For a bug: symptom + impact (who/how often affected). -->

## Bug Details  *(only RFC-Type = Bug-Fix)*
<!-- Standard bug-report fields (issue-form style): -->
- **Severity / impact:** S1 outage/data-loss · S2 core function broken · S3 degraded · S4 cosmetic
- **Reproducible:** always · sometimes · observed once
- **Repro steps:** 1. … 2. … 3. …
- **Expected vs. actual:** *expected* … / *actual* …
- **Environment / version:** Prod \| Dev · deploy/commit · browser/client
- **Evidence:** log reference (log group + time window), screenshot, affected records/IDs

## Current State
<!-- How is it today? For a bug: root cause with `file:line` evidence; for prod bugs, log evidence
     (log group + time window). Facts, not guesses. -->

## Target State
<!-- How should it be once this RFC is implemented? -->

## Proposed Solution
<!-- High-level approach + Detailed Design where useful (data model, API contract, flows, algorithm).
     - Refactor/Migration: **Atomic Deploy** — no phase split ("backend first, frontend later").
       Too big ⇒ scope-cut the RFC, do not half-implement (see `docs/rfc-planning-discipline.md` §3).
     - Ported code: keep an origin comment (`// ported from <source>: …`). -->

## Work Packages  *(when needed — for multi-part / larger efforts)*
<!-- One cuttable, independently deployable step per package. Core package before secondary ones.
     Small/isolated ⇒ drop this section (then "Proposed Solution" + Implementation Checklist suffice). -->
- **P1 — <title>:** <result/scope> · *(deployable: yes/no · reversible: yes/no)* · Status: open
- **P2 — <title>:** … · Status: open

## Architecture-Impact  *(when needed)*
<!-- Affected layers/modules from `docs/architecture.md`; new packages; DB changes (migration);
     API changes. ⚠️ Does the RFC touch a load-bearing invariant of the system?
     → keep it **additive** and confirm the change does not violate the documented invariant. -->
- **Affected layers**: [e.g. API, Service, Persistence]
- **New packages/modules**: [if any]
- **Database changes**: [new tables, columns, migrations]
- **API changes**: [new/modified endpoints]

## Security / Privacy  *(when needed)*
<!-- Authentication/roles, input validation, PII/customer data, cross-tenant/multi-tenant. -->

## Performance  *(when needed)*
<!-- Expected load, caching, query/IO optimization, DB impact. -->

## Alternatives Considered  *(when needed)*
<!-- Which other approaches were evaluated? Why rejected? -->

## Test Strategy
<!-- What specifically needs to be tested? Unit / Integration / E2E / edge cases / manual acceptance.
     ⚠️ Gate: run the full test/verify command (`[TEST_COMMAND]`) green before every commit. -->
- **Unit tests**: [key scenarios]
- **Integration tests**: [what to verify end-to-end]
- **Edge cases**: [specific edge cases to cover]

## Dependencies
| RFC | Description | Must be done first |
|-----|-------------|--------------------|
|     |             |                    |

## Design Approval
<!-- Question: does this effort need a technical or domain sign-off BEFORE code begins?
     MANDATORY for: Refactor · Migration · and always for Privacy/Money/Cross-Tenant or a load-bearing
     invariant — see `docs/rfc-planning-discipline.md` §5 ("Brain Surgery Moment").
     Without such a trigger (e.g. a small Bug-Fix): "n/a — <short reason>".
     `/rfcs update NNN "In Progress"` checks this gate. -->
- [ ] **Technically approved** (architecture/design OK — code allowed from here). (Date: YYYY-MM-DD)
- [ ] **Domain approved** (business/product OK — if domain-relevant). (Date: YYYY-MM-DD)

## Open Questions
<!-- Unresolved decisions or unknowns. -->

## Implementation Checklist  *(maintained during implementation)*
<!-- Break the work into deployable Work Packages (P1, P2, …), NOT a flat step list — see
     `docs/rfc-planning-discipline.md` "Implementation Discipline". Each package: independently
     deployable + reversible; ship the smallest stable one first, then the next (do not pipeline).
     Refactor/Migration = Atomic Deploy: packages structure the work but ship together. -->
- [ ] <step 1>
- [ ] Full test/verify command (`[TEST_COMMAND]`) green
- [ ] Manual acceptance / E2E
- [ ] Pushed → Status **"In Review"** + README row updated
- [ ] **Deployed to prod ([DEPLOY_MODEL]) + verified → Status "Done"** *(see `docs/rfc-planning-discipline.md` §4)*
- [ ] **Closeout review**: Done-Definition met in prod? Truth-spec docs reflect the shipped state?
