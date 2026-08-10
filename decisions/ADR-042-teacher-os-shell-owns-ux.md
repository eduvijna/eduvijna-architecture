---
id: ADR-042
title: Teacher OS Shell owns user experience, not business capabilities
owner: EduVijna Enterprise Architecture Office · Product Architecture
status: approved
version: 1.0.0
created: 2026-08-10
last_updated: 2026-08-10
reviewers:
  - Founder / Product Architecture
  - Principal Software Engineer
---

# ADR-042 — Teacher OS Shell Owns User Experience

**Status:** Accepted  
**Date:** 2026-08-10  
**Related:** PA-001 · EBP-001 · EDR-002 · FEATURE_BOUNDARIES.md

---

## Context

Teacher OS introduces a permanent application shell (navigation, layout, session context, Mission landing). Without a clear ownership boundary, implementers may pull worksheet generation, quiz generation, reports, or analytics *into* the shell module — duplicating existing Platform AI / ERP capabilities and violating repository strategy.

## Decision

**Teacher OS Shell owns user experience. It does not own business capabilities.**

### Shell owns

| Concern | Examples |
|---------|----------|
| Navigation | Outcome nav, routing chrome |
| Orchestration **entry points** | CTAs that start Teaching Intent / deep-link into capabilities |
| Layout | TeacherShell, extension slots, Mission composition |
| Session context | TeacherOsContext selections, Continuous Context *container* |
| User experience | Mission, Review Queue UX, progressive disclosure |

### Shell does **not** own

| Concern | Remains in |
|---------|------------|
| Business logic | Domain modules / API services |
| Worksheet generation | Existing Platform AI / worksheet engines |
| Quiz generation | Existing assessment / Platform AI modules |
| Reports | Existing reporting / analytics modules |
| Analytics | Existing analytics surfaces |

**Business capabilities remain in their existing modules.** The shell orchestrates *entry* and *experience*; it does not re-implement generators.

## Consequences

**Positive**

- Clear module boundaries; EDR-002 shell foundation stays thin  
- Reuse of Platform AI and legacy generators without parallel apps  
- Review Queue and Mission can compose capability outputs without owning engines  

**Negative / follow-ups**

- Shell must expose stable extension points and route constants (already in EDR-002)  
- Deep-links and orchestration contracts must be explicit (Teaching Intent / capability IDs)

## Alternatives considered

1. **Monolithic Teacher OS app owning generators** — Rejected: duplicates existing modules; violates preserve-repos strategy.  
2. **Shell as thin iframe host only** — Rejected: Mission / Review UX requires first-class composition.

## Compliance

Engineering tasks that put generation/report/analytics business logic inside `features/teacher-os` shell packages are **non-compliant** unless Product Architecture is updated first.
