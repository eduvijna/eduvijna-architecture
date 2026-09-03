---
id: ADR-AIEOS-055
title: AIEOS Assessment & Learning Evidence Authority
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.2
created: 2026-09-03
last_updated: 2026-09-03
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-055 — AIEOS Assessment & Learning Evidence Authority

**Status:** Frozen / Approved
**Chief Architect architecture review:** ACCEPTED — 2026-09-03
**Founder / Product Architecture freeze:** APPROVED — 2026-09-03
**Date:** 2026-09-03  
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-052](ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) · [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) · [ADR-AIEOS-054](ADR-AIEOS-054-aieos-teaching-execution-observation-authority.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR freezes the **AIEOS Assessment & Learning Evidence Authority** for Teacher OS **TOS-DEV08**. Founder / Product Architecture approval was granted **2026-09-03**. Architecture freeze ≠ Backend implementation authorization ≠ Frontend implementation authorization ≠ migration authorization ≠ OpenAPI authorization ≠ NATS provisioning ≠ Temporal authorization ≠ deployment authorization ≠ production mutation authorization. Implementation slices **DEV08-I01+** require separate Chief Architect authorization.

**ID family note:** `ADR-AIEOS-055` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS ADR-046 / ADR-047 / ADR-048 product-language decisions.

**Architecture choice (TOS-DEV08A / TOS-DEV08P1):** OPTION D — Hybrid class-level-first Assessment. AIEOS owns durable **ClassroomAssessment**; ERP / SIS remains Class and roster master authority.

Does **not** reopen: Generic Content authority (ADR-AIEOS-027); DEV04 preparation architecture (ADR-AIEOS-052); Review Queue authority (ADR-048); Publication implementation; TeachingAssignment authority (ADR-AIEOS-053); TeachingExecution authority (ADR-AIEOS-054); ADR-AIEOS-046R1 publisher scope.

Historical ADR-AIEOS-053 and ADR-AIEOS-054 bodies remain unchanged.

TOS-DEV08A discovery and TOS-DEV08P1 design + adversarial validation are **COMPLETE**. Founder / Product Architecture approval was granted on **2026-09-03**. This freeze does **not** authorize implementation.

---

## Context

Teacher OS has proven the preparation, review, publication, assignment, and classroom-execution sequence through TOS-DEV04 / TOS-DEV05 / TOS-DEV06 / TOS-DEV07:

```text
Teaching Intent
  → TeachingWork
  → preparation (Generate / Prepare)
  → Generic Content / ContentVersion
  → ReviewDecision
  → Publication
  → TeachingAssignment
  → TeachingExecution (Taught)
```

`/teacher-os/assess` and `/teacher-os/improve` remain placeholders. TeachingExecution owns **Taught** truth only. ADR-AIEOS-054 explicitly defers learner-specific evidence and assessment truth.

TOS-DEV08A established that AIEOS currently has:

- **assessment content** as Generic Content (`quiz`, `worksheet`, `homework`)
- **no** Assessment SoR
- **no** learner identity / roster SoR in AIEOS
- **no** learner attempt / submission / score / mastery SoR

**Repurposing forbidden:**

| Existing aggregate | Must NOT become assessment truth |
|--------------------|----------------------------------|
| **TeachingAssignment** | Assignment intent ≠ assessed |
| **TeachingExecution** | Taught ≠ Assessed |
| **Content / ContentVersion** | Artifact payload ≠ assessment event |
| **TeachingExecutionObservation** | CLASS_OBSERVATION ≠ assessment result |

ADR-AIEOS-012 / ADR-AIEOS-013 / ADR-AIEOS-014 (historical Learner Intelligence / Education Domain / Platform Data titles) were **not found** in this repository. This ADR does not invent their contents. Mastery remains outside Assessment unless a later recovered frozen ADR explicitly says otherwise.

---

## Decision

### 1. Separation invariants (binding)

Preserve ADR-AIEOS-053 and ADR-AIEOS-054:

```text
Generated   ≠  Approved
Approved    ≠  Published
Published   ≠  Assigned
Assigned    ≠  Taught
Taught      ≠  Assessed
Assessed    ≠  Mastered
```

Also preserve:

```text
Assigned  ≠  Externally Delivered
Assigned  ≠  Attempted
Assigned  ≠  Submitted
Assigned  ≠  Graded
```

**ClassroomAssessment** introduces only the authoritative **Assessed / class-level assessment evidence** boundary. It does **not** introduce mastery, learner attempts, or Improve recommendations.

### 2. Authority table

| Concern | Authority |
|---------|-----------|
| Teaching preparation intent / context | **TeachingWork** — AIEOS Teaching |
| Artifact / version payload | **Content / ContentVersion** — AIEOS Content ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)) |
| Teacher approval | **ReviewDecision** — AIEOS Content governance ([ADR-048](ADR-048-review-queue-owns-approval.md)) |
| Publication eligibility | **Publication** |
| Class assignment intent | **TeachingAssignment** — AIEOS Teaching ([ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md)) |
| Actual classroom execution | **TeachingExecution** — AIEOS Teaching ([ADR-AIEOS-054](ADR-AIEOS-054-aieos-teaching-execution-observation-authority.md)) |
| Class / roster master | **ERP / SIS / Admin School Context** |
| Class-level assessment evidence / grades / results | **ClassroomAssessment** — AIEOS Assessment (this ADR); learner-specific Assessment results remain future Assessment design after roster authority exists |
| Learner identity / membership | **ERP / SIS / Admin School Context** (not AIEOS Assessment baseline) |
| Learner visibility / attempt / response / submission facts | **Future Student / Learning domain** ([ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md)) — **not** Assessment SoR |
| Mastery / learner model | **Future Learner Intelligence** (not this ADR) |
| Improve recommendations | **Future Improve** (not this ADR) |

ADR-AIEOS-053 remains the current Frozen / Approved authority for learner visibility / attempt / submission. This ADR **must not** silently move attempt/submission SoR ownership into Assessment. Assessment may later **consume** governed Student / Learning evidence and record/derive separately governed Assessment results; it does **not** become the attempt/submission SoR merely because a future type might be named `AssessmentAttempt`. Any later move of attempt/submission ownership into Assessment requires an explicit governed forward revision of ADR-AIEOS-053.

### 3. ClassroomAssessment aggregate

**Name:** `ClassroomAssessment`  
**Domain:** Assessment  
**Proposed PostgreSQL schema:** `assessment`

The aggregate conceptually owns **only** what is required to represent a represented teacher's governed **class-level** Assessment result against an exact immutable ContentVersion and an authorized ClassRef.

**Minimum conceptual identity / state (DEV08 baseline):**

| Field | Semantics |
|-------|-----------|
| `assessment_id` | Aggregate identity (UUIDv7) |
| `tenant_id` | Tenant scope |
| `teacher_principal_id` | Represented / effective HUMAN teacher (see §5) |
| `class_ref` | Opaque School Context class identifier |
| `content_id` | Exact Content identity |
| `content_version_id` | Exact immutable ContentVersion |
| `class_result_level` | `DEMONSTRATED` \| `MIXED` \| `NOT_YET_DEMONSTRATED` |
| `class_result_note` | Optional class-level commentary (max 4096 characters) |
| `lifecycle_state` | `RECORDED` \| `VOIDED` |
| `work_id` | Optional TeachingWork composition reference |
| `execution_id` | Optional TeachingExecution composition reference |
| `assignment_id` | Optional TeachingAssignment composition reference |
| `aggregate_revision` | Optimistic concurrency |
| `recorded_at` | Server-controlled timestamp at RECORD |
| `voided_at` | Set on VOID |
| `created_at`, `updated_at` | Audit timestamps |

**Explicitly excluded from DEV08 baseline:**

- `learner_id`, `student_id`, LearnerRef, StudentRef
- roster snapshot, membership snapshot
- learner attempt / response / submission facts or aggregates
- individual result, individual score, individual grade
- mastery, competency attainment
- misconception / strength / weakness as raw fact columns
- Improve recommendation fields
- NATS event identity as business authority
- cross-domain PostgreSQL foreign keys by default

Composition validation belongs to governed application authority (ResourceRef / UoW reads), consistent with [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md).

### 4. Baseline is class-level only

The first product boundary allows a represented teacher to record a governed class-level Assessment fact **without inventing learner identity**.

Future learner-specific Assessment requires separately governed learner / roster authority **and** remains composed with Future Student / Learning attempt/submission facts per ADR-AIEOS-053. Learner-specific Assessment design must be **ADDITIVE**. It must **not** redefine ClassroomAssessment ownership, must **not** make TeachingAssignment attempt truth, must **not** make TeachingExecution assessment truth, and must **not** move attempt/submission SoR ownership into Assessment without an explicit ADR-AIEOS-053 forward revision.

Student OS remains outside this baseline.

### 5. Teacher principal semantics

Compatible with ADR-AIEOS-053 / ADR-AIEOS-054:

| Field | Meaning |
|-------|---------|
| `teacher_principal_id` | Represented / effective **HUMAN teacher** whose ClassroomAssessment is recorded |
| `principal_id` | Actual authenticated / calling AIEOS Principal |
| `effective_actor_id` | Principal on whose behalf the governed action is executed |

Direct Teacher OS baseline: `teacher_principal_id` = `effective_actor_id` = `principal_id`.

Delegation is **NOT** implemented. Audit must preserve actual / effective provenance per [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) and [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md).

`teacher_principal_id` is **not** automatically the transport caller, workload identity, or service account.

### 6. ClassRef — Bootstrap Assessment authorization semantics

Class / roster remains ERP / SIS / Admin School Context authority. ClassroomAssessment does **not** create a Class SoR.

**Bootstrap Assessment authorization semantics (DEV08 baseline):**

The existing server-side proof that `class_ref` is a **current teaching target available to the represented teacher** (same School Context port used by TeachingAssignment CREATE / TeachingExecution START) is accepted as sufficient current ClassRef authority to **record class-level Assessment**.

This is **Bootstrap Assessment authorization semantics**. It is **not** a universal permanent rule that `assignable == assessable`. Future ERP/SIS integration may expose distinct assessment authority.

Frontend class lists are **advisory UX only**. A stale cached ClassRef is **not** authorization.

All protected Assessment mutations **must** re-evaluate current server-side ClassRef authority. Historical ClassroomAssessment ownership / state is **not** perpetual authorization.

Unknown / cross-tenant / stale / unauthorized / authority-unavailable → **FAIL CLOSED**.

Do **not** invent learner membership.

### 7. Exact ContentVersion source-authority (three cases)

An Assessment must identify the **exact** ContentVersion actually being assessed. After RECORD, `content_id` + `content_version_id` become **immutable**. Assessment **never** follows a later published/current pointer.

Eligible kinds: `quiz`, `worksheet`, `homework`.  
Reject: `lesson_plan`, `answer_key`, `teacher_notes`.  
Do **not** create a new Generic Content type named `assessment` in DEV08. Do **not** change existing education schemas.

#### CASE A — Execution-bound

When `execution_id` is supplied:

- TeachingExecution must exist in the same tenant.
- Represented teacher and `class_ref` must match the execution.
- Supplied `work_id`, if any, must match `execution.work_id`.
- Execution lifecycle must be **`COMPLETED`**. `IN_PROGRESS` or `CANCELLED` cannot serve as execution-linked Assessment source in the baseline.
- The exact ContentVersion must be an eligible (`quiz` / `worksheet` / `homework`) immutable `TeachingExecutionContentBinding`.
- That historical execution-bound version remains eligible even if `Content.published_version_id` later moves.
- Do **not** follow the current published pointer.

#### CASE B — Assignment-bound

When `assignment_id` is supplied:

- TeachingAssignment must exist in the same tenant.
- Represented teacher and `class_ref` must match the assignment.
- Exact `content_id` / `content_version_id` must equal the assignment's immutable binding.
- Audience remains `CLASS`.
- Later publication movement does **not** invalidate that historical version.
- Assignment existence **MUST NOT** imply attempted, submitted, or graded.

#### CASE C — Standalone

Without `execution_id` or `assignment_id`:

- The exact requested ContentVersion must currently equal `Content.published_version_id`.
- Content type must be learner-facing eligible assessment material (`quiz` / `worksheet` / `homework`).

If both `execution_id` and `assignment_id` are supplied, all composition facts must be mutually consistent. No automatic lifecycle mutation of either source aggregate.

### 8. Atomic RECORD model

Do **not** introduce `IN_PROGRESS` merely by copying TeachingExecution. ClassroomAssessment is created atomically by **RECORD**.

Successful RECORD produces **`RECORDED`**.

**Lifecycle states (only):**

```text
RECORDED
VOIDED
```

**Rejected lifecycle states:** `IN_PROGRESS`, `COMPLETED`, `CANCELLED`, `ASSESSED`, `GRADED`, `MASTERED`.

**RECORDED** means: the represented teacher successfully recorded this class-level Assessment result.

**RECORDED is NOT proof of:** all learners assessed; learner submission received; mastery achieved.

### 9. Class result contract

Freeze-candidate enum (teacher-entered class-level facts):

| Code | Meaning |
|------|---------|
| `DEMONSTRATED` | Teacher judges that class-level evidence from this assessment broadly demonstrates the intended understanding for this exact assessment context |
| `MIXED` | Teacher judges the class-level evidence is materially mixed / uneven |
| `NOT_YET_DEMONSTRATED` | Teacher judges the class-level evidence does not yet demonstrate the intended understanding |

None of these means **Mastery**. None is a reteach recommendation. Do **not** use `NEEDS_RETEACH` / `CLASS_NEEDS_RETEACH` as a raw result enum (reteaching is a future Improve recommendation).

No percentage or fake aggregate score is required in baseline.

### 10. Class result note

Optional `class_result_note`, maximum **4096** characters.

- Class-level teacher commentary only
- Does not replace `CLASS_OBSERVATION`
- Does not overwrite TeachingExecution observations
- Is not Mastery
- Is not learner-specific structured evidence
- Must not automatically become AI input
- Must not automatically become learner evidence

The schema contains **no** structured learner identity.

Teacher OS should instruct the teacher **not** to include learner-identifying information in this class-level field. This ADR does **not** claim that free-text validation can technically guarantee absence of PII. Treat teacher-entered free text according to AIEOS data minimization and audit requirements.

### 11. CORRECT and VOID

RECORDED may be changed only through explicit governed commands. No silent overwrite. No ordinary generic PATCH semantics.

**CORRECT:** `RECORDED` → `RECORDED`

- Requires If-Match / expected aggregate revision and Idempotency-Key
- Changes only the current `class_result_level` / `class_result_note`
- Increments `aggregate_revision`
- Stale revision → **FAIL CLOSED**

**VOID:** `RECORDED` → `VOIDED`

- Requires If-Match and Idempotency-Key
- VOID means this previously recorded Assessment has been explicitly invalidated
- Do **not** call this CANCEL
- `VOIDED` is terminal

### 12. Audit vs business-history SoR

Security audit per [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) records mutation provenance and before/after facts required for accountability.

**Security audit is NOT the Assessment business-history SoR.** Application / business history is **not** owned by the audit ledger.

Baseline product needs only **current Assessment state** + governed mutation audit.

If future product requirements require teacher-visible correction history, define a dedicated Assessment history/correction model in a later governed design. Do not add it merely for future-proofing now.

Conceptual audit actions (candidates aligned with RECORD / CORRECT / VOID; **final capability and audit string freeze is deferred to implementation authorization** unless a later ADR freeze restates them):

```text
assessment.classroom.record
assessment.classroom.correct
assessment.classroom.void
```

### 13. Idempotency

RECORD, CORRECT, and VOID use the standard AIEOS command identity rule ([ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md)):

| Condition | Result |
|-----------|--------|
| Same Idempotency-Key + same canonical request | Replay same outcome |
| Same key + different material | Conflict / FAIL CLOSED |
| Different key | Deliberate separate command |

**No** business uniqueness over teacher + class + date + ContentVersion. The same class/artifact may deliberately be assessed multiple times.

### 14. No automatic creation / inference

| Event | Must NOT cause |
|-------|----------------|
| TeachingExecution COMPLETED | Auto-create ClassroomAssessment |
| TeachingAssignment exists or CLOSED | Imply ClassroomAssessment or Assessed |
| ClassroomAssessment RECORDED | Close assignment |
| ClassroomAssessment RECORDED | Alter TeachingExecution |
| ClassroomAssessment RECORDED | Create Mastery |
| ClassroomAssessment RECORDED | Automatically generate Improve recommendations |

UI may offer “Assess this class” from a completed execution. That is navigation, not automatic SoR creation.

Assessment may be recorded **without** a TeachingExecution (Case C). Teach recording is not an artificial prerequisite for Assess.

### 15. Fact / derived / recommendation / mastery

| Concept | Classification |
|---------|----------------|
| `class_result_level` | **FACT** — Assessment SoR |
| `class_result_note` | **FACT** — Assessment SoR |
| recorded / voided state | **FACT** — Assessment SoR |
| exact assessment ContentVersion | **FACT** — Assessment SoR |
| composition references | **FACT** — Assessment SoR |
| misconception | **DERIVED INTERPRETATION** |
| strength | **DERIVED INTERPRETATION** |
| weakness | **DERIVED INTERPRETATION** |
| Bloom / result interpretation | **DERIVED INTERPRETATION** |
| learning-outcome interpretation | **DERIVED INTERPRETATION** |
| reteach target | **RECOMMENDATION** — future Improve |
| follow-up activity | **RECOMMENDATION** — future Improve |
| next Teaching Intent suggestion | **RECOMMENDATION** — future Improve |
| competency attainment | **MASTERY / future Learner Intelligence** |
| long-term proficiency | **MASTERY / future Learner Intelligence** |
| learner model | **MASTERY / future Learner Intelligence** |
| personalized learning state | **MASTERY / future Learner Intelligence** |

Do not put derived / AI / recommendation / mastery state into raw Assessment fact columns.

Hard rule:

```text
ClassroomAssessment result  ≠  Mastery
```

### 16. AI authority

Baseline:

- **NO** authoritative AI grading
- **NO** AI-generated learner evidence
- **NO** automatic misconception authority
- No live model dependency required for RECORD / CORRECT / VOID

Future AI-derived interpretation requires separately governed provenance and human-confirmation semantics.

### 17. Improve handoff (read-only contract)

Future Improve **may READ**:

- `assessment_id`, `class_ref`, `teacher_principal_id`
- exact ContentVersion
- `class_result_level`, `class_result_note`
- `work_id`, `execution_id`, `assignment_id`
- later separately governed teacher-confirmed interpretations

Improve does **not** own Assessment facts, does **not** rewrite Assessment facts, and does **not** create Mastery.

This ADR does **not** design full Improve implementation.

### 18. Mission handoff

Future Mission composition may consume recorded Assessment facts as **signals**. It must **not** infer mastery from Assessment existence or result. No Mission implementation is authorized here.

### 19. Events / Temporal

First DEV08 baseline requires:

- **NO** Assessment NATS event family
- **NO** ADR-AIEOS-046R1 publisher expansion
- **NO** Temporal workflow

There is currently no authorized consumer requiring either. Future consumers may trigger separate architecture expansion.

### 20. Security / authorization

Assessment will require a future Assessment capability family under [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md).

**Do not freeze final capability identifier strings in this ADR** beyond conceptual operations:

- record
- correct
- void
- read
- list

All mutations require: current tenant; current represented teacher authority; current ClassRef authority; default **DENY**; security audit.

No browser authority. No JWT authorization snapshot authority. Authorization unavailable → fail closed (distinct from DENY).

### 21. Item identity (future, not baseline)

`quiz` / `worksheet` / `homework` payloads already require unique `question.id` within a ContentVersion. `(content_version_id, question.id)` is sufficient immutable identity for a **future** optional class-level item result. DEV08 baseline does **not** require item outcomes. Do not create global item-bank identities. Do not modify Content schemas in this ADR.

### 22. Future learner-specific extension

After authoritative learner identity / roster architecture exists:

- **Future Student / Learning** may own learner attempt / response / submission facts ([ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md)).
- **Future Assessment** may consume those governed facts and produce assessment results / interpretations according to a separately approved learner-specific Assessment design.

Both may reference:

- authoritative learner reference
- exact ContentVersion
- assessment / class context (optionally `ClassroomAssessment`)

The future design **MUST** preserve:

```text
TeachingAssignment  ≠  Attempt
TeachingExecution   ≠  Assessment
Attempt / Submission ≠  Assessment Result
Assessment Result   ≠  Mastery
```

This ADR does **not** freeze a new learner-specific aggregate name. Names such as `AssessmentAttempt`, `LearnerAssessment`, or `AssessmentSubmission` remain future design questions unless a frozen authority already establishes them. Assessment must **not** become attempt/submission SoR by naming convenience.

Student OS remains outside this baseline.

### 23. Future implementation plan (architecture intent only)

All require **separate post-freeze authorization**. This ADR does **not** authorize them.

| Slice | Intent |
|-------|--------|
| DEV08-I01 | Assessment domain + persistence |
| DEV08-I02 | Assessment application / API |
| DEV08-I03 | Teacher OS Assess UX |
| DEV08-I04 | Real-stack Product E2E |

Every slice will include both Lane A — UI and Lane B — Backend without inventing unnecessary changes. Backend-led slices may keep Lane A as explicit read-only / no-change contract verification.

### 24. Conceptual persistence

Prefer one aggregate table unless later evidence proves a child record is needed:

```text
assessment.classroom_assessments
```

Do **not** introduce in baseline: learner attempt/submission tables, learner result tables, or mastery tables.

No migration is authorized by this ADR.

---

## Non-goals

- Learner identity / roster implementation
- Student OS
- Learner attempts / submissions
- Individual scores / grades
- Mastery
- Improve implementation
- AI grading / AI misconception authority
- Assessment events
- Temporal
- New Content types
- Production deployment
- LMS implementation
- Changing ADR-AIEOS-053 or ADR-AIEOS-054 bodies
- Backend / Frontend / OpenAPI / Alembic change

---

## Consequences

- Teacher OS Assess can later replace PlaceholderPage with a governed class-level RECORD flow without inventing learners.
- Taught remains TeachingExecution; Assessed becomes ClassroomAssessment; Mastered remains future Learner Intelligence.
- Historical execution-bound and assignment-bound ContentVersions remain assessable after later publication (Cases A/B).
- Standalone Assess remains gated to the current published learner-facing version (Case C).
- Founder freeze has been granted. DEV08-I01+ still requires separate Chief Architect authorization.

---

## Founder decision

Founder / Product Architecture **APPROVED** class-level-first hybrid ClassroomAssessment (Option D) on **2026-09-03**.

This freeze does **not** authorize Backend, Frontend, migration, OpenAPI, DEV08-I01+, Improve, learner-specific Assessment, Student OS, Mastery, or production deployment.
