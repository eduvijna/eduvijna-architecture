---
id: ADR-AIEOS-056
title: AIEOS Improve & Remediation Authority
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.2
created: 2026-09-04
last_updated: 2026-09-04
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-056 — AIEOS Improve & Remediation Authority

**Status:** Frozen / Approved
**Chief Architect architecture review:** ACCEPTED — 2026-09-04
**Founder / Product Architecture freeze:** APPROVED — 2026-09-04
**Date:** 2026-09-04
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-052](ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) · [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) · [ADR-AIEOS-054](ADR-AIEOS-054-aieos-teaching-execution-observation-authority.md) · [ADR-AIEOS-055](ADR-AIEOS-055-aieos-assessment-learning-evidence-authority.md) · [ADR-045](ADR-045-teaching-intent-owns-goals.md) · [ADR-048](ADR-048-review-queue-owns-approval.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR freezes the **AIEOS Improve & Remediation Authority** for Teacher OS **TOS-DEV09 — Teacher OS Class-Level Improve & Remediation**. Founder / Product Architecture approval was granted **2026-09-04**. Architecture freeze ≠ Backend implementation authorization ≠ Frontend implementation authorization ≠ migration authorization ≠ OpenAPI authorization ≠ NATS provisioning ≠ Temporal authorization ≠ deployment authorization ≠ production mutation authorization. Implementation slices **DEV09-I01+** require separate Chief Architect authorization **after** this architecture PR is merged and closure verified. **TOS-DEV09-I01+ is NOT AUTHORIZED.**

**ID family note:** `ADR-AIEOS-056` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS product ADR-042–048 language decisions.

**Architecture choice (TOS-DEV09A / TOS-DEV09P1):** **OPTION B** — Improve is an **application capability** that creates durable **TeachingWork** using a new governed remediation Teaching Intent type plus an **immutable Teaching-owned remediation-origin relation**. No dedicated Improve / Remediation aggregate SoR. No separate Improve lifecycle.

Does **not** reopen: Generic Content (ADR-AIEOS-027); preparation kit (ADR-AIEOS-052); Review Queue (ADR-048); Publication; TeachingAssignment (ADR-AIEOS-053); TeachingExecution (ADR-AIEOS-054); ClassroomAssessment (ADR-AIEOS-055); ADR-AIEOS-046R1 publisher scope.

Historical ADR-AIEOS-055 body remains unchanged. ADR-AIEOS-055 §17 read-only Improve handoff remains binding and is specialized (not rewritten) by this ADR.

TOS-DEV09A discovery is **COMPLETE / ACCEPTED**. TOS-DEV09P1 design + adversarial validation and TOS-DEV09P1R1 provenance + AI-input correction were deposited as Proposed (v1.0.0 / v1.0.1) before freeze. Founder / Product Architecture approval was granted on **2026-09-04**. This freeze does **not** authorize implementation.

---

## Context

Teacher OS has proven the class-level loop through Prepare → Teach → Assess:

```text
Teaching Intent (prepare_tomorrow)
  → TeachingWork
  → preparation (Generate / Prepare)
  → Generic Content / ContentVersion
  → ReviewDecision
  → Publication
  → TeachingAssignment
  → TeachingExecution (Taught)
  → ClassroomAssessment (Assessed — class-level)
```

`/teacher-os/improve` remains a PlaceholderPage. ADR-AIEOS-055 freezes Assessment ownership and a **read-only** Improve handoff. It does **not** design Improve implementation.

TOS-DEV09A established:

- Class-level Assessment facts are available without learner identity.
- TeachingWork + Content + Review + Publication + Assignment authorities already close Prepare-again after a new Intent.
- IntentType currently permits only `prepare_tomorrow` (domain enum + DB CHECK).
- No Memory / Notification / Improve / remediation SoR exists.
- Development Outcome First / client-demo readiness prioritizes closing the Daily Loop placeholder without learner SoR.

**Repurposing forbidden:**

| Existing aggregate | Must NOT become Improve truth |
|--------------------|-------------------------------|
| **ClassroomAssessment** | Assessed ≠ Improvement required; Assessment must not auto-create remediation |
| **TeachingExecution** | Taught ≠ remediation intent |
| **prepare_tomorrow TeachingWork** | Must not secretly mean remediation |
| **Content / ContentVersion** | Artifact payload ≠ Improve acceptance |
| **Continuous Context / DEV session** | Session ≠ Teacher Memory ≠ remediation SoR |

---

## Decision

### 1. Separation invariants (binding)

Preserve ADR-AIEOS-053 / 054 / 055:

```text
Generated   ≠  Approved
Approved    ≠  Published
Published   ≠  Assigned
Assigned    ≠  Taught
Taught      ≠  Assessed
Assessed    ≠  Mastered
```

**New invariants introduced by this ADR:**

```text
Assessed                    ≠  Improvement required
Improvement suggested       ≠  Improvement accepted
Improvement accepted        ≠  Content generated
Generated remediation       ≠  Approved
Approved remediation        ≠  Published
Published remediation       ≠  Assigned
```

No ClassroomAssessment RECORD may automatically create Improve state, recommendations, TeachingWork, Content, Publication, Assignment, or Mastery.

### 2. Authority table

| Concern | Authority |
|---------|-----------|
| Class-level assessment evidence | **ClassroomAssessment** — Assessment ([ADR-AIEOS-055](ADR-AIEOS-055-aieos-assessment-learning-evidence-authority.md)) |
| Improve / remediation **capability** (application orchestration) | **Teaching application** — creates remediation TeachingWork; does **not** own Assessment |
| Remediation Teaching Intent (transient request) | Inbound command shape only — **not** a durable aggregate |
| Durable remediation preparation container | **TeachingWork** — Teaching (`intent_type = remediate_class`) |
| Immutable Improve initiation provenance | **TeachingWorkRemediationOrigin** — Teaching-owned 1:1 origin relation (this ADR) |
| Artifact / version payload | **Content / ContentVersion** — Content ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)) |
| Teacher approval | **ReviewDecision** — Content governance ([ADR-048](ADR-048-review-queue-owns-approval.md)) |
| Publication eligibility | **Publication** |
| Class assignment intent | **TeachingAssignment** — Teaching ([ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md)) |
| Classroom execution | **TeachingExecution** — Teaching ([ADR-AIEOS-054](ADR-AIEOS-054-aieos-teaching-execution-observation-authority.md)) |
| Class / roster master | **ERP / SIS / Admin School Context** |
| Teacher Memory | **Future** — not this ADR; recommended second baseline after Improve |
| Mastery / learner model | **Future Learner Intelligence** — not this ADR |
| Learner attempt / submission | **Future Student / Learning** per ADR-AIEOS-053 — not this ADR |

### 3. Architecture options evaluated

| Option | Summary | Verdict |
|--------|---------|---------|
| **A** | Dedicated Improve / Remediation aggregate SoR + lifecycle | **Rejected** — no durable business state that TeachingWork + immutable origin cannot own; would invent DRAFT/SUGGESTED/ACCEPTED for UI convenience |
| **B** | Improve as application capability → TeachingWork + new IntentType + immutable provenance | **Selected** |
| **C** | Dedicated ImprovementIntent/RemediationIntent aggregate → later TeachingWork | **Rejected** — Intent remains transient per existing Teaching model (ADR-045 / TeachingWork); second aggregate adds state without invariant gain |
| **D** | Hybrid with Improve SoR owning recommendations | **Rejected** — automatic recommendation SoR violates teacher-deliberate baseline and Assessment≠Improve Required |

**Selected: OPTION B.**

### 4. Why no smaller safe option exists

A pure reuse of `prepare_tomorrow` without a distinct IntentType would falsify intent semantics and the DB CHECK authority. Omitting immutable provenance would fail Assessment CORRECT/VOID adversarial cases (silent rewrite or loss of creation basis). A dedicated Improve SoR is larger, not smaller. Therefore the minimum safe design is:

1. New IntentType `remediate_class`
2. Durable TeachingWork (existing SoR)
3. Immutable Teaching-owned remediation-origin relation pinned to Assessment **revision + `class_result_level` snapshot** at creation
4. Application command with Idempotency-Key
5. Teacher-confirmed `goal_text` as the sole Assessment-derived remediation generator instruction (no live Assessment/Observation reread into model context)

### 5. Improve ownership boundary

**Improve owns:**

- The teacher-deliberate decision to initiate remediation preparation from eligible Assessment facts
- The command that materializes remediation TeachingWork + origin provenance
- UX composition that **reads** Assessment / Execution / Observation facts for display

**Improve does NOT own:**

- Assessment facts (no rewrite / correct / void)
- TeachingExecution facts
- Content / Review / Publication / Assignment facts after Work creation
- Mastery / learner evidence
- Automatic recommendations as durable truth
- Teacher Memory

After TeachingWork creation, the existing Work → Generate → Review → Publish → Assign path owns subsequent lifecycle. Improve does **not** introduce a parallel lifecycle.

### 6. Teaching Intent / TeachingWork decision

#### 6.1 IntentType

DEV09 **requires** a distinct IntentType:

| Identifier | Meaning |
|------------|---------|
| `remediate_class` | Teacher-deliberate **class-level remediation preparation** intent initiated from a RECORDED ClassroomAssessment. Produces a TeachingWork for remediation materials for the assessed class context. Does **not** assert that improvement is required, that mastery failed, or that any learner subgroup exists. |

**Forbidden:** reusing `prepare_tomorrow` to mean remediation.

Implementation note (not authorized by this deposit): domain `IntentType` enum **and** `teaching.works` CHECK constraint must both widen to include `remediate_class` in the same governed Backend change.

#### 6.2 Intent semantics

| Question | Decision |
|----------|----------|
| Who creates the intent? | Effective HUMAN teacher Principal (same represented-teacher semantics as ADR-AIEOS-053) |
| Transient? | **Yes** — Teaching Intent remains request shape only; no `teaching_intents` table |
| What becomes durable? | TeachingWork + TeachingWorkRemediationOrigin |
| Who owns `goal_text`? | Teacher — authoritative generator input after confirmation |
| May teacher edit goal before generation? | **Yes** — confirm/edit at Improve create; may refine on Work after create per existing refine rules (`intent_type` immutable) |
| What may be prefilled (non-authoritative)? | Display-only Assessment / Observation context on screen; optional draft goal suggestions that are **never** persisted or sent to AI until teacher confirms |
| What requires explicit teacher confirmation? | Remediation `goal_text` only — the authoritative Teaching Intent / generator instruction |

### 7. Provenance model

#### 7.1 Aggregate

**Name:** `TeachingWorkRemediationOrigin`  
**Ownership:** Teaching domain (not Assessment; not a separate Improve SoR)  
**Cardinality:** exactly **one** origin row per remediation TeachingWork (`intent_type = remediate_class`)  
**Mutability:** **immutable after insert** (no update API; no cascade rewrite)

#### 7.2 Required immutable fields

| Field | Purpose |
|-------|---------|
| `work_id` | Remediation TeachingWork identity (PK / 1:1) |
| `tenant_id` | Tenant boundary |
| `source_assessment_id` | Which ClassroomAssessment initiated remediation |
| `source_assessment_aggregate_revision` | Exact Assessment revision the teacher acted on |
| `source_class_result_level_snapshot` | Exact `class_result_level` from that same validated revision at create |
| `source_class_ref` | Provenance class at initiation (≠ future assignment target) |
| `source_content_id` | Assessed Content identity |
| `source_content_version_id` | Exact assessed ContentVersion |
| `source_work_id` | Originating TeachingWork if present on Assessment (nullable) |
| `source_execution_id` | Originating TeachingExecution if present (nullable) |
| `source_assignment_id` | Originating TeachingAssignment if present (nullable) |
| `initiating_teacher_principal_id` | Teacher who deliberately chose Improve |
| `created_at` | Initiation timestamp |

**`source_class_result_level_snapshot` semantics:**

- Captured from the **same** live Assessment revision validated by `expected_assessment_aggregate_revision`
- Immutable provenance snapshot of what class-level result the teacher acted upon
- **NOT** new Assessment authority
- **NOT** current Assessment truth after later CORRECT
- **NOT** Mastery
- **NOT** a recommendation
- Never rewritten after Assessment CORRECT
- Remains preserved after Assessment VOID

**DEV09 baseline excludes** from Teaching provenance:

- `class_result_note` copy
- CLASS_OBSERVATION body copy
- `include_class_result_note_in_goal_context`
- `include_selected_observation_ids`

Teacher UX **MAY display** current authorized `class_result_note` / CLASS_OBSERVATION before Work creation. The teacher may use human judgment from those displayed facts when writing `goal_text`. Direct model consumption of note/observation body requires a future separately governed immutable input-snapshot design — **not reserved silently in this baseline**.

No generic ungoverned stringly provenance bag. No mutable origin. No Improve lifecycle.

#### 7.3 Create atomicity

Successful create **atomically commits** in one transaction:

1. `TeachingWork` with `intent_type = remediate_class`
2. `TeachingWorkRemediationOrigin` from the **same** validated Assessment snapshot

Forbidden residual states:

- remediation TeachingWork without its required origin
- origin without its TeachingWork

Idempotent replay returns the **same** committed Work/origin outcome and **must not** create a second origin or orphan Work.

#### 7.4 Cross-domain rule

Assessment remains Assessment-owned. Origin stores identifiers + revision + `class_result_level` snapshot only. Improve / Teaching **must not** rewrite Assessment rows when reading origin. UI may compose “source Assessment later CORRECTED / VOIDED” by comparing live Assessment state to pinned revision/snapshot — **without mutating TeachingWork or origin**. The snapshot is historical creation-basis provenance, not current Assessment authority.

### 8. Assessment revision / correction / void semantics

| Case | Required behavior |
|------|-------------------|
| **A — CORRECT after remediation** | Existing remediation TeachingWork + origin **MUST NOT** silently rewrite. Origin retains creation revision **and** `source_class_result_level_snapshot`. UI may show source-superseded advisory. |
| **B — VOID after remediation** | VOID **MUST NOT** cascade-delete / archive / cancel remediation TeachingWork. Origin remains unchanged historical provenance. UI may show source-invalidated advisory. Teacher retains Work control under existing Teaching rules. |
| **C — VOID before Improve** | VOIDED Assessment is **not eligible** to initiate new remediation work. Create command **FAIL CLOSED**. |
| **D — Multiple Improve from same Assessment** | **Allowed**. No business uniqueness on `assessment_id`. Replay protection via Idempotency-Key only. |

**CORRECT example (binding):**

```text
Assessment A revision 3: class_result_level = MIXED
  → teacher creates remediation Work
  → origin freezes:
       source_assessment_id = A
       source_assessment_aggregate_revision = 3
       source_class_result_level_snapshot = MIXED
  → Assessment CORRECT → revision 4: class_result_level = DEMONSTRATED

Required:
  origin remains revision 3 / MIXED
  TeachingWork unchanged
  live Assessment = revision 4 / DEMONSTRATED
  UI may note source Assessment subsequently changed
  do NOT rewrite origin to DEMONSTRATED
  do NOT claim snapshot is current Assessment authority
```

**Eligibility for create:** `lifecycle_state = RECORDED` only. `DEMONSTRATED` / `MIXED` / `NOT_YET_DEMONSTRATED` are all eligible — Assessed ≠ Improvement required; teacher may still choose Improve.

### 9. Teacher-deliberate AI boundary

```text
Assessment / Observation facts may INFORM the screen
  → teacher chooses Improve
  → teacher owns/confirms remediation goal_text
  → TeachingWork + origin created atomically
  → generation uses teacher-confirmed goal_text as THE authoritative remediation generator instruction
     (+ ordinary existing TeachingWork generator context already governed)
```

| Input | Baseline use |
|-------|----------------|
| Live `class_result_level` | May be **displayed** while teacher forms the goal. Snapshot stored on origin for provenance. **MUST NOT** be dynamically reread into model context after Work creation to change remediation intent. |
| `class_result_note` | May be **displayed** before create. **MUST NOT** be copied into Teaching provenance. **MUST NOT** be forwarded to the model in DEV09 baseline. |
| CLASS_OBSERVATION body | May be **displayed** before create. **MUST NOT** be copied into Teaching provenance. **MUST NOT** be forwarded to the model in DEV09 baseline. |
| PRIVATE_EXECUTION_NOTE | **Out of Improve baseline** (teacher-private; not remediation input) |
| Teacher-confirmed `goal_text` | **THE authoritative** remediation generator instruction |

Remove prior wording that allowed `class_result_level` injection into generation context merely “after Work exists”. Mutable Assessment facts **MUST NOT** be read later at generation time in a way that changes the teacher's creation basis.

**No automatic AI remediation** on Assessment RECORD / CORRECT / VOID / list.

### 10. Class-level only

DEV09 baseline **MUST NOT** require or invent:

`learner_id`, `student_id`, roster, attempt, response, submission, score, grade, learner group, student profile, mastery, competency attainment, personalized learning state, fake counts such as “12 students” / “G1/G2”.

Permitted class-level context for UX / create: `class_ref`, displayed `class_result_level` / note / CLASS_OBSERVATION (display-only), teacher-confirmed `goal_text`, exact ContentVersion, TeachingWork context. Durable Teaching remediation input remains `goal_text` + ordinary Work generator context; durable provenance remains origin fields only.

### 11. ClassRef semantics

| Concern | Rule |
|---------|------|
| Provenance `source_class_ref` | Immutable on origin; answers “which class was assessed when Improve started” |
| Create-time ClassRef authority | FAIL CLOSED if current School Context authority for `source_class_ref` is unavailable/stale/unauthorized for the initiating teacher (same fail-closed posture as Assignment/Execution/Assessment mutations) |
| Future TeachingAssignment target | **May** deliberately target another authorized class; assignment remains governed by ADR-AIEOS-053. Provenance class ≠ assignment target. |
| Default UX | Prefill assignment suggestion to `source_class_ref`; teacher may change under assignment authorization |

### 12. Improve lifecycle

**None.** TeachingWork lifecycle + Content Review / Publication / Assignment lifecycles already satisfy durable business needs. Do **not** introduce DRAFT / SUGGESTED / ACCEPTED / GENERATING / COMPLETED Improve states.

“Improvement accepted” in the invariant table means: teacher confirmed goal and created remediation TeachingWork — recorded by Work existence + origin, not a separate status enum.

### 13. API / command model (design only — not implemented)

#### 13.1 Create remediation TeachingWork

Conceptual command:

```text
POST /api/v1/teaching/works/from-classroom-assessment
Idempotency-Key: <required>
```

Preferred over `/improvements` because no Improve aggregate exists.

**Request (conceptual):**

- `assessment_id` (required)
- `goal_text` (required; teacher-confirmed; non-empty)
- `target_date`, `locale` (as existing Work create)
- optional `class_label` / `subject` / `topic` (teacher-editable context; not ClassRef SoR)
- `expected_assessment_aggregate_revision` (required) — FAIL CLOSED if live Assessment revision differs (optimistic create basis)

**Success:** atomically creates TeachingWork (`intent_type=remediate_class`) **and** immutable origin (including `source_class_result_level_snapshot` from the validated Assessment); returns Work identity for navigation to existing Work UX.

#### 13.2 Eligibility listing (composition)

Improve hub may list recent RECORDED ClassroomAssessments for the effective teacher via **existing** Assessment LIST composition (`teacher_principal_id` + `lifecycle_state=RECORDED` filters). No new Improve SoR required.

#### 13.3 Frozen command semantics

| Concern | Rule |
|---------|------|
| Tenant boundary | From authenticated tenant only; cross-tenant Assessment → FAIL CLOSED |
| Teacher authority | Effective HUMAN teacher Principal; Assessment `teacher_principal_id` must match initiating teacher → else FAIL CLOSED |
| ClassRef authorization | Current assignable authority for provenance class_ref → else FAIL CLOSED |
| Assessment eligibility | Must exist, same tenant, RECORDED, revision match → else FAIL CLOSED |
| Idempotency-Key | Required; same key + same fingerprint → replay stored outcome; same key + different fingerprint → conflict |
| Audit | Required security audit on successful create (Teaching vocabulary; exact codes deferred to implementation design under ADR-AIEOS-028) |
| Authorization unavailable | **FAIL CLOSED** |
| Browser-owned authority | **Forbidden** |

### 14. Idempotency / concurrency

- Create uses platform Idempotency-Key scope consistent with TeachingWork create.
- No uniqueness on `(teacher, assessment_id)`.
- Create must validate Assessment `lifecycle_state = RECORDED` **and** live `aggregate_revision = expected_assessment_aggregate_revision`, then **atomically snapshot** live `class_result_level` into `source_class_result_level_snapshot` before commit.
- If revision changed: **FAIL CLOSED**. Teacher must reload/review current Assessment and deliberately retry with a new command / Idempotency-Key as appropriate. Do **not** silently adopt the new Assessment revision.
- Idempotent replay returns the same committed Work/origin outcome and cannot create a second origin or orphan Work.
- TeachingWork refine / generate concurrency remains existing Work/Content rules (If-Match / ETag where already established). Generation after create uses teacher-confirmed `goal_text` + ordinary Work generator context — it does **not** dynamically reread Assessment result/note or Observation body as remediation intent.

### 15. Content / Review / Publication / Assignment reuse

Preferred flow (binding):

```text
RECORDED ClassroomAssessment
  → teacher explicitly chooses Improve
  → teacher confirms remediation goal
  → durable remediate_class TeachingWork + immutable origin
  → existing generation capability
  → ContentVersion / Review Queue
  → teacher Review / Approve
  → Publication
  → TeachingAssignment
  → future Teach cycle
```

Improve **MUST NOT** bypass Review Queue, auto-publish, or auto-assign.

### 16. Teacher Memory boundary

Teacher Memory is **out of TOS-DEV09**. No Memory SoR, learn-from-edit persistence, write-back, inferred preferences, or Memory confirmation UX. Absence of Memory **must not** block this baseline. Compatible with a later Memory baseline.

### 17. Events / Temporal

**DEV09 baseline:**

- **NO** new NATS Improve event family
- **NO** Temporal Improve workflow
- **NO** ADR-AIEOS-046R1 publisher expansion for Improve

Existing Content / Teaching event families remain as previously authorized. Client-demo convenience is insufficient to add events/workflows.

### 18. Product UX flow (minimal)

```text
/teacher-os/assess
  → RECORDED ClassroomAssessment
  → "Improve this class"
  → /teacher-os/improve (or dedicated create step)
  → show source Assessment context (read-only)
  → teacher enters/confirms remediation goal
  → POST works/from-classroom-assessment
  → navigate to existing /teacher-os/work/:workId
  → generate / Review / Publish / Assign
```

Also: `/teacher-os/improve` hub listing eligible recent RECORDED assessments via Assessment LIST composition.

No learner heatmaps. No fake student groups.

### 19. Real-stack Product E2E target

Future Product E2E (after freeze + implementation authorization):

```text
Browser → Vite /api → FastAPI → real app/domain/repository → disposable PostgreSQL
```

Zero `/api` mocks for the Improve create + Work path.

Story: Prepare → Publish → Assign → Teach → complete Execution → Record ClassroomAssessment → Improve → create remediation TeachingWork → prepare/generate remediation artifact → Review → Approve → Publish → Assign.

If live AI makes deterministic E2E fragile, use the existing governed development / FakeStructuredModelGateway boundary for generation only — **without** weakening real HTTP + PostgreSQL verification of Improve create, origin persistence, Review, Publication, and Assignment.

### 20. Explicit non-goals

- Learner identity / roster / attempts / submissions / grades / mastery
- Automatic Improve recommendations as durable authority
- Parent drafts / PTM / remarks communication
- Teacher Memory
- Notification Center
- AI Assistant expansion
- Assess Analyze heatmaps
- Library product surface
- Production NATS / Temporal / DigitalOcean / GA hardening
- Rewriting ADR-AIEOS-055 Assessment semantics

### 21. Implementation slice sketch (not authorized)

| Slice | Scope (illustrative only) |
|-------|---------------------------|
| **DEV09-I01** | IntentType + origin persistence + domain create |
| **DEV09-I02** | Application/API create + eligibility composition + audit/idempotency |
| **DEV09-I03** | Teacher OS Improve UX (replace PlaceholderPage) |
| **DEV09-I04** | Real-stack Product E2E |

**Not authorized by this ADR deposit.**

---

## Consequences

### Positive

- Closes Daily Loop with least new durable state
- Preserves Assessment / Teaching ownership boundaries
- Survives Assessment CORRECT / VOID without cascade corruption
- Reuses proven Content → Review → Publish → Assign path
- Keeps learner / mastery / Memory boundaries intact for future phases

### Negative / accepted costs

- Requires IntentType + CHECK widening migration when implemented
- Requires new origin table/relation when implemented
- Teachers may create multiple remediation Works from one Assessment (by design)
- Source-invalidated advisories need UX composition after CORRECT/VOID

### Risks if violated

- Secret `prepare_tomorrow` remediation → false Intent analytics and irreversible semantic confusion
- Missing revision + result snapshot → creation basis not explainable after CORRECT (ADR-AIEOS-055 has no Assessment business-history SoR)
- Cascade cancel on VOID → destroys teacher work without consent
- Dynamically rereading live Assessment/Observation into generation → changes teacher creation basis behind their back
- Auto-forwarding / duplicating `class_result_note` or Observation body into Teaching storage → ungoverned AI/PII/retention scope
- Improve SoR / fake learner groups → premature Student Intelligence coupling

---

## Adversarial validation (TOS-DEV09P1 / TOS-DEV09P1R1)

| # | Scenario | Result |
|---|----------|--------|
| 1 | VOIDED Assessment cannot initiate new Improve work | **PASS** — eligibility RECORDED only; FAIL CLOSED |
| 2 | Assessment CORRECT after remediation does not mutate Work | **PASS** — immutable origin + pinned revision + result snapshot |
| 3 | Assessment VOID after remediation does not cascade-delete Work | **PASS** — no cascade; advisory UI only |
| 4 | Same Assessment may produce multiple remediation Works | **PASS** — no assessment_id uniqueness |
| 5 | Same Idempotency-Key + same request replays | **PASS** — platform idempotency replay of same Work/origin |
| 6 | Same Idempotency-Key + different request conflicts | **PASS** — fingerprint conflict |
| 7 | Cross-tenant Assessment reference fails closed | **PASS** |
| 8 | Different teacher Assessment reference fails closed | **PASS** |
| 9 | Unauthorized/stale ClassRef fails closed | **PASS** — current ClassRef authority gate |
| 10 | Assessment note not automatically sent to AI | **PASS** — note display-only; generation uses goal_text only |
| 11 | No learner identity introduced | **PASS** |
| 12 | No Mastery claim introduced | **PASS** — Assessed ≠ Mastered; Assessed ≠ Improvement required |
| 13 | Review Queue cannot be bypassed | **PASS** — existing Content path only |
| 14 | Publication not implied by generation | **PASS** |
| 15 | Assignment not implied by publication | **PASS** — ADR-AIEOS-053 |
| 16 | Teacher Memory absence does not block baseline | **PASS** |
| 17 | Improve does not rewrite Assessment | **PASS** |
| 18 | Improve does not modify TeachingExecution | **PASS** |
| 19 | No new NATS/Temporal requirement for baseline | **PASS** |
| 20 | Source provenance remains durable and explainable | **PASS** — TeachingWorkRemediationOrigin with revision + result snapshot |
| 21 | CORRECT preserves exact `source_class_result_level_snapshot` | **PASS** — MIXED at rev 3 remains MIXED after rev 4 DEMONSTRATED |
| 22 | Revision number + result snapshot mutually consistent at create | **PASS** — both taken from same validated Assessment revision |
| 23 | Generation after Work creation does not reread corrected Assessment result as remediation intent | **PASS** — goal_text + ordinary Work generator context only |
| 24 | `class_result_note` is not copied into Teaching provenance | **PASS** |
| 25 | CLASS_OBSERVATION body is not copied into Teaching provenance | **PASS** |
| 26 | Teacher-confirmed `goal_text` is sufficient generator intent | **PASS** |
| 27 | TeachingWork + origin creation is atomic | **PASS** — no orphan Work or orphan origin |
| 28 | Idempotent replay cannot create a second origin or orphan Work | **PASS** |

**CURRENT — INVALID scenario failures: 0**

---

## Authorization boundary

This ADR is **Frozen / Approved**.

Frozen / Approved ≠ implementation authorization. **TOS-DEV09-I01+ is NOT AUTHORIZED.** Implementation requires a separate Chief Architect governed package after this architecture PR is merged and closure verified.

Does **not** authorize: Backend, Frontend, Product, migration, OpenAPI, NATS, Temporal, Teacher Memory, learner Assessment, mastery, production deployment, or DigitalOcean mutation.

---

## Founder decision

Founder / Product Architecture **APPROVED** OPTION B class-level Improve & Remediation Authority on **2026-09-04**.

Chief Architect architecture review: **ACCEPTED — 2026-09-04**.

This freeze does **not** authorize Backend, Frontend, Product, migration, OpenAPI, DEV09-I01+, Teacher Memory, learner groups, mastery, NATS Improve events, Temporal, or production deployment.
