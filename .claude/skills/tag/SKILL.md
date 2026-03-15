---
name: tag
description: Create annotated git tags with a structured release summary. Use when the user asks to tag a version or create a release.
disable-model-invocation: true
user-invocable: true
allowed-tools: Bash(git *)
argument-hint: <version> [message]
---

# Git Tag with Release Summary

Create an annotated git tag with a structured summary of all changes since the last tag (or all commits if no previous tag exists).

## Rules

- **NEVER** add "Co-Authored-By" or AI attribution lines
- Tag format: `v<semver>` (e.g., `v0.0.1`, `v0.1.0`, `v1.0.0`)
- Summary is in German (project language), section headings in German
- Use `git tag -a` with `-m` for annotated tags
- Always push the tag after creation: `git push origin <tag>`

## Steps

1. Determine version from argument (e.g., `0.1.0` → `v0.1.0`)
2. Find the previous tag: `git describe --tags --abbrev=0 HEAD~ 2>/dev/null`
3. Collect commits since last tag (or all commits if first tag):
   - With previous tag: `git log <prev-tag>..HEAD --oneline`
   - First tag: `git log --oneline --reverse`
4. Group commits into categories and summarize
5. Build the tag message using the template below
6. If the tag already exists, delete it first (`git tag -d` + `git push origin :refs/tags/`)
7. Create the tag: `git tag -a <version> -m "<message>"`
8. Push: `git push origin <version>`

## Tag Message Template

```
<version> — <kurze Beschreibung>

Features:
- <feature 1>
- <feature 2>
- ...

Infrastruktur:
- <infra change 1>
- ...

Dokumentation:
- <doc 1>
- ...

Fixes:
- <fix 1>
- ...

Commits: <anzahl> (<first-hash>..<last-hash>)
```

### Category Rules

| Commit-Prefix | Kategorie |
|---------------|-----------|
| `feat:` | Features |
| `build:`, `chore:` | Infrastruktur |
| `docs:` | Dokumentation |
| `fix:` | Fixes |
| `refactor:` | Features (wenn funktional relevant) oder Infrastruktur |
| `test:` | Infrastruktur |

- Fasse zusammengehoerige Commits zu einem Punkt zusammen (z.B. RFC + Implementierung = ein Feature-Punkt)
- Jeder Punkt beschreibt das Ergebnis, nicht die einzelnen Commits
- Nenne konkrete Zahlen wo sinnvoll (Anzahl Tests, Endpoints, Entities)
- Leere Kategorien weglassen

## Examples

Good tag message:
```
v0.1.0 — Portfolio Access Control und Testing

Features:
- Portfolio-Zugriffskontrolle mit Provider/Subscriber-Modell (RFC-005)
- Stop-Loss Marks und Portfolio-Events (RFC-006)
- Notes und Q&A System fuer Portfolios (RFC-007)

Infrastruktur:
- 75 JUnit 5 Integration Tests (38 JPA + 37 REST API)
- PostgreSQL 18.3 Upgrade

Commits: 12 (d419055..a9d02f0)
```

Bad tag message:
```
v0.1.0

- did some stuff
- updated files
```
