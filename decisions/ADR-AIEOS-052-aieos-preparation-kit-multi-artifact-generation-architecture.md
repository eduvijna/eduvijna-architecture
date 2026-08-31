---
id: ADR-AIEOS-052
title: AIEOS Preparation Kit & Multi-Artifact Generation Architecture
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.1
created: 2026-08-28
last_updated: 2026-08-28
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-052 — AIEOS Preparation Kit & Multi-Artifact Generation Architecture

**Status:** Frozen / Approved  
**Date:** 2026-08-28  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-044](ADR-044-ai-platform-behind-stable-services.md) · [ADR-045](ADR-045-teaching-intent-owns-goals.md) · [ADR-046](ADR-046-artifact-status-lifecycle.md) · [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) · [ADR-048](ADR-048-review-queue-owns-approval.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR freezes the **AIEOS Preparation Kit & Multi-Artifact Generation Architecture** for Teacher OS **TOS-DEV04 — Prepare Tomorrow Depth**. Founder / Product Architecture approval was granted **2026-08-28**. Architecture freeze **does not** authorize Backend implementation, Frontend implementation, database migration, OpenAPI change, live provider call, or production Content catalog activation. Implementation slices require separate Chief Architect authorization gates per slice — including **DEV04-I10** live provider proof.

**ID family note:** `ADR-AIEOS-052` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is **distinct** from Teacher OS **ADR-047** (Outcome-first Prepare Tomorrow). Teacher OS ADR-047 expresses product language and outcome intent; this ADR freezes platform execution, Content, provenance, and API boundaries for multi-artifact preparation. Platform **ADR-AIEOS-047** (Production Workflow Plane Identity) is also a distinct decision.

**Supersedes / unlocks:** Teacher OS ADR-047 deferred multi-artifact Prepare Tomorrow orchestration pending architecture review. This ADR **unlocks** that deferred orchestration architecture for the TOS-DEV04 development slice — **not** for production deployment or implementation by architecture freeze alone.

---

## Context

TOS-DEV03 established a proven path:

```text
Teaching Work → education.generate_worksheet → GenerationRun → one Worksheet ContentVersion → IN_REVIEW → Review Queue
```

Teacher OS product architecture ([ADR-047](ADR-047-outcome-first-prepare-tomorrow.md)) requires **Prepare Tomorrow** to produce **one coherent preparation kit** — not six independent generator products exposed to teachers.

The baseline DEV04 kit comprises six governed artifacts:

1. `lesson_plan`
2. `worksheet`
3. `quiz`
4. `homework`
5. `answer_key`
6. `teacher_notes`

Current platform evidence (TOS-DEV03 merged source) exposes a structural mismatch:

| Layer | Current shape | DEV04 need |
|-------|---------------|------------|
| `GenerationRun` result | Singular `result_content_id` / `result_version_id` | Six governed results per preparation execution |
| `POST …/actions/generate` response | Singular `artifact` | Six artifacts or kit envelope |
| `GET …/artifacts` | Plural list, singular backing | All kit artifacts for a preparation run |
| Content provenance uniqueness | At most one AI version per `generation_run_id` | At most one AI version per `generation_run_id` + `artifact_kind` |
| GenerationRun work fence (DEV03) | At most one `RUNNING`/`SUCCEEDED` run per `work_resource_id` | Capability/revision-aware fences (see §7) |

The merged DEV03 migration establishes conceptually:

```text
UNIQUE(tenant_id, work_resource_id)
WHERE status IN ('RUNNING', 'SUCCEEDED')
```

That fence prevents a successful `education.generate_worksheet` run from coexisting with a later `education.generate_preparation_kit` run for the same Teaching Work — violating additive DEV03/DEV04 compatibility. DEV04 requires explicit semantic evolution of this fence (§7).

Generic Content ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)) remains the sole authoritative artifact payload and version System of Record. Workflow history ([ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md)) is not Content authority. Provider responses are not business authority.

This ADR freezes how **one preparation execution** produces **many governed ContentVersions** without introducing a second Content SoR, without durable full-kit payload staging in the AI schema, and without a `PreparationKit` business aggregate in DEV04.

---

## Decision

### 1. Product outcome — one Prepare action

Teachers operate **one outcome action** for baseline DEV04 preparation:

```text
Create preparation kit
```

or product-equivalent outcome language such as **Prepare tomorrow**.

The frontend must **not** expose six independent generator products as the primary Prepare Tomorrow mental model.

The canonical DEV04 flow:

```text
Teaching Intent
  ↓
Teaching Work
  ↓
Prepare Tomorrow
  ↓
education.generate_preparation_kit
  ↓
ONE provider-neutral structured generation
  ↓
provider-independent schema + Educational Quality validation
  ↓
ONE atomic Generic Content materialization transaction
  ↓
six complete immutable governed ContentVersions
  ↓
six IN_REVIEW admissions
  ↓
Teacher Review Queue
```

### 2. Authority model

| Concern | Authority |
|---------|-----------|
| Durable teacher preparation container | **Teaching Work** |
| One preparation execution / provenance identity | **GenerationRun** |
| Artifact payload, version, stewardship | **Generic Content / ContentVersion** ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)) |
| Teacher judgement (approve / reject / request changes) | **ReviewDecision** — exact-version scoped ([ADR-048](ADR-048-review-queue-owns-approval.md)) |
| Learner delivery truth | **Publication** — separate, explicit |
| Review Queue listing | Derived projection over governed Content — not a competing SoR |

> **Clarified by [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md):** “Learner delivery truth | Publication” means **eligibility for downstream distribution** via the official published ContentVersion pointer — **not** classroom assignment or learner receipt. Classroom assignment intent is owned by **TeachingAssignment** under ADR-AIEOS-053. DEV04 preparation architecture, Content authority, and Review Queue authority are unchanged.

**Explicit rejections for DEV04 baseline:**

- No **PreparationKit** business aggregate.
- No **AI Content** competing SoR.
- No **workflow history** as approval truth.
- No **provider response** as business authority.
- No **`ai.generation_validated_outputs`** or equivalent durable full-kit JSON staging in the AI schema.
- No **`ai.generation_artifacts`** table as a canonical result bridge or second Content index.

### 3. No AI result payload staging

DEV04 baseline **rejects** durable storage of complete generated educational payloads in the AI execution schema, including patterns such as:

```text
ai.generation_validated_outputs
```

or equivalent full-kit JSON staging on `GenerationRun`.

**Reason:** Generic Content is the sole authoritative Content payload and version SoR. `GenerationRun` continues to hold minimized execution and provenance metadata only — not complete generated educational payloads.

If a future asynchronous architecture genuinely requires durable intermediate typed outputs, that requires a **separate architecture decision** defining authority, retention, and lifecycle.

### 4. No generation_artifacts table in DEV04 baseline

DEV04 baseline **does not introduce**:

```text
ai.generation_artifacts
```

as the canonical result bridge.

**Reason:** The authoritative `ContentVersion` already carries mandatory AI generation provenance. The binding:

```text
ONE GenerationRun → MANY ContentVersions
```

is represented through typed **Content provenance**, not a duplicated result index.

A future **derived read-model / projection** may be introduced if query performance requires it. Any such structure must remain **derived and non-authoritative**.

### 5. AIGenerationProvenanceV2

Introduce **AIGenerationProvenanceV2** as an extension of the frozen provenance model ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) GCI provenance rules).

**V1 remains readable and valid.** Existing DEV03 ContentVersions with V1 provenance MUST continue to parse and satisfy uniqueness invariants.

**V2** extends the existing allow-listed provenance object with one mandatory additional field:

| Field | Requirement |
|-------|-------------|
| `artifact_kind` | Stable lowercase identifier |

DEV04 expected `artifact_kind` values:

```text
lesson_plan
worksheet
quiz
homework
answer_key
teacher_notes
```

Every DEV04 AI-origin `ContentVersion` MUST carry:

- `generation_run_ref`
- `provider_id`
- `model_id`
- `capability_id`
- `source_refs` (including the exact Teaching Work revision)
- `correlation_id`
- `artifact_kind`
- all other frozen provenance fields applicable to V1/V2

All six ContentVersions from one preparation execution reference:

- the **same exact** `GenerationRun` `ResourceRef`, and
- the **same exact** Teaching Work revision in `source_refs`.

**Forbidden in provenance:** raw prompt, raw provider output, credential, chain-of-thought.

Implementation MUST define V2 as a strict allow-listed extension — not an open JSON bag.

### 6. Database uniqueness evolution

Current DEV03 migration establishes:

```text
uq_content_versions_ai_generation_run_id
```

which enforces at most **one** AI `ContentVersion` per:

```text
tenant_id + generation_run_id
```

DEV04 requires a **backward-compatible replacement** of this semantic contract.

| Provenance version | Uniqueness invariant |
|--------------------|----------------------|
| **V1** | At most one AI ContentVersion per `tenant_id + generation_run_id` |
| **V2** | At most one AI ContentVersion per `tenant_id + generation_run_id + artifact_kind` |

Existing DEV03 V1 ContentVersions and V1 parsing MUST remain valid.

Exact SQL/index text is an implementation concern. The **semantic uniqueness contract** above is architecture-binding.

### 7. GenerationRun work-execution fence evolution

DEV03 establishes `uq_ai_generation_runs_work_active_or_succeeded`, which conceptually enforces at most one `RUNNING` or `SUCCEEDED` `GenerationRun` per `tenant_id + work_resource_id` regardless of capability or Work revision.

DEV04 requires **replacement/evolution** of that semantic fence with **two** architecture invariants. Exact PostgreSQL index names and SQL are implementation concerns. The migration MUST preserve existing DEV03 `GenerationRun` history.

#### Fence A — Work revision + capability outcome fence

For a given:

```text
tenant
Teaching Work (work_resource_id)
exact Work revision (work_resource_revision)
capability_id
```

there may be at most **one** `GenerationRun` in `RUNNING` **or** `SUCCEEDED`.

Conceptual invariant:

```text
UNIQUE(
  tenant_id,
  work_resource_id,
  work_resource_revision,
  capability_id
)
WHERE status IN ('RUNNING', 'SUCCEEDED')
```

This ensures:

- the same preparation capability cannot create duplicate kit outcomes for one Work revision;
- a `SUCCEEDED` preparation blocks another preparation for that same Work revision and capability;
- a later refined Work revision MAY produce a new preparation outcome;
- provider/model changes do not create a parallel business outcome for the same capability/revision (`provider_id` and `model_id` remain fingerprint/provenance dimensions only).

#### Fence B — Single active execution per Work + capability

For a given:

```text
tenant
Teaching Work (work_resource_id)
capability_id
```

there may be at most **one** `RUNNING` execution **across Work revisions**.

Conceptual invariant:

```text
UNIQUE(
  tenant_id,
  work_resource_id,
  capability_id
)
WHERE status = 'RUNNING'
```

**Reason:** If Work revision R0 is being prepared and the teacher refines the Work to R1 during the provider call, do **not** permit a second simultaneous preparation provider execution for R1. The first execution remains bound to its exact claimed Work revision R0. Once that execution becomes terminal or is recovered, a later revision MAY prepare independently.

### 8. DEV03 and DEV04 capability coexistence

Explicitly:

```text
education.generate_worksheet
```

and:

```text
education.generate_preparation_kit
```

are **distinct capabilities**.

Therefore they **MAY** each have a successful `GenerationRun` for the same Teaching Work **and** the same Work revision, because the `capability_id` values differ.

This is **required** for additive DEV03 compatibility.

Do **not** treat `provider_id` or `model_id` as the business outcome partition. They MUST NOT permit two successful preparation kits for the same Work + revision + preparation capability.

### 9. Work refinement during preparation

**Scenario:** Preparation run A claims Work revision R0 and is `RUNNING`. The teacher refines the Teaching Work to revision R1.

Required semantics:

1. Run A remains bound to R0.
2. If A succeeds, its provenance references the exact Work revision R0 in `source_refs`.
3. While A remains `RUNNING`, another `education.generate_preparation_kit` execution for the same Work is blocked by Fence B (single active execution per Work + capability).
4. After A becomes terminal:
   - if A `SUCCEEDED` for R0, R1 MAY subsequently have its own preparation execution (Fence A is revision-scoped);
   - if A `FAILED`, R1 MAY subsequently execute;
   - if A is stale `RUNNING`, normal lease recovery resolves A before another active preparation execution proceeds.
5. Do **not** rewrite or silently rebind A from R0 to R1.
6. Work artifact projections MUST preserve exact source revision provenance so historical artifacts cannot masquerade as artifacts generated from a newer revision.

Frontend stale-artifact UX is out of scope for this ADR.

### 10. GenerationRun compatibility

`GenerationRun` is **not** redesigned into a Content aggregate.

Durable statuses remain:

```text
RUNNING → SUCCEEDED | FAILED
```

`VALIDATED` remains compatibility-only per existing DEV03 architecture; ordinary paths must not leave durable `VALIDATED` rows.

Existing singular fields:

```text
result_content_id
result_version_id
result_content_revision
```

remain compatible with DEV03 worksheet generation.

**DEV04 preparation runs MUST NOT** populate legacy singular result fields with a representative artifact (for example worksheet-only or lesson-plan-only IDs).

**Preferred DEV04 behavior:** legacy singular result fields are **NULL** for multi-artifact preparation runs. The authoritative result set is resolved by querying ContentVersions bound to the `GenerationRun` through provenance V2.

Future deprecation or removal of singular fields is **not** part of DEV04.

### 11. Atomic materialization — binding DEV04 decision

After the model returns and **all** typed schema validation and Educational Quality validation pass:

materialize the entire six-artifact kit in **ONE Content Unit of Work / ONE PostgreSQL transaction**.

Within that single transaction:

1. create six Content aggregates;
2. create six immutable AI ContentVersions;
3. persist mandatory V2 provenance on each version;
4. submit all six exact versions for review (`GENERATED → IN_REVIEW`);
5. write required audit / outbox intents according to existing contracts;
6. commit once.

If **any** of the six materializations or review admissions fails:

**ROLL BACK THE ENTIRE CONTENT TRANSACTION.**

Result: **zero** DEV04 kit Content artifacts committed.

DEV04 baseline does **not** leave 1/6, 3/6, or 5/6 normal preparation results.

This is intentionally stricter and simpler than partial-kit recovery alternatives.

Per-artifact invalid partial `ContentVersion` creation remains **forbidden** ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)).

### 12. Kit state (derived — no durable PARTIAL)

Because Content materialization is atomic, DEV04 baseline kit states are **conceptual** and **derived**:

| State | Derivation |
|-------|------------|
| `NONE` | No preparation `GenerationRun` for the Work revision, or only failed runs with zero kit Content |
| `PREPARING` | `GenerationRun` status = `RUNNING` |
| `READY` | `GenerationRun` status = `SUCCEEDED` **and** the complete six-artifact ContentVersion set exists |
| `FAILED` | `GenerationRun` status = `FAILED` |

DEV04 baseline does **not** introduce durable:

```text
PARTIAL
PARTIALLY_READY
```

DEV04 baseline does **not** add a new `GenerationRun` status.

### 13. Failure and recovery semantics

`GenerationRun` in `FAILED` is **terminal**. It is **not** stale-reclaimed. Once a run is durably marked `FAILED`, same idempotency key + same fingerprint replays the same durable failure with **zero** provider calls and **zero** new Content — preserving DEV03 idempotency expectations.

Stale reclaim applies **only** to `RUNNING` runs with an **expired lease**. It is **not** the same as retrying a `FAILED` `GenerationRun`.

#### Terminal failure scenarios

For provider unavailable, request rejected, model output incomplete/missing/invalid, Educational Quality failure, or Content batch failure:

| Outcome | Provider re-call on same key |
|---------|------------------------------|
| `GenerationRun` → `FAILED`; zero kit ContentVersions | **No** — same key replays failure |

#### Stale RUNNING reclaim

A stale `RUNNING` run with **zero** committed DEV04 ContentVersions MAY be reclaimed (lease expired). Because ADR-052 rejects durable generated-payload staging, the reclaimed execution **MAY** need to call the provider again. That is acceptable and is **not** a `FAILED`-run retry.

#### Post-Content-commit crash (provider-free recovery)

If all six ContentVersions and review admissions committed atomically, but the process crashes before `GenerationRun` finalization:

- `GenerationRun` may still be stale `RUNNING`.
- Recovery MUST query Content provenance V2 by `generation_run_id`.
- Recovery MUST require the exact six `artifact_kind` values as the authoritative committed set.
- Recovery MUST finalize the **same** `GenerationRun` as `SUCCEEDED`.
- Recovery MUST return the same six IDs with **zero** provider calls.
- No new result bridge table. No AI payload staging.

#### Retry after FAILED

A genuinely **new** retry after `FAILED` requires a **new** idempotency key. Because `FAILED` releases the Fence A active/succeeded slot, a new run MAY be created subject to normal authorization and concurrency rules — provided:

- no `RUNNING` run exists for the Work + capability (Fence B);
- no `SUCCEEDED` run already exists for the exact Work revision + capability (Fence A).

Do **not** invent automatic retry loops. Do **not** automatically call the provider from a replay of a `FAILED` key.

#### Scenario reference table

| Scenario | Durable outcome | Same-key replay | New-key after FAILED |
|----------|-----------------|-----------------|----------------------|
| Provider / model failure | `FAILED`; zero Content | Replay failure; zero provider | New attempt permitted if fences allow |
| Structured output invalid | `FAILED`; zero Content | Replay failure; zero provider | New attempt permitted if fences allow |
| EQ failure | `FAILED`; zero Content | Replay failure; zero provider | New attempt permitted if fences allow |
| Content batch failure | Rollback; `FAILED`; zero Content | Replay failure; zero provider | New attempt permitted if fences allow |
| Crash before Content commit | Stale `RUNNING`; zero Content | Stale reclaim MAY re-call provider | N/A until terminal |
| Content committed; crash before finalize | Six Content exist; stale `RUNNING` | Reconcile → `SUCCEEDED`; zero provider | N/A |
| HTTP response lost after success | `SUCCEEDED` | Same six IDs; zero provider | Blocked if Fence A succeeded |

### 14. Idempotency and concurrency

Retain DEV03 fingerprint concepts.

Preparation request fingerprint includes at minimum:

```text
work_id
work_revision
capability_id = education.generate_preparation_kit
provider_id
model_id
```

#### Idempotency matrix

| Condition | Behavior | Provider call | New Content |
|-----------|----------|---------------|-------------|
| Same key + `RUNNING` + fresh lease | `work_generation_in_progress` for competing caller | Zero from competing caller | None |
| Same key + `RUNNING` + stale lease + zero Content | Reclaim permitted | Provider **may** be called again on reclaim | None until success path |
| Same key + `RUNNING` + stale lease + complete six Content | Reconcile → `SUCCEEDED` | Zero | None |
| Same key + `SUCCEEDED` | Return same exact six artifacts | Zero | Zero |
| Same key + `FAILED` | Replay same durable failure | Zero | Zero |
| Same key + different fingerprint | Idempotency conflict | Zero | None |
| Different key + same Work revision + same capability + `SUCCEEDED` | Existing-generation conflict | Zero | None |
| Different key + prior `FAILED` run | New attempt permitted if Fence A/B allow | Per new execution rules | Per new execution |
| DEV03 worksheet + DEV04 preparation, same Work revision | Both permitted | Independent per capability | Independent per capability |

Fence semantics (§7) govern concurrency. DEV04 does **not** invent regeneration semantics. Future selective or whole-kit regeneration requires separate architecture review.

### 15. Preparation capability

Freeze native provider-neutral capability:

```text
education.generate_preparation_kit
```

**Do not replace:**

```text
education.generate_worksheet
```

DEV03 worksheet capability remains stable and independently callable through the DEV03 compatibility path (`POST …/actions/generate`).

DEV04 Prepare Tomorrow is **additive**.

**Not required for DEV04 baseline:**

- Planner Agent
- Content Composer Agent
- Quality Reviewer Agent
- MCP server/client exposure as generator
- Temporal workflow orchestration
- LangChain / LlamaIndex / CrewAI / AutoGen / Semantic Kernel

Orchestration is a bounded synchronous application service behind stable product APIs ([ADR-044](ADR-044-ai-platform-behind-stable-services.md)).

### 16. Provider execution strategy

Freeze:

```text
one bounded provider-neutral structured generation for the preparation kit
```

Do **not** freeze in this ADR:

- OpenAI as product architecture
- specific model identifiers
- specific output token budgets
- specific timeout values
- reasoning effort settings
- specific provider request parameters

Those remain implementation / development provider configuration.

Architecture requires only:

- bounded typed structured output;
- provider neutrality;
- capability-specific output budget configurable outside product authority;
- no frontend provider selection;
- no provider SDK imports outside provider adapters.

### 17. Typed preparation output — PreparationKitV1

Conceptual provider-neutral capability output envelope:

```text
PreparationKitV1
```

Top-level fields:

```text
title
teacher_summary
shared_learning_objectives
lesson_plan
worksheet
quick_quiz
homework
teacher_notes
```

Each component uses its own versioned educational sub-schema — not `dict[str, Any]`.

**Important:** The model does **not** independently generate a second answer-key copy. **Answer Key** is created **deterministically by AIEOS** from the already validated question-bearing components (`worksheet`, `quick_quiz`, `homework`). This reduces duplicate answer authority, cross-artifact answer drift, structured output size, and unnecessary provider work.

Shared learning objectives are canonical within the kit. Component drafts reference shared objective IDs.

Question IDs must be unique within the preparation result or namespaced by artifact kind.

### 18. Answer key

`answer_key` is a **separate governed Content artifact** in the teacher's preparation kit.

Its entries are **deterministically derived** from validated worksheet, quiz, and homework question/answer structures — not independently model-generated competing answer truth.

`AnswerKeyV1` MUST identify for each entry:

- source artifact kind
- source question ID
- answer
- explanation (when available)

During materialization, where implementation permits, bind the answer key to the exact source ContentVersion `ResourceRef`s.

No answer-key entry may reference a nonexistent question.

### 19. Worksheet compatibility

Retain:

```text
content_type: worksheet
schema_id: education.worksheet
schema_version: 1
```

Do **not** casually create WorksheetV2.

If `PreparationKitV1` uses a normalized shared-objective draft shape internally, implementation MUST deterministically materialize the existing authoritative **WorksheetV1** contract.

DEV03 worksheet behavior must not regress.

### 20. New Content types (development only)

DEV04 candidate **development** Content types and schema identities:

| content_type | schema_id | schema_version |
|--------------|-----------|----------------|
| `lesson_plan` | `education.lesson_plan` | 1 |
| `worksheet` | `education.worksheet` | 1 |
| `quiz` | `education.quiz` | 1 |
| `homework` | `education.homework` | 1 |
| `answer_key` | `education.answer_key` | 1 |
| `teacher_notes` | `education.teacher_notes` | 1 |

Naming follows the existing DEV03 convention (`education.worksheet@1`).

This ADR does **not** activate production Content catalog registration.

### 21. Teacher notes and answer key audience

`teacher_notes` and `answer_key` are **teacher-facing governed Content**.

Both enter Review Queue in DEV04.

Neither is automatically learner-publishable.

Do **not** encode audience semantics inside free-text description fields alone. Audience / publication eligibility MUST be represented by typed schema, content policy, or another governed mechanism.

Any public learner-delivery policy for these types remains separate from DEV04 if the existing catalog cannot express it safely.

### 22. Educational Quality

Require **both**:

- component quality, and
- cross-artifact coherence quality.

All hard validation occurs **before** the Content transaction begins.

Preserve all applicable DEV03 worksheet checks when validating the worksheet component.

Architecture-binding check concepts include at minimum:

```text
schema_valid
shared_objectives_present
lesson_plan_objectives_mapped
worksheet_objectives_mapped
quiz_objectives_mapped
homework_objectives_mapped
question_identifier_integrity
answer_key_complete
answer_key_reference_integrity
cross_artifact_objective_consistency
cross_artifact_topic_consistency
unsupported_alignment_claim_absent
teacher_notes_present
```

Subjective heuristics MUST NOT be frozen as hard failures without a deterministic, provider-independent definition.

Warnings MAY exist but MUST NOT silently convert a hard invariant into a pass.

### 23. Review

Successful DEV04 kit baseline produces:

- 6 Content aggregates
- 6 immutable AI ContentVersions
- 6 `IN_REVIEW` admissions

Expected Review Queue increase per kit: **+6**.

Approval remains exact-version and per artifact. Approving one artifact does **not** approve siblings. Request Changes on one does **not** modify siblings.

No kit-level approval shortcut.

```text
AI Generated ≠ Approved
Approved ≠ Published
```

([ADR-048](ADR-048-review-queue-owns-approval.md), [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md))

### 24. API evolution

Preserve existing:

```text
POST /api/v1/teaching/works/{work_id}/actions/generate
```

for DEV03 worksheet compatibility.

Additive DEV04 product action:

```text
POST /api/v1/teaching/works/{work_id}/actions/prepare
```

Outcome language only. Preserve:

- `If-Match`
- `Idempotency-Key`
- trusted tenant/principal context
- stable RFC 9457 error semantics

Successful response conceptually returns:

```text
work_id
generation_run_id
artifacts[]
```

where `artifacts[]` contains the exact six governed results. May include kit-level Educational Quality projection.

Do **not** expose provider, model, agent, or capability catalogue as normal teacher inputs.

```text
GET /api/v1/teaching/works/{work_id}/artifacts
```

remains the authoritative Work artifact projection surface and MUST be extended to return all Content results bound to preparation runs, including `artifact_kind` and derived kit status.

### 25. No PreparationKit aggregate yet

Defer a durable Teaching-domain **PreparationKit** aggregate.

Current identities:

| Role | Identity |
|------|----------|
| Business container | Teaching Work |
| Execution | GenerationRun |
| Artifact authority | Generic Content |

A future **PreparationKit** aggregate MAY become justified when product requirements introduce:

- independent artifact regeneration;
- whole-kit regeneration versions;
- stable kit identity across multiple GenerationRuns;
- lesson execution references;
- kit-level lifecycle independent of Work.

That requires separate architecture review.

### 26. Temporal

Do **not** introduce Temporal in DEV04 baseline.

The operation is a bounded synchronous application operation with idempotency, lease, and reconciliation already available from DEV03.

Revisit Temporal only if future behavior crosses [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) boundaries for long-running asynchronous execution, human-wait workflow, multi-system durable orchestration, or scheduled/recurrent preparation.

---

## Consequences

### Positive

- Coherent outcome-first teacher experience aligned with [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md)
- One provider execution per baseline kit — shared context and coherence
- Six separately governed artifacts — exact-version review preserved
- One Content SoR — no duplicated result or staging SoR
- Simple crash recovery after Content commit via provenance queries
- Existing Review Queue reuse
- DEV03 worksheet compatibility path preserved

### Trade-offs

- Content batch transaction is larger and longer-held
- Failure during artifact 6 materialization rolls back artifacts 1–5 within the same transaction attempt
- Stale recovery **before** Content commit may require repeating provider generation
- V2 provenance, version-aware Content uniqueness index, and capability/revision-aware GenerationRun fence migration are required
- Future selective regeneration may require PreparationKit or equivalent resource evolution
- Kit-level UX grouping in Review Queue remains a product projection concern, not a new approval authority

### Implementation consequences (not authorized by architecture freeze alone)

Future implementation, **after separate Chief Architect authorization**, may include:

- `AIGenerationProvenanceV2`
- plural repository query by `generation_run_id`
- version-aware unique indexes on `content.content_versions` provenance
- **evolution of `uq_ai_generation_runs_work_active_or_succeeded`** to Fence A (revision + capability outcome) and Fence B (single active execution per Work + capability) semantics — preserving existing DEV03 `GenerationRun` history
- batch AI Content materialization-for-review service
- `PreparationKitV1` and typed drafts
- deterministic `AnswerKeyV1` builder
- `GeneratePreparationKitCapability`
- Preparation quality evaluator
- `PrepareTeachingWorkService` with idempotency, recovery, and reconciliation
- additive `/actions/prepare` API
- Work artifacts plural projection
- Teacher OS preparation-kit frontend UX
- PostgreSQL adversarial tests
- controlled live-provider proof under separate gate

---

## Recommended implementation slicing

Implementation slices require separate authorization after this ADR merges:

| Slice | Scope |
|-------|-------|
| **DEV04-I01** | Typed PreparationKit contracts + provenance V2 contracts |
| **DEV04-I02** | Content + GenerationRun persistence migration: version-aware Content uniqueness, capability/revision-aware fences, plural provenance queries |
| **DEV04-I03** | Atomic six-artifact Content materialization service |
| **DEV04-I04** | `GeneratePreparationKitCapability` + deterministic answer-key builder |
| **DEV04-I05** | Preparation Educational Quality / coherence baseline |
| **DEV04-I06** | `PrepareTeachingWorkService`: idempotency + recovery + reconciliation |
| **DEV04-I07** | Additive HTTP/OpenAPI contracts + Work artifacts projection |
| **DEV04-I08** | PostgreSQL / RLS / concurrency / recovery adversarial verification |
| **DEV04-I09** | Teacher OS preparation-kit frontend UX |
| **DEV04-I10** | Controlled live-provider product proof — **separate authorization** |

---

## Alternatives considered

1. **`ai.generation_artifacts` result bridge** — Rejected for DEV04 baseline: duplicates Content provenance authority; risks second result SoR.
2. **Durable validated-kit staging in AI schema** — Rejected for DEV04 baseline: violates single Content payload authority; blurs execution vs Content boundaries.
3. **Partial kit materialization (3/6 valid artifacts)** — Rejected for DEV04 baseline: increases recovery complexity; conflicts with atomic kit product promise for baseline slice.
4. **Six independent provider calls** — Rejected for DEV04 baseline: weaker coherence; higher cost; unnecessary orchestration surface for baseline kit.
5. **PreparationKit business aggregate now** — Rejected for DEV04: premature before regeneration and cross-run kit identity requirements are product-authorized.

---

## Compliance

Frozen / Approved architecture **does not** authorize:

- DEV04 Backend or Frontend implementation by this document alone.
- database migration implementation.
- OpenAPI implementation change.
- live provider call or credential creation.
- production Content catalog activation.

DEV03 paths must remain functional.

Implementation slices require separate Chief Architect authorization gates per slice — including **DEV04-I10** live provider proof.
