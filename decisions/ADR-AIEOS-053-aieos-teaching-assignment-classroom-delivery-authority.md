---
id: ADR-AIEOS-053
title: AIEOS Teaching Assignment & Classroom Delivery Authority
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: proposed
version: 1.0.1
created: 2026-08-31
last_updated: 2026-08-31
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-053 — AIEOS Teaching Assignment & Classroom Delivery Authority

**Status:** Proposed / Freeze Candidate  
**Date:** 2026-08-31  
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-052](ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) · [ADR-042](ADR-042-teacher-os-shell-owns-ux.md) · [ADR-044](ADR-044-ai-platform-behind-stable-services.md) · [ADR-045](ADR-045-teaching-intent-owns-goals.md) · [ADR-046](ADR-046-artifact-status-lifecycle.md) · [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) · [ADR-048](ADR-048-review-queue-owns-approval.md)

**Catalogue note:** This ADR is the **architecture freeze candidate** for Teacher OS **TOS-DEV06 — Assignment & Classroom Delivery**. Status is **Proposed / Freeze Candidate**. It is **not** Frozen / Approved until Founder authorization. Architecture freeze (when granted) is **ARCHITECTURE AUTHORITY ONLY** and **does not** authorize Backend implementation, Frontend implementation, database migration, OpenAPI change, School Context provider provisioning, LMS connector work, deployment, or production mutation. Implementation slices **DEV06-I01+** require separate Chief Architect authorization after Founder freeze of this ADR.

**ID family note:** `ADR-AIEOS-053` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS ADR-046 / ADR-047 / ADR-048 product-language decisions and from platform ADR-AIEOS-046 / ADR-AIEOS-047 / ADR-AIEOS-048 infrastructure decisions.

**Narrowly clarifies / supersedes conflicting delivery semantics only in:**

- [ADR-046](ADR-046-artifact-status-lifecycle.md) — Artifact Status Lifecycle (Published meaning)
- [ADR-AIEOS-052](ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) — authority row “Learner delivery truth | Publication”

Does **not** reopen: common artifact lifecycle vocabulary; Generic Content authority (ADR-AIEOS-027); DEV04 preparation architecture; Review Queue authority (ADR-048); Publication implementation.

---

## Context

Teacher OS has completed preparation and publication composition through TOS-DEV04 / TOS-DEV05:

```text
Teaching Work
  → ContentVersion
  → ReviewDecision
  → Publication (published_version_id pointer)
```

Implemented Teacher OS UX already states that publishing does **not** assign or send artefacts to learners. Product and earlier ADR wording sometimes collapsed **Published** with assign/send / learner receipt. TOS-DEV06A discovery established that AIEOS today has:

- **no** Class / Section / Roster / Enrollment SoR in Teaching
- **no** TeachingAssignment aggregate
- **no** LMS connector / delivery-attempt SoR
- School Context / ERP / SIS as Admin-owned master data Teacher OS may **consume**, not administer

This ADR freezes the authority composition for teacher-owned classroom assignment intent without conflating publication, assignment, external delivery, attempts, submissions, or grading.

---

## Decision

### 1. Authority composition

Canonical sequence:

```text
Teaching Work
  ↓
ContentVersion
  ↓
ReviewDecision
  ↓
Publication
  ↓
TeachingAssignment
  ↓
optional external LMS / Student delivery
```

**Separation invariants (binding):**

```text
Generated  ≠  Approved
Approved   ≠  Published
Published  ≠  Assigned
Assigned   ≠  Externally Delivered
Assigned   ≠  Attempted
Assigned   ≠  Submitted
Assigned   ≠  Graded
```

### 2. Authority table

| Concern | Authority |
|---------|-----------|
| Durable teacher preparation context | **Teaching Work** |
| Artifact payload / version | **Content / ContentVersion** ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)) |
| Exact-version teacher approval | **ReviewDecision** ([ADR-048](ADR-048-review-queue-owns-approval.md)) |
| Official / published ContentVersion pointer **and** eligibility for downstream distribution | **Publication** |
| Teacher-owned classroom assignment intent | **TeachingAssignment** (this ADR) |
| Class / Roster / Enrollment master data | **Admin / ERP / SIS School Context** |
| Optional remote mirrored assignment / delivery | **External LMS** (when a future connector exists) |
| Learner visibility / attempt / submission | **Future Student / Learning domain** |
| Grades / results | **Future Assessment authority** |

### 3. Prior ADR clarification (canonical forward interpretation)

Older wording that **Published** itself means delivery to the intended audience **must no longer** be interpreted as proof of classroom assignment or learner receipt.

Canonical forward interpretation:

| Concept | Meaning |
|---------|---------|
| **Publication** | Official / published ContentVersion pointer **plus** eligibility for downstream distribution |
| **TeachingAssignment** | Teacher-owned classroom assignment intent |
| **External delivery** | Separate downstream concern (LMS / Student OS) |

Historical ADR-046 / ADR-AIEOS-052 bodies retain their original text; this ADR provides current precedence for delivery semantics only.

### 4. TeachingAssignment aggregate

**Name:** `TeachingAssignment`  
**Domain:** Teaching  

**Owns conceptually:**

- `assignment_id`
- `tenant_id`
- `teacher_principal_id`
- `content_id`
- `content_version_id`
- `audience_type`
- `class_ref`
- optional `audience_display_label`
- optional `source_work_id`
- `lifecycle_state`
- `assigned_at`
- `available_from`
- `due_at`
- `closed_at`
- `cancelled_at`
- `aggregate_revision`
- `created_at` / `updated_at`
- audit / event identity

**Does NOT own:**

- Content / ContentVersion payload
- ReviewDecision
- Publication
- Class / Section / Roster / Enrollment master records
- learner membership lists
- learner attempts / submissions / grades
- remote LMS assignment records

**No kit-level assignment aggregate.** Assignment is per Content artifact version, never a PreparationKit entity.

#### `teacher_principal_id` — represented / effective teacher

`teacher_principal_id` is the **represented / effective HUMAN teacher Principal** whose classroom assignment intent the TeachingAssignment records.

It is **not** automatically:

- the transport caller
- the authenticated workload
- the executing service
- the delegated acting Principal

Compatibility with [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) and [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md):

| Identity | Meaning |
|----------|---------|
| `principal_id` | Actual authenticated / calling AIEOS Principal |
| `effective_actor_id` | Principal on whose behalf the governed action is performed |
| `teacher_principal_id` | Represented / effective HUMAN teacher whose classroom intent is recorded |

Default while delegation is absent:

```text
effective_actor_id = principal_id
```

For ordinary direct Teacher OS assignment:

```text
teacher_principal_id = effective_actor_id = principal_id
```

If a **future separately authorized** delegation mechanism allows `principal_id != effective_actor_id`, then:

- `teacher_principal_id` **MUST** identify the effective / represented teacher
- required security audit provenance **MUST** retain `principal_id`, `effective_actor_id`, and `delegation_id` as applicable

This ADR does **not** authorize delegation implementation and does **not** add delegation fields to the TeachingAssignment aggregate merely for that future case.

### 5. Exact ContentVersion binding

TeachingAssignment binds **immutably** to:

```text
content_id + content_version_id
```

At **CREATE**, server authority **MUST** prove under a transactionally consistent / race-safe Content authority check (or equivalent governed mechanism):

```text
Content.published_version_id == requested content_version_id
```

- **APPROVED but unpublished** → **REJECT**
- Frontend GET observation alone is **never** sufficient business authority
- If Content later receives or publishes another ContentVersion, existing TeachingAssignment **MUST** remain bound to the original exact version (does **not** follow the live published pointer)

### 5A. CREATE authority composition (dual gate)

TeachingAssignment **CREATE** requires **both** server-authoritative gates:

**A. Content eligibility**

```text
Content.published_version_id == requested content_version_id
```

(plus learner-assignable ContentAudience / policy eligibility under §6)

**B. Audience eligibility / current ClassRef authority**

```text
requested class_ref resolves within the exact tenant
  AND is currently an assignable class target
  for the represented / effective teacher
  under current AIEOS authority
```

Both checks are **server authoritative**. Frontend Content GET and School Context GET remain **advisory observations only**.

Implementation must later make both gates race- / freshness-safe according to their own authority boundaries. A stale browser class list **cannot** authorize assignment.
### 6. Artifact eligibility

Baseline **learner-assignable** preparation kinds:

- `worksheet`
- `quiz`
- `homework`

Baseline **teacher-only / non-assignable**:

- `lesson_plan`
- `answer_key`
- `teacher_notes`

Eligibility is **server-side** Educational ContentAudience / policy authority.

This ADR does **not** freeze a permanent closed three-kind universe. Future learner-facing kinds may be added through governed Educational Intelligence / audience metadata.

**Unknown or unclassified audience → FAIL CLOSED.**

Frontend visibility of an Assign button is UX only — never authority.

### 7. Audience / ClassRef

DEV06 baseline:

```text
audience_type = class
class_ref     = one stable opaque School Context class identifier
```

- `class_ref` comes through an **AIEOS School Context read façade / port**
- Class / Roster / Enrollment master authority remains **Admin / ERP / SIS**
- Teacher OS must **not**: create or mutate Class master records; create Roster authority; use `TeachingWork.class_label` as Class identity; call ERP/SIS directly from the browser
- `TeachingWork.class_label` remains presentation / context only
- Learner subset targeting: **DEFERRED**
- Individual learner targeting: **DEFERRED**
- Section-specific semantics: **DEFERRED** unless already represented by the authoritative ClassRef provider

### 8. School Context read boundary and ClassRef create-time current authority

**Prerequisite for DEV06-I01:**

AIEOS School Context **class read façade / port** that returns only classes the current authenticated / current-authority teacher is permitted to assign to.

- Underlying ERP / SIS / provider remains replaceable
- No duplicate Class SoR may be introduced in Teaching
- Frontend calls **AIEOS only**
- This ADR does **not** invent an exact capability identifier unless already frozen elsewhere
- This ADR does **not** choose an ERP / SIS vendor

**Binding CREATE rule:**

`GET` / list School Context classes is **UX / read assistance only**. It is **not** durable authorization to create an assignment.

At TeachingAssignment **CREATE**, server-side **current authority MUST** revalidate the requested `class_ref`:

```text
class_ref resolves within the exact tenant
  + is currently an assignable class target
  for the represented / effective teacher
  under current AIEOS authority
```

If any of the following holds:

- `class_ref` is unknown
- `class_ref` belongs to another tenant
- the represented / effective teacher is no longer authorized for it
- School Context authority is unavailable
- current authority cannot be established

→ **FAIL CLOSED** → **no TeachingAssignment commit**.

A stale browser class list cannot authorize assignment.
### 9. Roster semantics

TeachingAssignment is **ClassRef-scoped**.

- Do **not** persist a roster-member snapshot in the core aggregate
- Do **not** freeze live membership vs historical snapshot membership for learner delivery yet
- That decision is deferred to future Student OS / learner visibility / attempt / submission / delivery-evidence architecture
- Roster changes do **not** mutate TeachingAssignment `class_ref` identity

### 10. Lifecycle

Frozen states:

```text
ACTIVE
CLOSED
CANCELLED
```

Allowed transitions:

```text
ACTIVE → CLOSED
ACTIVE → CANCELLED
```

- `CLOSED` and `CANCELLED` are **terminal** in DEV06 (no reopen to `ACTIVE`)
- Do **not** introduce `DRAFT`
- Do **not** introduce `SCHEDULED` merely because `available_from` is in the future
- A future `available_from` assignment remains **ACTIVE**; effective learner visibility is derived separately
- Due time does **not** automatically close the assignment

### 11. Timing

Conceptual minimum:

| Field | Rule |
|-------|------|
| `assigned_at` | Required; server-controlled |
| `available_from` | Optional; defaults to `assigned_at` |
| `due_at` | Optional; teacher-controlled while `ACTIVE` subject to authorization / concurrency |
| `closed_at` | Set only on close |
| `cancelled_at` | Set only on cancel |

Learner-engagement restrictions are **not** defined in DEV06 core (attempt / engagement authority does not yet exist).

### 12. Idempotency

HTTP / application mutation commands require:

```text
Idempotency-Key
```

Frozen semantics:

| Case | Result |
|------|--------|
| Same `Idempotency-Key` + same canonical material request | Same result / same assignment |
| Same `Idempotency-Key` + materially different request | Idempotency conflict / **FAIL CLOSED** |
| Different `Idempotency-Key` | Separate deliberate business command |

**Explicit rejection:** Do **not** create business uniqueness over:

```text
teacher + content_version + class_ref + due_at
```

Two deliberate assignments **may** have identical business fields when created through distinct valid command identities (`Idempotency-Key`s). This is intentional.

### 13. Concurrency

TeachingAssignment aggregate mutations use:

```text
aggregate_revision + ETag / If-Match optimistic concurrency
```

Stale mutation → **FAIL CLOSED**.

Applies at minimum to: due-date update, close, cancel.

Create publication eligibility must be checked race-safely against authoritative Content publication truth before the assignment commit.

### 14. Events / audit

Assignment mutations participate in existing AIEOS transactional security audit / accountability architecture ([ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md)).

Initial event family (exact namespace / payload formatting may be finalized during implementation contract work consistent with [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md)):

- `teaching.assignment.created.v1`
- `teaching.assignment.due_updated.v1`
- `teaching.assignment.closed.v1`
- `teaching.assignment.cancelled.v1`

Do **not** reuse `content.published.v1` as assignment truth.

Mutation + required event publication intent + required audit intent must follow the existing transactional architecture.

### 15. LMS / external delivery

Long-term boundary:

- **AIEOS owns TeachingAssignment intent**
- External LMS may own a remote mirrored assignment

**DEV06 core does NOT require an LMS.**

Do **not** introduce in DEV06 core:

- `assignment_delivery_attempts`
- delivery provider state
- `external_assignment_ref`
- delivery retry engine

Those require a future connector / delivery architecture slice when an actual LMS integration is prioritized.

LMS outage must **not** prevent creation of a valid native TeachingAssignment in DEV06 core.

---

## Out of scope (explicitly deferred)

- learner subset / individual learner assignment
- roster snapshot policy
- Student OS learner UI
- learner visibility implementation
- attempts / submissions / grading
- notifications / parent delivery
- LMS connector / MCP LMS connector
- delivery retry engine / delivery-attempt persistence
- bulk assignment / kit-level assignment
- production deployment

---

## Scenario validation

| ID | Scenario | Expected behavior |
|----|----------|-------------------|
| A | Published Worksheet exact version → Grade 5B ClassRef | TeachingAssignment `ACTIVE` |
| B | Same version → Grade 5A and Grade 5B | Independent assignments |
| C | Same exact version + same ClassRef with different Idempotency-Keys | Two deliberate assignments allowed |
| D | Retry same canonical command with same Idempotency-Key | No duplicate; same assignment / result |
| E | Same Idempotency-Key + different material command | Conflict / fail closed |
| F | New ContentVersion or later publication | Existing assignment remains bound to original version |
| G | Unpublished exact version | Reject |
| H | Answer Key | Reject |
| I | Future `available_from` | Remains `ACTIVE` (not a `SCHEDULED` lifecycle state) |
| J | Due-date update | Requires current aggregate revision / If-Match |
| K | Cancel | `ACTIVE` → `CANCELLED` (terminal) |
| L | Roster change | Does not mutate TeachingAssignment ClassRef; membership-delivery policy deferred |
| M | LMS unavailable | Native TeachingAssignment can still be created |
| N | Publication changes between client observation and assign command | Server-side authoritative validation prevents stale / unpublished assignment |
| O | Stale / unauthorized ClassRef: teacher loads permitted classes; authority/relationship changes before CREATE; client submits old ClassRef | Server revalidates current ClassRef authority → reject → no assignment commit |
| P | Effective actor compatibility: ordinary direct Teacher OS command has `principal_id = effective_actor_id = teacher_principal_id`; future separately authorized delegated execution may have `principal_id != effective_actor_id` | TeachingAssignment remains owned by the represented teacher; security audit preserves actual / effective / delegation provenance as applicable. Architecture compatibility only — **does not authorize delegation implementation** |

---

## Recommended implementation order

Architecture freeze alone does **not** authorize these implementations:

| Slice | Scope |
|-------|-------|
| **DEV06-I01** | School Context ClassRef read contract / façade |
| **DEV06-I02** | TeachingAssignment domain + PostgreSQL persistence / RLS |
| **DEV06-I03** | Assignment application / API contract |
| **DEV06-I04** | Teacher OS assignment UX |
| **DEV06-I05** | Real product E2E |
| **Future DEV06-D** | External LMS delivery only if product priority warrants |

---

## Consequences

**Positive**

- Clear separation of Publication eligibility vs classroom assignment intent
- Reproducible exact-version binding
- ERP / SIS remain Class / Roster masters; Teacher OS does not become ERP admin UI
- Native assignment works without LMS

**Negative / follow-ups**

- Requires School Context class read façade before Teacher OS Assign UX can ship
- Product lifecycle copy that equates Published with Assigned must be aligned after Founder freeze
- Learner visibility / roster membership policy remains open for Student OS architecture

## Alternatives considered

1. **Treat Publication as classroom delivery truth** — Rejected: contradicts implemented Teacher OS semantics and collapses governance with delivery.
2. **LMS-only assignment SoR** — Rejected for DEV06 core: no LMS connector exists; would block Teacher OS and violate frontend→AIEOS-only boundary if rushed.
3. **Business uniqueness on teacher + version + class + due** — Rejected: conflicts with deliberate repeat assignment via distinct command identities; Idempotency-Key is the retry authority.
4. **Kit-level assignment aggregate** — Rejected: no PreparationKit SoR; assignment is per ContentVersion.

## Compliance

- ADR-046 common lifecycle vocabulary remains binding; Published delivery wording is clarified by this ADR.
- ADR-AIEOS-052 preparation architecture remains binding; “Learner delivery truth | Publication” is clarified to mean eligibility, not assignment.
- ADR-AIEOS-027 Content SoR and Publication remain authoritative for published version pointers.
- Frontend continues to call AIEOS APIs only ([ADR-042](ADR-042-teacher-os-shell-owns-ux.md) / [ADR-044](ADR-044-ai-platform-behind-stable-services.md)).
