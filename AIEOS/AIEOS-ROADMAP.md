# AIEOS — Roadmap

**Purpose:** High-level roadmap for EduVijna AIEOS  
**Rule:** Do **not** invent an artificial detailed roadmap. Use existing repository blueprint / PA phases.  
**Rule:** Discovery recommendations are **not** approved implementation.

---

## Authority note

Conflict preference:

1. Approved architecture decisions  
2. Approved product / engineering documents  
3. Current source code / contracts  
4. AIEOS orientation documents

Labels used below:

- **Approved** — authorized / accepted decision or closed approved slice  
- **Proposed** — discovery or blueprint-derived recommendation pending architecture authorization  
- **Deferred** — consciously not now  
- **Future** — vision / later phase

---

## Completed

| Item | Status | Notes |
|------|--------|-------|
| AIEOS product identity orientation | Approved (project context) | EduVijna = AIEOS complete product |
| Teacher OS product architecture foundation (PA-001 / TLM-001) | Approved (product docs) | Teacher OS subsystem model |
| Engineering Constitution EBP-000 v1.0 | Approved / Frozen | Implementation standards |
| ADR-042 … ADR-048 | Approved | Teacher OS architecture set |
| EBP-001.1 Teacher OS Shell | Approved | Closed slice |
| EBP-001.2 Today's Mission | Approved | Closed UX slice (mock-backed data) |
| EBP-001.3 Teaching Intent | Approved | Closed UX slice |
| EBP-001.4 Intent → Preparing Bridge | Approved | Closed bridge slice |
| EBP-001.5 Review Queue | Approved | Closed UX/semantics; mock SoR |
| EBP-001.6 Continuous Context | Approved | Session Context; not Memory |
| EBP-001.7 Mission Service Hardening | Approved | Closed hardening slice |
| EBP-001.8 Teacher / School Context | Approved (slice tracking) | Read surface; not Memory |
| TOS-DEV04 — Prepare Tomorrow native implementation | Approved / Complete | Backend `origin/main` `06e05277e73e0c71172cae4904efb37d771c3fad` |
| TOS-DEV06 — TeachingAssignment native implementation + Product E2E | Approved / Complete | Backend `06e05277e73e0c71172cae4904efb37d771c3fad`; Frontend `89ee9f1330f635de3186d21e0102cb63c5c698e1` (TOS-DEV06-I05) |

---

## Current

| Item | Status | Notes |
|------|--------|-------|
| EBP-001.9 Wave 1 next-slice discovery & preflight | Proposed / under architecture review | Not implemented |
| Persistence / Content SoR design for durable Review Queue | Proposed / under architecture review | Preflight: **DB CHANGE REQUIRED**; **DB creation NOT authorized** |
| Teacher OS behind `teacher_os_enabled` | Approved (flagged rollout path) | Default off pattern |

---

## Approved Next

No new implementation slice is recorded here as **Approved** solely because discovery suggested it.

| Item | Status | Notes |
|------|--------|-------|
| [ADR-AIEOS-052](../decisions/ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) — Prepare Tomorrow multi-artifact kit architecture (TOS-DEV04) | **Architecture Frozen / Approved** | Founder / Product Architecture approval **2026-08-28**; **TOS-DEV04 native implementation COMPLETE** (Backend `06e05277e73e0c71172cae4904efb37d771c3fad`); live provider proof (**DEV04-I10**) remains separate gate |
| [ADR-AIEOS-053](../decisions/ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) — Teaching Assignment & Classroom Delivery Authority (TOS-DEV06) | **Architecture Frozen / Approved** | Founder / Product Architecture approval **2026-08-31**; **TOS-DEV06 native implementation + Product E2E COMPLETE** (Backend `06e05277e73e0c71172cae4904efb37d771c3fad`; Frontend `89ee9f1330f635de3186d21e0102cb63c5c698e1`); external LMS / Student OS delivery deferred |
| [ADR-AIEOS-046R1](../decisions/ADR-AIEOS-046R1-aieos-production-event-plane-multi-domain-publisher-scope-revision.md) — Production Event Plane Multi-Domain Publisher Scope Revision | **Architecture Frozen / Approved** | Founder / Product Architecture approval **2026-08-31**; TeachingAssignment outbox events **implemented** in TOS-DEV06-I03; production EVENT activation / NATS provisioning **NOT authorized** |
| [ADR-AIEOS-054](../decisions/ADR-AIEOS-054-aieos-teaching-execution-observation-authority.md) — Teaching Execution & Observation Authority (TOS-DEV07) | **Architecture Frozen / Approved** | Founder / Product Architecture approval **2026-09-01**; HYBRID / Option D; TeachingExecution SoR; Assigned ≠ Taught ≠ Assessed ≠ Mastered; **TOS-DEV07 implementation COMPLETE** (DEV07-I01–I04 formally closed; real-stack Product E2E complete); program closeout synchronized by **TOS-DEV07-C01**; learner-specific / attendance / assessment / mastery / production NATS remain **not authorized** |
| [ADR-AIEOS-055](../decisions/ADR-AIEOS-055-aieos-assessment-learning-evidence-authority.md) — Assessment & Learning Evidence Authority (TOS-DEV08) | **Architecture Frozen / Approved** | Founder / Product Architecture approval **2026-09-03**; OPTION D class-level-first ClassroomAssessment; **TOS-DEV08 implementation COMPLETE** (DEV08-I01–I04 formally closed; real-stack Product E2E complete); program closeout synchronized by **TOS-DEV08-C01**; Backend `1fe28f4fd1a2a2070aa69d67daa49cd53ba5820d`; Frontend `30c94f3e0403b9a5a2e955c706766035490598f9`; Alembic `tosd080002`; learner-specific / mastery / Improve / production remain **not authorized** |
| TOS-DEV04 live provider proof (DEV04-I10) | **Not authorized** | Separate gate |
| Review Queue ↔ Existing Generators Integration (derived EBP-001.9 candidate) | **SATISFIED / HISTORICAL CANDIDATE** | Native TOS-DEV03/DEV04 + Generic Content implementation now provides the durable generate → ContentVersion(IN_REVIEW) → Review Queue path; old EBP-001.9 gap is superseded by current implementation. Historical record retained; no new implementation package. |
| Sprint 4 hardening / GA readiness (EBP-001) | **Proposed** (blueprint) | Production hardening remains deferred under Development Outcome First; not selected by TOS-DEV08 |

Architecture for multi-artifact Prepare Tomorrow is Frozen / Approved and **native TOS-DEV04 implementation is complete**. **ADR-AIEOS-054 (TOS-DEV07) is Frozen / Approved and TOS-DEV07 implementation (DEV07-I01 through DEV07-I04) is COMPLETE.** **ADR-AIEOS-055 (TOS-DEV08 Assessment) is Frozen / Approved** and **TOS-DEV08 implementation (DEV08-I01 through DEV08-I04) is COMPLETE** (Backend `1fe28f4fd1a2a2070aa69d67daa49cd53ba5820d`; Frontend `30c94f3e0403b9a5a2e955c706766035490598f9`; Alembic `tosd080002`; real-stack ClassroomAssessment Product E2E COMPLETE; closeout sync **TOS-DEV08-C01**). Assess completion does **not** authorize Improve, Teacher Memory, Student Intelligence, learner-specific assessment, Notification Center, AI Assistant expansion, or production activation. Next eligible activity is fresh architecture discovery. Production hardening remains deferred.

---

## Deferred

| Item | Status | Notes |
|------|--------|-------|
| Teacher Memory (durable) | Deferred | Explicitly not EBP-001.8 |
| Personalization / inferred preferences | Deferred | |
| Agents | Deferred | ADR-044; not premature |
| MCP | Deferred | ADR-044; not premature |
| AI Assistant expansion | Deferred | Placeholder today |
| External learner delivery / LMS / Student OS | Deferred | Published ≠ Assigned; Assigned ≠ Externally Delivered / Attempted / Submitted / Graded ([ADR-AIEOS-053](../decisions/ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md)); native Publication and TeachingAssignment are **implemented** |
| Student OS / Parent OS / Principal OS (Wave 1) | Deferred | EBP-001 out of scope |
| New generators / new model providers | Deferred | EBP-001 out of scope |
| Major database redesign / unauthorized Content SoR creation | Deferred / blocked | Under architecture review |

---

## Future / Vision

From product architecture roadmap phases (vision sequencing — **Future**, not sprint commits):

| Phase | Status | Outcome (product docs) |
|-------|--------|------------------------|
| Phase 1 — Teacher OS Foundation | Current programme / partial | Outcome nav; Intent; Review; Context inheritance |
| Phase 2 — Teaching Assistant | Future | Daily Loop depth; Observe/Assess/Improve; notifications |
| Phase 3 — Student Intelligence | Future | Teacher-mediated personalisation |
| Phase 4 — School Intelligence | Future | Principal OS aggregates / teaching health |
| Phase 5 — Parent Intelligence | Future | Parent clarity without teacher overload |

AI Engineering Platform sophistication (Agents, MCP, multi-provider routing) remains **Future** behind stable product services (**ADR-044**).

---

## Blueprint reminder (EBP-001 Wave 1)

Planned engineering sequence (not rewritten here):

```text
Sprint 0 flags + shell
  → Sprint 1 nav + Mission
  → Sprint 2 Review Queue (critical path)
  → Sprint 3 Continuous Context + dashboard depth
  → Sprint 4 hardening
```

EBP-001.1–001.8 delivered substantial UI spine; Sprint 2 durable generator→queue acceptance remains the critical unfinished Wave 1 gap per discovery evidence.

---

## Related

- `AIEOS-CURRENT-STATE.md`
- `AIEOS-VISION.md`
- `eduvijna-product/engineering/EBP-001/`
- `eduvijna-product/product-architecture/teacher-os/ROADMAP.md`
