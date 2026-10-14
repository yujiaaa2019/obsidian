---
title: EDPR Directional Pillars 2026+
date: 2026-03-13
tags: [edpr, strategy, netflix, platform]
source: https://docs.google.com/document/d/1PzzZpnrgb-cqO-AIfcz9F6p-OKzB4bcP860SPsT3sUw/edit
authors: Eduardo Ramirez, Drew Koszewnik
status: Draft
---

## TL;DR

EDPR is entering its next phase following general availability in January 2026. The strategy pivots from a strict all-or-nothing managed runtime to a **gradient adoption model** organized around three pillars: a consistent application surface area of shared primitives, a managed application model that allows proportional deviation, and an agentic coding harness. No existing applications will be required to migrate — teams adopt when natural opportunities arise, and value is proportional to adoption.

---

## What's Changing in 2026

**Key shifts from the previous phase:**

- **Managed Runtime → Managed Application**: The platform no longer requires zero external dependencies. Teams can bring their own application-layer libraries (utility deps, thick SDK clients), trading a proportional degree of management for that flexibility. Full conformance still means a fully managed experience; deviation degrades it proportionally. Build/delivery infrastructure constraints remain opinionated regardless of deviation.

- **No brownfield migration mandate**: EDPR will not require or prioritize mass migrations of existing applications. Adoption is organic — teams add primitives for new functionality, not by replacing what works.

- **No cliff edge**: The previous "no external dependencies" rule blocked teams entirely if they needed anything outside the BOM. The new model treats partial conformance as a first-class state with transparent cost (a visible "conformance meter"), removing that hard cutoff.

- **Application surface area investment enabled**: Closer partnership with CIS teams (who own many of the primitives) and bandwidth freed by a stable GA platform now allow EDPR to invest heavily in unifying the inner-loop primitive set.

- **Agentic coding becomes a first-class concern**: EDPR's constrained, opinionated model is exactly the environment where coding agents are most effective. Work underway includes injecting EDPR context into agent sessions via NECP, Claude.md, hooks, and subagents.

---

## The 3 Strategic Pillars

### Pillar 1: Consistent Application Surface Area

A small, cohesive set of primitives that form the building blocks of any EDPR application:

| Primitive | Capability |
|---|---|
| **SAZ** | Auth |
| **RAW Hollow** | Data store |
| **HLIPC** | IPC (service-to-service calls) |
| **Link** | Events (push) |
| **GraphQL** | Queries / composition |
| **Conductor SDK** | Distributed coordination / workflows |

**Design principles:**
- Each primitive works the same way regardless of whether the application is on the managed runtime or a standard SBN app — they are engineered to be independently adoptable.
- Shared patterns across all primitives: how integrations are declared, how typed clients are generated, how schemas are validated and evolved, how environments route.
- An engineer who learns one primitive can approach any other with the same mental model, drastically reducing onboarding cost.
- The CIS Primitive Capability Opinions document codifies the consistency standard spanning schema authoring, integration declaration, evolution guardrails, and access control.

**Maturity today:** HLIPC, RAW Hollow, and Conductor are relatively mature. Link, Hermes, and GraphQL are earlier in the journey. EDPR is working with CIS partners to harden the latter set.

**What is NOT a primitive (system integrations instead):** Graph Search, UDA, data movement (SDSP), Paperboy. These are capabilities the platform wires your app into automatically — you don't write code for them. The managed application handles the integration overhead so teams don't have to.

---

### Pillar 2: Managed Application

Evolution of the managed runtime — not a separate offering.

**Three layers:**

1. **Application Layer (Inner Loop)** — what EDPR owns vs. what the developer owns:
   - EDPR controls the Spring Boot server shell (removes EZConfig boilerplate, discourages Spring "magic")
   - Standardized abstractions = the surface area primitives above
   - Code generation preferred over distributing thick libraries
   - Deviation allowed for application-layer deps (utility libraries, thick SDK clients), but NOT for deps that require changes to build/delivery infrastructure (e.g., Flyway post-deploy hooks for Postgres schema management are explicitly out of scope for this phase)

2. **Build Layer** — Opinionated Gradle lifecycle:
   - Near-empty build.gradle; EDPR plugin pre-configures everything
   - BOM applied automatically; opt-in custom deps allowed (with cost: longer build times, owner responsibility for compatibility)
   - Validation injected into the build graph (schema compatibility, IPC contract checks)

3. **Deployment & Resources (Outer Loop)** — zero config files in user repos for operational concerns:
   - EDPR provisions app + all associated resources
   - Archetypes integration for centralized Jenkins/Spinnaker management across the fleet
   - Standardized environments: Test, Staging, Production, plus forked preview environments for testing destructive changes in full isolation
   - Environment routing baked into all primitives (prefers peer app in same preview env, falls back to main stack)

**Adoption paths:**
- **Existing apps**: Adopt primitives incrementally for new functionality — no migration required
- **New apps**: Start on managed application, get full opinions by default, diverge proportionally if needed
- **Hybrid**: Run managed application for conforming services + a separate SBN app for non-conforming needs, integrated via surface area primitives
- **Graceful ejection**: Since EDPR is built on SBN, EDPR can create delivery pipelines and hand over previously managed resources, transitioning to a standard SBN app without reimplementation

---

### Pillar 3: A Harness for Agentic Coding

The constrained, opinionated EDPR model makes it uniquely well-suited for AI coding agents:

- Agents need a knowledgeable "engineer" to steer them — EDPR's constraints and ownership of operations allow the platform itself to fill that role
- Current work: injecting EDPR context into agent sessions via NECP
- Planned: a cohesive agent harness specifically tuned for EDPR applications (Claude.md, hooks, subagents, auto-activating EDPR-specific AI skills for developers in EDPR repos)
- The abstractions and tools built for human developers are expected to work equally well for agents

---

## Key Decisions & Principles

- **Paced evolution alignment**: Platform layer should be stable — reinvention belongs at the application layer. Platform's role is to enable business-layer reinvention, not impose infrastructure churn.
- **Value proportional to adoption**: No cliff edges, no all-or-nothing. Partial conformance is explicitly supported and surfaced via a "conformance meter."
- **No mandated migrations**: Teams adopt on their own timelines when natural opportunities arise. Success is not measured by fleet-wide adoption percentage.
- **Small surface area is load-bearing**: Keeping the primitive set deliberately small is how EDPR reduces long-term maintenance complexity and makes the environment learnable for both humans and agents.
- **Deviation is reversible**: Teams can bring non-BOM dependencies and remove them later, restoring full management. This is explicitly a dial, not a one-way door.

---

## Notable Open Questions & Debates (from Comments)

- **Concrete deviation examples**: Multiple commenters asked for concrete examples of how the deviation/conformance dial works in practice. Eduardo clarified that "application dependencies" (utility libs, team POJOs, thick SDK clients) are in scope, while "broad dependencies" requiring build/delivery changes (e.g., Temporal, Postgres via Flyway) are out of scope for this phase.

- **Spring context modifications**: Unresolved — bringing a thick client that ships with autoconfiguration is acknowledged as something that needs design, described as "advanced and off-path but still possible."

- **Auth (SAZ) as a primitive**: Was explicitly added to the inner loop surface area after comments flagged the proliferation of auth in CE use cases.

- **Adoption tracking gap**: Toolbox/primitive adoption has not been actively measured or instrumented — acknowledged as a gap if the strategy leans into incremental adoption. Managed runtime adoption is tracked via the EDPR UI, but standalone primitive adoption is not.

- **Timeline expectations**: At least one commenter noted that the no-mandated-migration stance will significantly lengthen the timeline to a homogeneous fleet, and that org expectations should be set accordingly.

- **System integrations (UDA, data movement, notifications)**: Some commenters pushed for including these in the application surface area primitives. Eduardo's position: these are system integrations (platform wires them up), not inner-loop primitives (what you write code against). Open debate about whether tighter integrations could eventually promote them to primitive status.

- **Measuring "outsized value"**: Multiple comments flagged that the strategy's promise of outsized value for conforming teams needs data to back it up — campaigns saved, dependency-related incident costs, etc. Acknowledged as important but not yet quantified.

- **Agentic harness scope**: Comment thread describes the vision as: layering decision-making context into NECP so agents understand EDPR conventions, available dependencies, correct config, and architectural motivations — plus auto-activating EDPR-specific AI skills/plugins for devs working in an EDPR repo. Eduardo confirmed this aligns with his thinking and added managing the agent loop (Claude.md, hooks, subagents) as an explicit goal.
