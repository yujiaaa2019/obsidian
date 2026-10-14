---
title: L5 Promotion Plan — April 2027
date: 2026-03-18
tags: [career, L5, promotion, EDPR, scope-impact, mentoring]
---

# L5 Promotion Plan: April 2027

**Status:** Active | **Target:** April 2027 (13 months) | **Current Level:** L4 (since April 2025)

## The Case in One Paragraph

You are identifying and owning a **platform-critical problem**: EDPR's fragmented resource ownership model blocks scalable cost attribution, campaign routing, and incident response. This work requires you to architect a unified ownership system (Console SoT, cascading Temporal jobs), collaborate cross-functionally with Resource Catalog/SquaSSH/Gandalf teams, and mentor a junior engineer through 12+ months of implementation. Success unblocks EDPR's GA roadmap and becomes a reference pattern for platform ownership across Netflix.

---

## Your L5 Gaps & How This Project Closes Them

| Pillar | Your Strength | Gap | How Project Solves It |
|--------|---|---|---|
| **Scope & Impact** | Executing cross-functional work | **Identifying & driving new problems** | ✓ You spotted fragmentation, articulated impact, drove design with Eduardo |
| **Technical Depth** | Resource lifecycle management | Owning system-level decisions across platforms | ✓ Architect Console/Spinnaker/SquaSSH/Gandalf/Temporal integration |
| **Decision-Making & Comms** | Participates in discussions | **Proactively driving best practices** | ✓ RFC-driven design, monthly stakeholder syncs, decision logs |
| **Influence & Collaboration** | **Strong — cross-functional work** | **Mentoring & growing junior engineers** | ✓ Sustained 12-month partnership with junior L4, document growth |
| **Living Values** | Team accountability | Cross-team values modeling | ✓ How you navigate multi-team complexity with humility |

---

## Quarterly Roadmap

### Q2 2026: Design Finalization & Alignment (Apr–Jun)

**Goal:** RFC locked with Resource Catalog sign-off

**Your role:** Lead architect, gather requirements, farm for dissent

**Key activities:**
- Week 1-2: Meet Resource Catalog team — map dependencies, unblock constraints
- Week 3-4: Publish RFC: "EDPR Resource Ownership: Unified SoT via Console EDPR_APP"
- Week 5-8: Resolve RFC feedback (SAZ handoff, Console plugin design, Temporal reliability)
- EOQ: RFC accepted, Resource Catalog committed to timeline

**Evidence to preserve:**
- RFC doc with design rationale & alternatives considered
- Feedback threads from Spinnaker/SquaSSH/Gandalf/Resource Catalog teams
- Decision log on hard questions (why Console over alternatives? why Temporal over webhooks?)

---

### Q3 2026: Implementation Begins + Mentoring (Jul–Sep)

**Goal:** Identify mentor-ee, Phase 1 delivery (Console EDPR_APP schema + lifecycle)

**Your role:** Architect lead, mentor, decision-maker

**Key activities:**
- Week 1-2: Identify junior L4 engineer (EDPR or adjacent team) — pitch scope & learning opportunity
- Week 3-4: Design implementation plan together:
  - **Phase 1** (Jul–Sep): Console EDPR_APP schema + basic lifecycle (create, update owner)
  - **Phase 2** (Oct–Dec): Temporal job cascading ownership to Spinnaker, SquaSSH, Gandalf
  - **Phase 3** (Jan–Mar 2027): GitHub/Jenkins handoff, integration tests, fleet rollout
- Ongoing: Weekly 1:1s, architecture reviews, unblock impediments
- EOQ: Console schema merged to main, mentee shipping their first major component

**Evidence to preserve:**
- Phase 1 design doc (decisions, tradeoffs, why this API surface)
- Code reviews you gave (show system-level thinking, not just approval stamps)
- Mentoring notes: skills they developed, growth areas, feedback you gave
- Decision thread on schema design (show ownership of tradeoffs)

---

### Q4 2026: Core Integration + Cross-Functional Comms (Oct–Dec)

**Goal:** Temporal cascading job delivered & tested; design deep dive published

**Your role:** Tech lead, proactive communicator, decision owner

**Key activities:**
- Implementation: Temporal job monitors Console for ownership changes, cascades to Spinnaker/SquaSSH/Gandalf
- Proactive communication:
  - Monthly syncs with Resource Catalog, SquaSSH, Gandalf teams on blockers/tradeoffs
  - Publish "Ownership Cascading Design Deep Dive" internal doc for future platforms
- Testing: Work with mentee to design integration tests, canary rollout strategy
- EOQ: Temporal job + SquaSSH sync merged & validated in staging

**Evidence to preserve:**
- Temporal job design doc (tradeoff decisions: polling vs. subscription, retry strategy, consistency model)
- Internal design deep dive (become the reference for ownership patterns)
- Integration test strategy doc
- Meeting notes from stakeholder syncs (show proactive issue resolution)

---

### Q1 2027: Fleet Rollout + Completeness (Jan–Mar)

**Goal:** 80%+ EDPR fleet on new model; production stable; mentee developed

**Your role:** Delivery lead, on-call, mentor wrap-up

**Key activities:**
- Phase 3: GitHub/Jenkins ownership propagation, SAZ manual handoff docs
- Canary rollout: 5 apps → 20 → 100 → fleet (you own expand/go-live decisions)
- Production support: First 2 weeks post-launch, on-call for ownership transfer issues
- Mentoring wrap-up: Mentee leads one more feature (e.g., campaign routing heuristics), you review
- Documentation: EDPR Resource Ownership Runbook + troubleshooting guide
- EOQ: 80%+ fleet adoption, system stable

**Evidence to preserve:**
- Canary rollout decision log (show judgment, risk assessment, go-live criteria)
- Production incident response log (how you handled edge cases)
- Mentoring feedback from mentee (growth, technical development, what you modeled)
- Runbook & operational docs
- Impact metrics (cost attribution accuracy improved, campaign manual triage reduced by X%)

---

### Q2 2027: Promotion Case (Apr)

**Goal:** Package evidence, present to promotion committee

**Activities:**
- Compile impact summary: costs attributed, campaign routing automated, ownership transfers now self-service
- Gather all evidence: RFCs, design docs, code reviews, mentoring feedback, runbooks
- Articulate sustained performance across 4+ quarters (Q2 2026–Q1 2027)

---

## Evidence Portfolio Template

Build this continuously, not at the end. Share with manager monthly.

### 1. Scope & Impact
- Problem identification: How you spotted fragmentation as a systemic bottleneck
- Business impact: Cost attribution, campaign routing simplification, ownership transfers unblocked
- Scope breadth: Affects EDPR fleet, Resource Catalog workflow, campaign systems

### 2. Technical Depth & System-Level Ownership
- Component-level decisions: Why Console (not ServiceMesh)? Why Temporal (not webhooks)? Why cascading (not eventual)?
- Tradeoff analysis: Campaign routing heuristics — infrastructure → EDPR, business logic → user
- Complexity reduction: From ad-hoc ownership scripts to automated Temporal jobs
- Design docs with clear rationale

### 3. Decision-Making & Proactive Communication
- RFC-driven design with published decision logs
- Proactive stakeholder management (monthly syncs, not just reactive participation)
- Transparent communication on tradeoffs (e.g., "We chose X because Y, accepting cost Z, mitigated by W")

### 4. Influence & Mentoring (Closing Your Gap)
- Mentee growth documentation (technical skills, decision-making, independence progression)
- Mentee feedback: What they learned, how you unblocked them, what you modeled
- Cross-functional influence: Did you help Resource Catalog solve their problems too?

### 5. Living Netflix Values
- Radical candor with mentee (examples of direct feedback, holding to high standards)
- Cross-team collaboration: How you sought input, acknowledged constraints
- Transparency: RFCs and decision logs are open, not gatekeeping

---

## Key Milestones for Manager Check-ins

Share these monthly with your manager:

| Milestone | Target | Evidence | Status |
|-----------|--------|----------|--------|
| RFC: Unified Ownership SoT | Jun 2026 | RFC + cross-team sign-off | — |
| Identify & onboard mentee | Jul 2026 | Project kickoff notes, mentoring plan | — |
| Phase 1 (Console schema) shipped | Sep 2026 | Merged PR, design doc, code reviews | — |
| Phase 2 (Temporal job) shipped | Dec 2026 | Merged PR, integration tests, staging validation | — |
| Phase 3 (GitHub/Jenkins) + 50% adoption | Feb 2027 | Canary metrics, on-call summary | — |
| Full system stable + 80% adoption | Mar 2027 | Runbook, incident response, mentee feedback | — |
| **L5 Promotion Case** | **Apr 2027** | Impact summary + evidence portfolio | — |

---

## Project Context

**Problem origin:** EDPR resources are scattered across 15+ systems (Console, Spinnaker, SquaSSH, Gandalf, Jenkins, GitHub, Radar, SAZ, etc.) with no single source of truth for ownership. This blocks:
- Cost attribution (can't charge right team for infrastructure)
- Campaign routing (campaigns go to wrong team or require manual triage)
- Incident response (unclear who owns what during incidents)
- Ownership transfers (manual, error-prone, doesn't cascade)

**Solution space:** Unified ownership in Console (EDPR_APP resource type) as SoT, with Temporal jobs cascading changes to dependent systems (Spinnaker, SquaSSH, Gandalf, GitHub, Jenkins).

**Related docs:**
- [[EDPR Resources - Ownership]] — current design doc with cross-team feedback
- [[EDPR Resource Ownership & Authorization]] — foundational problem statement & current-state analysis
- [[EDPR Directional Pillars 2026+]] — platform strategy context

---

## Success Criteria

✅ RFC published and accepted by June 2026
✅ Mentee identified and onboarded by July 2026
✅ All three phases shipped by March 2027
✅ 80%+ EDPR fleet using new ownership model
✅ Production stable (zero ownership-related incidents)
✅ Cost attribution accuracy improved
✅ Campaign manual triage reduced
✅ Mentee demonstrates clear growth & independence
✅ Evidence portfolio ready for promotion committee by April 2027

---

## Next Steps (This Week)

1. **Schedule with manager:** Share this plan. Confirm 18-24 month timeline. Discuss mentee sourcing.
2. **Create L5 Evidence Doc (shared with manager):** Monthly review cadence.
3. **Schedule Resource Catalog kickoff:** RFC timeline, blockers, dependencies.
4. **Draft RFC outline:** Start writing unified ownership model RFC. Target publication by mid-May 2026.

---

**Last updated:** March 18, 2026
**Owner:** [[Yuji Xie]]
