---
id: ADR-AIEOS-054
title: AIEOS Teaching Execution & Observation Authority
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-09-01
last_updated: 2026-09-01
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-054 — AIEOS Teaching Execution & Observation Authority

**Status:** Frozen / Approved  
**Chief Architect architecture review:** ACCEPTED — 2026-09-01  
**Founder / Product Architecture freeze:** APPROVED — 2026-09-01  
**Date:** 2026-09-01  
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-046R1](ADR-AIEOS-046R1-aieos-production-event-plane-multi-domain-publisher-scope-revision.md) · [ADR-AIEOS-052](ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) · [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) · [ADR-042](ADR-042-teacher-os-shell-owns-ux.md) · [ADR-044](ADR-044-ai-platform-behind-stable-services.md) · [ADR-045](ADR-045-teaching-intent-owns-goals.md) · [ADR-046](ADR-046-artifact-status-lifecycle.md) · [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) · [ADR-048](ADR-048-review-queue-owns-approval.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR freezes the **AIEOS Teaching Execution & Observation Authority** for Teacher OS **TOS-DEV07 — Teach & Classroom Execution**. Founder / Product Architecture approval was granted **2026-09-01**. Architecture freeze ≠ Backend implementation authorization ≠ Frontend implementation authorization ≠ migration authorization ≠ OpenAPI authorization ≠ School Context provider provisioning ≠ NATS provisioning/change authorization ≠ deployment authorization ≠ production mutation authorization. Implementation slices **DEV07-I01+** require separate Chief Architect authorization.

**ID family note:** `ADR-AIEOS-054` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS ADR-046 / ADR-047 / ADR-048 product-language decisions and from platform ADR-AIEOS-046 / ADR-AIEOS-047 / ADR-AIEOS-048 infrastructure decisions.

**Architecture choice (TOS-DEV07A):** HYBRID / Option D — AIEOS owns durable **TeachingExecution**; ERP / SIS remains Class and future timetable master authority.

Does **not** reopen: Generic Content authority (ADR-AIEOS-027); DEV04 preparation architecture (ADR-AIEOS-052); Review Queue authority (ADR-048); Publication implementation; TeachingAssignment authority (ADR-AIEOS-053); ADR-AIEOS-046R1 publisher scope (no ADR-AIEOS-046R2 required).

Historical ADR-AIEOS-052 and ADR-AIEOS-053 bodies remain unchanged.

---

## Context

Teacher OS has proven the preparation and assignment sequence through TOS-DEV04 / TOS-DEV05 / TOS-DEV06:

```text
Teaching Intent
  → TeachingWork
  → preparation (Generate / Prepare)
  → Generic Content / ContentVersion
  → ReviewDecision
  → Publication
  → TeachingAssignment
  → Teach
```

TOS-DEV06 proves assignment product E2E. The current Teach UI is primarily **TeachingAssignment administration** — not classroom execution.

TOS-DEV07A discovery established that AIEOS today has **no authoritative fact** for:

- a lesson actually **started**
- a lesson actually **completed**
- the **exact material versions actually used** during teaching
- **private execution notes** captured during the lesson
- **class-level teaching observations**

**Repurposing forbidden:**

| Existing aggregate | Must NOT become execution truth |
|--------------------|----------------------------------|
| **TeachingAssignment** | Assignment intent ≠ lesson taught |
| **TeachingWork** | Preparation container ≠ classroom execution |
| **Content / ContentVersion** | Artifact payload ≠ execution record |

---

## Decision

### 1. Separation invariants (binding)

Preserve ADR-AIEOS-053:

```text
Generated  ≠  Approved
Approved   ≠  Published
Published  ≠  Assigned
Assigned   ≠  Externally Delivered
Assigned   ≠  Attempted
Assigned   ≠  Submitted
Assigned   ≠  Graded
```

**New invariants introduced by this ADR:**

```text
Assigned   ≠  Taught
Taught     ≠  Assessed
Assessed   ≠  Mastered
```

**TeachingExecution** introduces only the authoritative **Taught / classroom execution** boundary. It does **not** introduce learner assessment truth.

### 2. Authority table

| Concern | Authority |
|---------|-----------|
| Teaching preparation intent / context | **TeachingWork** — AIEOS Teaching |
| Artifact / version payload | **Content / ContentVersion** — AIEOS Content ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)) |
| Teacher approval | **ReviewDecision** — AIEOS Content governance ([ADR-048](ADR-048-review-queue-owns-approval.md)) |
| Publication eligibility | **Publication** |
| Class assignment intent | **TeachingAssignment** — AIEOS Teaching ([ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md)) |
| Class / roster master | **ERP / SIS / Admin School Context** |
| Actual classroom execution | **TeachingExecution** — AIEOS Teaching (this ADR) |
| Private execution notes | **TeachingExecution** observation state |
| Class-level teaching observations | **TeachingExecution** observation state |
| Learner-specific evidence | **Future Assess / Learner Intelligence architecture** |
| Attempts / submissions / grades / mastery | **NOT TeachingExecution authority** |

### 3. TeachingExecution aggregate

**Name:** `TeachingExecution`  
**Domain:** Teaching  

The aggregate conceptually owns **only** what is required to represent an actual teacher classroom execution.

**Minimum conceptual identity / state (DEV07 baseline):**

| Field | Semantics |
|-------|-----------|
| `execution_id` | Aggregate identity |
| `tenant_id` | Tenant scope |
| `teacher_principal_id` | Represented / effective HUMAN teacher (see §4) |
| `work_id` | Associated TeachingWork |
| `class_ref` | Opaque School Context class identifier |
| `lifecycle_state` | `IN_PROGRESS` \| `COMPLETED` \| `CANCELLED` |
| `started_at` | Server-controlled timestamp at start |
| `completed_at` | Set on COMPLETED |
| `cancelled_at` | Set on CANCELLED |
| `aggregate_revision` | Optimistic concurrency |
| `created_at`, `updated_at` | Audit timestamps |

**Explicitly excluded from DEV07 baseline** (do not add merely for future-proofing):

- `external_period_ref`, `timetable_id`, `period_id`
- roster snapshot, learner IDs
- `assignment_id` array
- PreparationKit ID
- LMS delivery ID
- attempt / submission / result IDs

Future governed migrations **may** add references when their authoritative integration actually exists.

**No PreparationKit aggregate** is introduced. Execution content bindings reference individual **ContentVersion** identities only.

### 4. Teacher principal semantics

Compatible with ADR-AIEOS-053:

| Field | Meaning |
|-------|---------|
| `teacher_principal_id` | Represented / effective **HUMAN teacher** whose actual classroom execution is being recorded |
| `principal_id` | Actual authenticated / calling AIEOS Principal |
| `effective_actor_id` | Principal on whose behalf the governed action is executed |

Direct Teacher OS baseline: `teacher_principal_id` = `effective_actor_id` = `principal_id`.

**Delegation is NOT implemented** in DEV07 baseline. Audit must preserve actual / effective provenance so future separately authorized delegation can be added without reopening this ADR's core semantics.

### 5. ClassRef authority

Class / roster authority remains **external School Context**. TeachingExecution does **not** create a Class SoR.

`TeachingWork.class_label` remains **presentation only** — never Class identity.

**At TeachingExecution START**, server-side current authority **must** validate:

1. `class_ref` resolves in the exact tenant, **and**
2. `class_ref` is currently a teaching target available to the represented / effective teacher under current authority

Frontend class lists are **advisory UX only**. A stale cached ClassRef is **not** authorization.

Unknown / cross-tenant / unauthorized / unavailable authority → **FAIL CLOSED** — no execution commit.

For all later protected execution mutations, backend **must** re-evaluate current server-side business authorization. Historical TeachingExecution ownership / state is **not** perpetual authorization.

Do **not** freeze a specific authorization capability identifier string unless already governed elsewhere.

### 6. Timetable boundary

No AIEOS timetable façade exists today. **DEV07 baseline MUST NOT depend** on full timetable integration.

Minimum Teach flow may operate from:

```text
teacher-selected TeachingWork
  +
teacher-selected authorized ClassRef
```

No period / timetable SoR is introduced. **Do NOT add `external_period_ref` in DEV07** merely as future-proofing.

When an authoritative ERP / SIS timetable integration is later prioritized, a **separate governed design** may add a schedule / period reference without changing TeachingExecution's ownership of actual execution truth.

### 7. ContentVersion execution bindings

TeachingExecution may bind **zero or more** individual exact artifact versions used during the actual execution.

**Conceptual child:** `TeachingExecutionContentBinding`

| Field | Semantics |
|-------|-----------|
| `execution_id` | Parent execution |
| `content_id` | Content identity |
| `content_version_id` | Exact immutable version |
| `artifact_kind` | e.g. `lesson_plan`, `worksheet`, `quiz`, … |

**Binding rules (binding):**

- Immutable reference **after** execution creation / start
- MUST NOT copy Content payload
- MUST NOT become Content authority
- MUST NOT create a PreparationKit aggregate or kit lifecycle
- MUST NOT require exactly six bindings
- MUST NOT require all preparation artifacts
- MUST NOT follow later Content current / published pointers
- MUST NOT silently change after Work regeneration

**Explicitly forbidden binding concepts:** `kit_id`, `kit_revision`, `kit_status`.

Execution binding means only: **this exact ContentVersion was selected / used for this teaching execution.**

If the Work later regenerates an artifact, historical execution bindings remain unchanged.

The binding operation must use current Content / Review authority. This ADR does **not** invent a competing Content approval lifecycle.

Teacher-facing and learner-facing content continue to obey existing Content / Review / Publication authorities.

### 8. Assignment composition

TeachingAssignment remains **independent**.

Teach may **display** assignments relevant to `class_ref`, `work_id`, and content / version — but TeachingExecution does **not** own assignment lifecycle.

**Forbidden inferences:**

| Must NOT infer | From |
|----------------|------|
| TeachingAssignment CLOSED | TeachingExecution COMPLETED |
| TeachingExecution COMPLETED | TeachingAssignment CLOSED |

Do **not** mutate assignment state automatically when a lesson completes.

No mandatory `execution_assignment_bindings` table in DEV07 baseline. If a future Assess / learner-delivery use case proves an exact assignment link is required, govern that separately.

### 9. Execution lifecycle

**Frozen states only:**

```text
IN_PROGRESS → COMPLETED   (terminal)
IN_PROGRESS → CANCELLED   (terminal)
```

Creation / start produces `IN_PROGRESS` with `started_at` = server-controlled timestamp.

**Explicitly rejected lifecycle states:** PLANNED, SCHEDULED, DELIVERED, ASSESSED, GRADED, MASTERED.

**COMPLETED** means only: the represented teacher explicitly recorded that this classroom execution was completed.

**COMPLETED is NOT proof of:**

- learner receipt
- learner participation
- learner understanding
- assessment
- mastery

### 10. Start idempotency

Execution START requires explicit **`Idempotency-Key`**.

| Condition | Result |
|-----------|--------|
| Same `Idempotency-Key` + same canonical material request | Replay same execution / result |
| Same `Idempotency-Key` + materially different request | Conflict — **FAIL CLOSED** |
| Different `Idempotency-Key` | Separate deliberate TeachingExecution |

**Explicitly reject** global business uniqueness over `(teacher_principal_id, work_id, class_ref, date)` or equivalent fields. A teacher may deliberately reteach the same Work to the same ClassRef.

### 11. Concurrency

TeachingExecution mutations use `aggregate_revision` + ETag / If-Match optimistic concurrency.

Stale mutation → **FAIL CLOSED**.

Applies to at least: observation mutation, complete, cancel. Exact implementation mechanics remain implementation concerns.

### 12. Observation model

DEV07 allows **only**:

| Kind | Scope | Example |
|------|-------|---------|
| **A — `PRIVATE_EXECUTION_NOTE`** | Teacher-only note about conducting the lesson | "Visual fraction example worked well." |
| **B — `CLASS_OBSERVATION`** | Class-level observation **without** learner identity | "Several learners confused numerator and denominator." |

DEV07 **MUST NOT** introduce: learner-specific observation, learner ID, student profile, attendance, score, submission, mastery, diagnosis.

**Conceptual child:** `TeachingExecutionObservation`

| Field | Semantics |
|-------|-----------|
| `observation_id` | Identity |
| `execution_id` | Parent |
| `observation_kind` | `PRIVATE_EXECUTION_NOTE` \| `CLASS_OBSERVATION` |
| `body` | Text |
| `recorded_at`, `updated_at` | Timestamps |
| `revision` | Observation-level concurrency |

**Mutation semantics:**

- While execution = `IN_PROGRESS`: teacher may create and **correct** permitted observations under current authority and concurrency rules
- After execution becomes `COMPLETED` or `CANCELLED`: observation state becomes historical / **immutable**

Do **not** require append-only UX merely for implementation convenience. Security audit / accountability still applies to protected mutations.

### 13. Content vs execution notes (binding distinction)

| Concept | Authority | When |
|---------|-----------|------|
| **`teacher_notes`** (preparation artifact) | Content / ContentVersion — AI-generated or authored **pre-lesson** material | Prepare |
| **`PRIVATE_EXECUTION_NOTE`** | TeachingExecution observation state — teacher-authored **during** classroom execution | Teach |

Execution notes **MUST NOT** mutate `teacher_notes` ContentVersion.

`teacher_notes` ContentVersion **MUST NOT** be used as execution observation storage.

### 14. Assess handoff (prepare only — do not implement Assess)

A later Assess capability may consume:

- `execution_id`, `tenant_id`, `teacher_principal_id`
- `class_ref`, `work_id`
- `started_at`, `completed_at`
- exact execution ContentVersion bindings
- permitted class observations (`CLASS_OBSERVATION`)

Do **not** require `assignment_id[]` as part of DEV07 execution authority.

**Not in DEV07:** learner attempt, submission, score, grade, mastery, misconception AI, Assessment Insight Agent, learner-specific evidence.

### 15. Event contract

[ADR-AIEOS-046R1](ADR-AIEOS-046R1-aieos-production-event-plane-multi-domain-publisher-scope-revision.md) already authorizes production EVENT publisher scope:

```text
io.eduvijna.aieos.content.>
io.eduvijna.aieos.teaching.>
```

**No ADR-AIEOS-046R2 is required for DEV07.**

**DEV07 baseline event facts (frozen subject to Founder freeze):**

```text
io.eduvijna.aieos.teaching.execution.started.v1
io.eduvijna.aieos.teaching.execution.completed.v1
io.eduvijna.aieos.teaching.execution.cancelled.v1
```

CloudEvent `type` must equal subject per ADR-AIEOS-046R1. Use existing transactional outbox / at-least-once architecture.

Events are facts, **never** TeachingExecution SoR.

**Deferred:** `teaching.execution.observation.*` events — not frozen in DEV07 baseline until a concrete integration consumer exists.

No NATS provisioning / change is authorized by this ADR deposit.

### 16. Security / audit

Preserve [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md), [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md), and Authorization Kernel architecture.

Frontend state is advisory only. Every protected effect uses **current** server-side authorization.

Execution mutations require: correct tenant; represented / effective teacher authority; execution ownership / authority; required current business authorization; valid aggregate revision where applicable.

No historical execution, ClassRef cache, Work state, or Assignment state is itself authorization.

Execution start / complete / cancel and protected observation mutations participate in the current security audit / accountability contract. Exact database CHECK / action strings are implementation details unless already frozen.

### 17. Persistence authority (conceptual — no migration in this deposit)

Teaching schema owns TeachingExecution persistence.

**Expected structures:**

| Table | Purpose |
|-------|---------|
| `teaching.executions` | TeachingExecution SoR |
| `teaching.execution_content_bindings` | Immutable exact version bindings |
| `teaching.execution_observations` | Observation state |

Exact SQL names may be adjusted during implementation only if semantics remain identical and repository conventions require it.

**Requirements:** tenant-scoped; FORCE-RLS-compatible with existing Teaching schema posture; teacher ownership; revision / concurrency; immutable exact content bindings; terminal observation immutability; transactional audit / outbox integration where required.

**Do NOT create migration in this Architecture PR.**

### 18. Teach product minimum (DEV07 boundary)

```text
Teacher opens Teach
  → selects relevant TeachingWork
  → selects currently authorized ClassRef
  → starts TeachingExecution
  → sees preparation artifacts
  → sees relevant assignment facts
  → conducts lesson
  → records private note / class observation
  → completes execution
  → reload proves durable execution + notes + exact version bindings
```

No timetable is required.

Today's Mission → Teach deep-link may be added as **derived UX** during DEV07 without creating a new SoR, but it is **NOT** a prerequisite for TeachingExecution authority.

### 19. Recommended implementation order (NOT authorized yet)

| Slice | Scope |
|-------|-------|
| **DEV07-I01** | TeachingExecution domain + persistence |
| **DEV07-I02** | Teach composition + application / API |
| **DEV07-I03** | Teacher OS Teach UX |
| **DEV07-I04** | Real-stack Product E2E |

Prefer **four slices** rather than five unless implementation evidence proves a split is necessary. Do not create a separate read-only slice merely for architecture neatness if the application / API slice can safely deliver the composition contract.

### 20. Scenario validation

| ID | Scenario | Expected outcome |
|----|----------|------------------|
| A | Start execution for authorized teacher + ClassRef | `IN_PROGRESS` |
| B | Unauthorized / stale ClassRef at START | FAIL CLOSED |
| C | Retry identical START with same Idempotency-Key | Same execution |
| D | Same key + materially different START | Conflict |
| E | Different key with same teacher / work / class | Separate deliberate execution allowed |
| F | Exact ContentVersions bound; later Work regeneration | Execution history unchanged |
| G | Record private note while IN_PROGRESS | Durable |
| H | Correct note while IN_PROGRESS with current revision | Allowed |
| I | Stale observation mutation | FAIL CLOSED |
| J | Complete | `COMPLETED` |
| K | Mutation of observations after COMPLETED | Reject |
| L | Assignment remains ACTIVE / CLOSED independently | Execution completion does not mutate assignment |
| M | Execution COMPLETED | Does not imply assessed / mastered |
| N | No timetable provider | Minimum Teach flow works through Work + ClassRef |
| O | Learner-specific observation submitted | Reject / unsupported |

### 21. Explicit non-goals

This ADR explicitly defers:

- Student OS; learner UI
- Learner roster snapshot; learner-specific observations
- Attendance; attempts; submissions; scores; grades; mastery
- Assessment Insight Agent; AI Teacher Assistant
- MCP implementation; Temporal workflow
- Full timetable / period integration; ERP / SIS mutation; LMS delivery
- Cover pack; principal observation; inspection mode
- Automatic assignment lifecycle mutation
- PreparationKit aggregate; kit lifecycle
- Production deployment
- `teaching.execution.observation.*` event publication (DEV07 baseline)
- NATS provisioning / change

---

## Consequences

### Positive

- Clean **Assigned ≠ Taught** boundary without abusing TeachingAssignment or TeachingWork
- Durable Assess handoff identity with immutable content-version evidence
- Teach can proceed without timetable integration
- Aligns with existing Teaching schema, School Context port, and ADR-AIEOS-046R1 event scope

### Negative / constraints

- New persistence and API surface in Teaching domain
- Observation model intentionally narrow — learner-specific evidence deferred
- Today's Mission and Teach UX require composition work beyond assignment admin

### Neutral

- ADR-AIEOS-052 and ADR-AIEOS-053 historical bodies unchanged
- No ADR-AIEOS-046R2 required

---

## References

- TOS-DEV07A — Teach & Classroom Execution Discovery Report (Chief Architect accepted **2026-09-01**)
- [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) — TeachingAssignment authority
- [ADR-AIEOS-052](ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) — Preparation kit architecture
- [ADR-AIEOS-046R1](ADR-AIEOS-046R1-aieos-production-event-plane-multi-domain-publisher-scope-revision.md) — Teaching event publisher scope
