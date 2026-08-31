---
id: ADR-046
title: Artifact Status Lifecycle — one lifecycle for every artifact
owner: EduVijna Enterprise Architecture Office · Product Architecture
status: approved
version: 1.0.0
created: 2026-08-10
last_updated: 2026-08-10
reviewers:
  - Founder / Product Architecture
  - Principal Software Engineer
---

# ADR-046 — Artifact Status Lifecycle

**Status:** Approved — **binding for all Artifact types**  
**Date:** 2026-08-10  
**Related:** PA-ARTIFACT-001 · ADR-042 · ADR-044 · ADR-045 · REVIEW_QUEUE.md  
**History:** Outcome-first Prepare Tomorrow language moved to **ADR-047** so this ID can lock the lifecycle.

---

## Decision

**Every artifact** — without exception — uses the **same** status lifecycle.

Applies to (non-exhaustive):

Worksheet · Quiz · Lesson Plan · PPT · Homework · Rubric · Question Bank · and every future Artifact type.

```text
Draft
   ↓
Generating
   ↓
Generated
   ↓
In Review
   ↓
Approved
   ↓
Published
   ↓
Archived
```

**No exceptions.** No type-specific alternate state machines. No skipping `In Review` for AI-produced output before publish. No parallel “worksheet status” enums that diverge from Artifact status.

## State meanings

| Status | Meaning |
|--------|---------|
| **Draft** | Stub / teacher-started shell; not yet (or not currently) generating |
| **Generating** | Capability/orchestration in progress |
| **Generated** | Machine produced a version; waiting to enter human review |
| **In Review** | In Review Queue / teacher editing, regenerating, or deciding |
| **Approved** | Teacher accepted; ready to publish |
| **Published** | Delivered to intended audience (students / parents / records) |
| **Archived** | Retained in Library history; not active |

> **Clarified by [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md):** The historical Published row above must **not** be read as proof of classroom assignment or learner receipt. Forward interpretation: **Publication** = official / published ContentVersion pointer + eligibility for downstream distribution; **TeachingAssignment** = teacher-owned classroom assignment intent; external delivery is a separate concern. The common lifecycle vocabulary (Draft → … → Published → Archived) is unchanged.

## Mapping from earlier PA wording

| Prior label (PA-ARTIFACT-001 draft) | ADR-046 status |
|-------------------------------------|----------------|
| Draft | Draft |
| *(implicit in-flight)* | **Generating** (explicit) |
| AI Generated | **Generated** |
| Teacher Review | **In Review** |
| Approved | Approved |
| Published | Published |
| Archived | Archived |

## Consequences

**Positive**

- One Review Queue for all types  
- Consistent badges, filters, telemetry (`lifecycle_state`)  
- Cheap to add Rubric / Question Bank / future types  

**Negative / follow-ups**

- Legacy content must adapt or dual-read until migrated  
- Product copy and UI must use these exact status names going forward  

## Alternatives considered

1. **Per-type lifecycles** — Rejected: recreates tool silos; breaks generic Review Queue.  
2. **Skip Generated → jump to In Review** — Rejected as a *separate* public enum; internally generation may move Draft→Generating→In Review, but the canonical public set still includes Generated when a completed machine version exists awaiting queue admission.  
3. **Omit Generating** — Rejected: teachers need honest in-flight status (Never Surprise Users).

## Compliance

- `ARTIFACT_MODEL.md` must align to this ADR.  
- Engineering Constitution cites ADR-046 as binding.  
- New Artifact types **inherit** this lifecycle; they do not invent another.  
- Frontend and APIs expose these statuses; AI platform internals (ADR-044) may use finer job states **mapped** into Generating / Generated.
