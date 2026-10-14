# Stride Skill Ideas: PR Review, Spec, and Post-Merge Archival

**Date:** 2026-03-16
**Status:** Brainstorm — not yet spec'd

---

## Three Skills Identified

### 1. `/pr-review` — Spec-Aligned Code Review (Highest Priority)

Review PRs against multiple dimensions, with the spec as the primary rubric when present.

**Core flow:**
- User invokes `/pr-review <PR-url-or-number>`
- Skill looks for a `spec/*.md` file in the PR's changed files
- If spec exists → use it as the review rubric
- If no spec and PR is non-trivial → ask for the spec file path
- Small PRs (bug fixes, dep bumps, typos) skip the spec requirement

**Review dimensions:**
- **Spec alignment** — does implementation match what was agreed?
- **Code quality** — best practices, readability, naming, structure
- **Bug detection** — logic errors, edge cases, race conditions, security (OWASP top 10)
- **Test coverage** — happy path + edge cases, untested code paths
- **Consistency** — follows existing repo patterns

**Severity via GitHub tags:** blocker / warning / nit — leveraging GitHub's label/tag system so authors know what must be fixed vs. nice-to-have.

**Context search priority order:**
1. Repo's existing patterns (highest priority)
2. NECP / Netflix internal docs for best practices
3. Sourcegraph deep search for similar usage across other Netflix repos

**Output:** HTML report. General-purpose — works on any Netflix GHES repo.

### 2. `/spec` — Spec Creation and Iteration (Medium Priority)

Create and iterate on feature spec documents during the design phase.

**Key decisions:**
- Specs live in `specs/feature-name.md` in the repo during development
- Current version only in the file — rely on `git log -- specs/feature-name.md` for iteration history (no duplication)
- Spec is generated collaboratively between Claude Code and the engineer

### 3. Post-Merge Cleanup — Archive to Google Docs (Lower Priority)

After PR merges, archive the spec and clean up the repo.

**Flow:**
- Export `specs/feature-name.md` to a Google Doc for long-term reference
- Remove the spec file from the repo
- Repo stays clean; Google Docs becomes the decision archive

**Open question:** Should this be a standalone skill or a post-merge hook triggered automatically?

---

## Priority Order

1. **`/pr-review`** — highest value, can't easily replicate manually
2. **`/spec`** — useful but engineers can write specs manually for now
3. **Post-merge archival** — nice-to-have, manual archival works as interim
