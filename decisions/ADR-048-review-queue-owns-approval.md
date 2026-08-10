---
id: ADR-048
title: Review Queue owns approval — teacher judgement only
owner: EduVijna Enterprise Architecture Office · Product Architecture
status: approved
version: 1.0.0
created: 2026-08-10
last_updated: 2026-08-10
reviewers:
  - Founder / Product Architecture
  - Principal Software Engineer
---

# ADR-048 — Review Queue Owns Approval

**Status:** Approved — **binding for Review Queue**  
**Date:** 2026-08-10  
**Related:** PA-REVIEW-Q-001 · ADR-042 · ADR-044 · ADR-045 · ADR-046 · ADR-047 · EXPERIENCE_PRINCIPLES.md · REVIEW_QUEUE.md

---

## Decision

**Review Queue owns approval.**

It does **not** own:

- Generation  
- Editing (as a primary responsibility)  
- Orchestration  

It owns **teacher judgement** only.

```text
Intent / Orchestration / Capabilities
        ↓
Artifacts (Generated → In Review)
        ↓
Review Queue  ←  teacher judgement
        ↓
Approved | Rejected | Regenerate request | Explain | Open editor
```

## Allowed actions

Review Queue may offer only:

| Action | Meaning |
|--------|---------|
| **Review** | Inspect the Artifact (preview / kit context) |
| **Approve** | Teacher accepts → lifecycle toward Published |
| **Reject** | Teacher declines / dismisses |
| **Regenerate** | Request a new version (capability runs elsewhere) |
| **Request explanation** | Surface *why* this draft exists |
| **Open editor** | Hand off to an editor surface — queue does not *become* the editor |

**Nothing more.**

## Forbidden

| Forbidden | Why |
|-----------|-----|
| Generate as a primary verb | Generation belongs to Intent / orchestration / capabilities |
| Embed full authoring / editing as the queue’s job | Editors are separate; queue opens them |
| Run or own orchestration | Orchestration is not Review Queue |
| Auto-publish without judgement | Violates “AI Assists, Teacher Decides” |
| Type-specific alternate approval cockpits | One queue; ADR-046 lifecycle |

## Complements Experience Principles

Review — **not** generation — is the primary Teacher OS experience after Intent.

This ADR locks that boundary so Wave 1+ slices cannot turn `/teacher-os/review` into a second generator menu or an orchestration console.

## Consequences

**Positive**

- Clean ownership vs ADR-045 (Intent), ADR-047 (outcome language / Prepare Tomorrow), ADR-044 (AI behind services)  
- Review Queue stays the approval cockpit (PA-REVIEW-Q-001)  
- Regenerating is a *request*, not in-queue generation UX  

**Negative / follow-ups**

- “Open editor” and “Regenerate” need clear hand-offs to other surfaces/services  
- EBP-001.5 must implement only judgement actions; generation stays out of scope for the queue itself  

## Alternatives considered

1. **Queue also generates missing kit items** — Rejected: blurs Intent/orchestration with approval.  
2. **Inline full editing as the queue product** — Rejected: queue becomes a mega-editor; Progressive Disclosure fails.  
3. **Per-type approval screens** — Rejected: breaks type-agnostic Artifact model (ADR-046).  

## Compliance

- `REVIEW_QUEUE.md` must state: owns judgement, not generation/editing/orchestration.  
- Engineering Constitution cites ADR-048.  
- EBP-001.5 and later Review Queue EDRs must not add Generate as a queue responsibility.  
- Lifecycle transitions remain ADR-046 (`In Review` → `Approved` / reject paths).
