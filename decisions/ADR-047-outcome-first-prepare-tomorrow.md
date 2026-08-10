---
id: ADR-047
title: Outcome-first teaching language — Prepare Tomorrow is the flagship expression
owner: EduVijna Enterprise Architecture Office · Product Architecture
status: approved
version: 1.0.0
created: 2026-08-10
last_updated: 2026-08-10
reviewers:
  - Founder / Product Architecture
  - Principal Software Engineer
---

# ADR-047 — Outcome-First Teaching Language (Prepare Tomorrow)

**Status:** Accepted  
**Date:** 2026-08-10  
**Related:** ADR-044 · ADR-045 · ADR-046 · PA-INTENT-001 · TLM-001 · EXPERIENCE_PRINCIPLES.md  
**Strategic thesis:** This is a primary differentiator for EduVijna Teacher OS.  
**History:** Formerly drafted as ADR-044, then ADR-046; renumbered so **ADR-046** can lock Artifact Status Lifecycle.

---

## Context

Most education tools ask teachers to operate **generators**:

> Generate Worksheet · Generate Quiz · Generate Lesson Plan

That forces teachers to think in tool silos and assemble a class kit manually. EduVijna’s Teacher Journey and Teaching Intent models reject the tool zoo.

## Decision

Teachers express **outcomes in natural teaching language**. The flagship expression is:

> **Help me prepare tomorrow.**

That single intent may eventually orchestrate many Artifacts, for example:

- Lesson Plan  
- Worksheet  
- Quiz  
- PPT / slides  
- Homework  
- Exit Ticket  
- Bloom Analysis  
- Answer Key  

**The teacher does not need to think about those individually** as separate products. Types remain Artifact attributes; every Artifact uses the **ADR-046** lifecycle. Orchestration is behind Teaching Intent (ADR-045) and stable product services (ADR-044) — never via frontend agent/MCP calls.

Direct “Generate X” entry points may still exist for power users and legacy paths, but they are **not** the primary Teacher OS mental model.

## Consequences

**Positive**

- Unique, teacher-first positioning  
- Aligns Mission → Prepare → Review Queue loop  
- Scales new Artifact types without new top-level tools  

**Negative / follow-ups**

- Orchestration, Review Queue, and Intent UX must land before the promise is fully real  
- Honest Mission copy when kits are incomplete (Never Surprise Users)  
- Multi-artifact “Prepare Tomorrow” orchestration remains deferred until unlocked by architecture review  

## Alternatives considered

1. **Generator-first IA (nav = Worksheet, Quiz, …)** — Rejected: TLM / PA-001.  
2. **Chatbot that only suggests which generator to open** — Rejected: still tool-centric; weak differentiation.

## Compliance

Product copy, nav, and Prepare flows must prefer outcome language. New top-level generator destinations in Teacher OS nav are non-compliant without Architecture change.
