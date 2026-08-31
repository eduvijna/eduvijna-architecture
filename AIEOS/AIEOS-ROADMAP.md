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
| [ADR-AIEOS-052](../decisions/ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) — Prepare Tomorrow multi-artifact kit architecture (TOS-DEV04) | **Architecture Frozen / Approved** | Founder / Product Architecture approval **2026-08-28**; unlocks deferred ADR-047 orchestration architecture; **TOS-DEV04 implementation NOT authorized / NOT implemented** |
| [ADR-AIEOS-053](../decisions/ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) — Teaching Assignment & Classroom Delivery Authority (TOS-DEV06) | **Proposed / Freeze Candidate** | Architecture freeze candidate deposited **2026-08-31**; Publication ≠ Assignment; TeachingAssignment intent SoR; ClassRef School Context prerequisite; **DEV06-I01+ implementation NOT authorized** |
| TOS-DEV04 source implementation (DEV04-I01 … I09) | **Not authorized** | Requires separate Chief Architect slice authorization after ADR merge |
| TOS-DEV04 live provider proof (DEV04-I10) | **Not authorized** | Separate gate |
| Review Queue ↔ Existing Generators Integration (derived EBP-001.9 candidate) | **Proposed** | Blueprint Wave 1 acceptance gap; may require Content SoR decision first |
| Sprint 4 hardening / GA readiness (EBP-001) | **Proposed** (blueprint) | After durable generate→queue path exists |

Architecture for multi-artifact Prepare Tomorrow is now Frozen / Approved. **No TOS-DEV04 implementation slice is authorized here.**

---

## Deferred

| Item | Status | Notes |
|------|--------|-------|
| Teacher Memory (durable) | Deferred | Explicitly not EBP-001.8 |
| Personalization / inferred preferences | Deferred | |
| Agents | Deferred | ADR-044; not premature |
| MCP | Deferred | ADR-044; not premature |
| AI Assistant expansion | Deferred | Placeholder today |
| Publishing / assign / send after Approved | Deferred | Approved ≠ Published; Published ≠ Assigned ([ADR-AIEOS-053](../decisions/ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) Proposed / Freeze Candidate) |
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
