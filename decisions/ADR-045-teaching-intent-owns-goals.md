---
id: ADR-045
title: Teaching Intent Owns Goals
owner: EduVijna Enterprise Architecture Office · Product Architecture
status: approved
version: 1.1.0
created: 2026-08-10
last_updated: 2026-08-10
reviewers:
  - Founder / Product Architecture
  - Principal Software Engineer
---

# ADR-045 — Teaching Intent Owns Goals

**Status:** Approved — **Constitutional**  
**Date:** 2026-08-10  
**Implementation:** EBP-001.3 (Teaching Intent Experience)  
**Deferred:** Multi-artifact Prepare Tomorrow orchestration (**ADR-047**) remains approved but not implemented in this slice  
**Related:** ADR-042 · ADR-043 · ADR-044 · ADR-046 · ADR-047 · PA-INTENT-001 · INTENT_AND_WORK.md · ARTIFACT_MODEL.md

---

## Decision

**Teaching Intent captures teacher goals.**

It **never** exposes generators (Worksheet · Quiz · PPT as selectable tools).

Generators become **implementation capabilities** behind the intent — invoked later by orchestration, not chosen as the primary UX.

```text
Teacher goal (Teaching Intent)
        ↓
(later) Capability Orchestration
        ↓
Capabilities (existing engines)
        ↓
Artifacts → Review Queue → Teacher decides
```

| Layer | Owns |
|-------|------|
| **Teaching Intent** | What the teacher wants to accomplish |
| **Orchestration** | Which capabilities to run (not in EBP-001.3) |
| **Generators / engines** | How to produce Artifact types (existing modules) |
| **Teacher OS Shell (ADR-042)** | Experience & entry points only |

## Status

**Approved**

## Implementation

**EBP-001.3** — Intent landing + wizard (goal → context → summary → placeholder Continue).  
No AI · No orchestration · No Review Queue · No Agents · No MCP · No APIs · No DB.

## Consequences

- Prepare nav opens goal selection, not a generator menu  
- Copy and cards speak outcomes (“Tomorrow’s Classes”, “Assessment”), not engines  
- Continue is blocked with “Coming in EBP-001.4” until Review/orchestration slice  

## Compliance

Engineering Constitution cites ADR-045. EDRs must not invert this stack.
