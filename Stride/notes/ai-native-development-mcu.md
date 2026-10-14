---
title: Enabling AI Native Development in MCU
date: 2026-03-13
tags: [ai, agentic-development, mcu, sdlc, netflix]
source: https://docs.google.com/document/d/1xETUwHO3ViFI_IyZtIBnSIbUDlwK6iiBbGPDi8GRxDA/edit
author: Rachel Salama
status: Approved
---

# Enabling AI Native Development in MCU

**Author**: Rachel Salama
**Last Updated**: March 11, 2026
**Status**: Approved
**Superseded By**: CBPE AI TechProd Best Practices

## TL;DR

A proposal to make MCU codebases ready for AI-native/agentic development by cleaning up technical debt, improving documentation, and embedding agents into the SDLC. Core thesis: **human judgment is now the bottleneck**, not code generation.

**Objective**: CBP KR 1.1 — Achieve 80% WAU of coding agents (Cursor, Claude) among engineers.

---

## 4-Phase Plan

| Phase | Duration | Goal |
|---|---|---|
| **0 — Assessment & Planning** | 1 week | Audit repos, score readiness, assign owners |
| **1 — Pruning** | 2 days | Delete deprecated code, consolidate duplicate APIs |
| **2 — Modernization** | 2 weeks | Docs, 90% test coverage, decouple services, add migrations |
| **3 — Agent Integration** | 1 week (parallel w/ Phase 2) | CI/CD automation, hooks, standardized toolchain |
| **4 — Pilot & Rollout** | 1 week | Validate, update onboarding, scale team-wide |

---

## Phase 0: Assessment

**Codebase Audit Focus Areas:**
- Dead artifacts (deprecated tables, classes, endpoints)
- Duplication (redundant API endpoints)
- Documentation quality (GraphQL, REST, Javadocs, ADRs, table comments)
- Test coverage
- Modularity & coupling
- Tooling & CI/CD pipeline gaps

**Agent Readiness Scorecard Metrics:**
1. Documentation Index Score (% of external APIs with consumer-facing docs)
2. Test coverage (target: 90%+)
3. Code path complexity score (goal: 1 clear path per CRUD operation)
4. Manual deployment steps (goal: 0 manual steps besides Spinnaker judgement)

---

## Phase 1: Pruning

- Remove deprecated tables/classes/endpoints with no active consumers
- If deletion is risky: mark with `_DEPRECATED_YYYYMMDD` suffix + `@Deprecated` annotations
- Consolidate duplicate REST endpoints to single canonical paths per CRUD operation
- Use Sourcegraph + logging to identify actual consumers before deprecating

---

## Phase 2: Modernization

### Documentation
- Javadocs / JSDocs for all public functions
- GraphQL schema documentation (queries, mutations, enums, objects)
- Swagger/OpenAPI for all REST endpoints
- `CLAUDE.md` files updated with core data structures and code paths
- ADRs for decision tracking

### Testing
- 90% unit test coverage for core business logic
- Integration tests for all primary service interfaces
- Audit and fix ineffectual tests ("testing the mocks")
- Integration tests for Cinder feed generation and diffing

### Automation
- Automated linting with standardized config
- Pre-commit/push validation hooks (SAZ policy, Gradle builds, GraphQL schema)
- DB migrations via Flyway/Liquibase (replacing raw SQL execution)
- Standardize on Jooq for SQL (vs. raw string injection)
- Development workflows as skills (e.g., scaffold new GQL endpoint with SAZ + docs)

---

## Phase 3: Agent Integration & Tooling

- Standardize on Cursor / Claude Code / IntelliJ+Claude across team
- Hooks via Husky or Claude hooks for automated dev steps
- CI/CD updates: automated lint, code review (Bitbot/Graphite), unit + e2e tests
- Automated DB migrations on merge
- Automated PR iteration based on review comments

---

## Phase 4: Pilot & Rollout

**Success Metrics:**
- 90% of feature development completed via agent-supervised workflow
- <5% increase in support inquiries or error logs

**Rollout:**
- Update team onboarding runbook
- Migrate remaining codebases based on Phase 0 urgency scores

---

## Key Debates / Open Questions

- Is 2-day pruning realistic? (Some say weeks/months for API consumer migrations)
- Javadocs vs. letting agents read code directly — some prefer spec-driven development
- Standardize on Jooq for SQL? (Rachel: yes, align test container versions first)
- Greenfield rewrite vs. modernizing existing codebases? (Revenue Data Eng took greenfield path)
- Should on-call triage be a dedicated phase?
- Where should docs live — repo `.md` files, Coda, go/manuals?

---

## Key Assumptions

1. Engineering is shifting from *humans writing code with AI assistance* → *humans directing agents*
2. AI-native development in greenfield codebases produces impressive results (not just hype)
3. Most feature work is in non-greenfield codebases — we can't start over from scratch
