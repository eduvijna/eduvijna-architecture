# AIEOS — Current State

**Question answered:** Where is EduVijna AIEOS today?  
**Evidence date orientation:** 2026-08-11 repository / discovery evidence  
**Nature:** Orientation document — does not authorize implementation

---

## Authority note

Conflict preference:

1. Approved architecture decisions  
2. Approved product / engineering documents  
3. Current source code / contracts  
4. AIEOS orientation documents

---

## AIEOS platform ADR catalogue

The AIEOS platform ADR family is now canonically deposited under `decisions/ADR-AIEOS-*`. Teacher OS `ADR-042`–`ADR-048` remain a distinct ID family.

Historical ADR-AIEOS-023 remains Frozen / Approved (Identity/Tenant/Security), but its original body is unavailable. ADR-AIEOS-023R1 is now the canonical restated identity/tenant/security implementation baseline. This closes the architecture-catalogue identity/tenant/security gap. It does **not** authorize identity implementation or production promotion by itself.

Current Asset / platform implementation remains **NON_PRODUCTION**. Architecture catalogue synchronization does **not** authorize production deployment or mutation.

**Production infrastructure (architecture freeze only):** DigitalOcean production infrastructure baseline is frozen ([ADR-AIEOS-037](../decisions/ADR-AIEOS-037-aieos-production-infrastructure-baseline.md)). DigitalOcean Spaces is **rejected** for authoritative Asset BlobStore under the current frozen contract. Cross-cloud managed Asset object storage exception is frozen ([ADR-AIEOS-038](../decisions/ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md)). Amazon S3 is the sole provider candidate advancing to controlled validation. Amazon S3 is **not** yet selected as the production provider. No infrastructure apply, AWS resource, SDK, or adapter is authorized by catalogue deposition.

---

## Product

**EduVijna AIEOS** — Artificial Intelligence Engineering Education Operating System

- Founder: **Sreekanth**
- Architecture role: **Chief AI Enterprise Architect — ChatGPT**
- **AIEOS** = complete product  
- **Teacher OS** = subsystem (current major delivery programme)

---

## Current major program

**Teacher OS / EBP-001 — Teacher OS Foundation (Wave 1)**

Philosophy: vertical-slice first on existing apps, behind `teacher_os_enabled`, architecture-bound by ADR-042…048 and EBP-000.

---

## Completed EBP work

Recorded as **APPROVED / CLOSED** in EBP-001.9 Phase 0 discovery status (aligned with implementation + review package evidence in product / Quiz-React):

| ID | Slice | Notes (evidence-based) |
|----|-------|-------------------------|
| **EBP-001.1** | Teacher OS Shell | Flag-gated shell, nav, routes, TeacherOsContext foundation (EDR-002) |
| **EBP-001.2** | Today's Mission | Mission landing UI; MissionService façade (mock-backed) |
| **EBP-001.3** | Teaching Intent | Intent landing + wizard UX (ADR-045); mock Intent |
| **EBP-001.4** | Intent → Preparing Bridge | Preparing Kit bridge; mock artifacts; Continue path |
| **EBP-001.5** | Review Queue | Queue UX + approve/reject/request-changes semantics (ADR-048); in-memory mock SoR |
| **EBP-001.6** | Continuous Context | Session/Intent thread via React Context (EDR-001); not durable Memory |
| **EBP-001.7** | Mission Service Hardening | MissionService read-composition hardening; Continuous Context snapshot integration |
| **EBP-001.8** | Teacher / School Context | Read surface; school name hydrate via existing `my-school`; **not** Teacher Memory |

**Important nuance:** Several completed slices are **UI / façade complete** with **mocks**. Wave 1 acceptance still requires Review Queue integration with existing generators (durable path not closed).

EBP-001 product review package docs may lag the latest slice numbering; prefer slice IDs + code/tests + latest `engineering/EBP-001/*` package for 001.8.

---

## Current work

### EBP-001.9

**Status:** Discovery / preflight — **not implemented** as a closed delivery slice.

Derived next slice (discovery recommendation, **not** automatic implementation authorization):

> **Review Queue ↔ Existing Generators Integration** — close the Wave 1 critical path: generate → needs-review → approve (no auto-publish), using stable product/content services — not Agents/MCP.

#### Persistence architecture status

Persistence preflight verdict:

# DB CHANGE REQUIRED

Findings (read-only discovery):

- Generic Content SoR expected by `ContentPersistence` (`edu.contents` / `edu.content_versions`) **not** evidenced in-repo migrations or live PostgREST schema cache.
- Existing `edu.content` (singular) is LMS/CMS — **not** the Platform AI / Teacher OS generic Content SoR.
- Specialized tables (e.g. kindergarten worksheets, sketchnotes) show stewardship facet patterns but are **not** the generic SoR.
- Durable Teacher OS Review Queue therefore needs Content SoR alignment/creation — **architecture design required**.

**Recorded constraint:**

> **Persistence architecture is under architecture review.**

**Explicitly NOT authorized by this document:**

- Database creation  
- Migrations  
- API changes  
- Implementation of EBP-001.9  

---

## Repositories

| Repository | Known responsibility |
|------------|----------------------|
| **Quiz-React** (eduvijna-web) | Teacher OS shell, Mission, Intent, Preparing Kit, Review Queue UI, Continuous Context, Teacher/School Context cards |
| **eduvijna-api** | Auth, feature flags (`teacher_os_enabled`), Platform AI / content domain, school-management APIs, persistence contracts |
| **eduvijna-product** | Product vision, PA-001 Teacher OS architecture, EBP-000/001 engineering blueprint & standards, EDRs |
| **eduvijna-architecture** | Enterprise architecture governance, ADRs (042–048), reviews, discovery, **AIEOS orientation** |

---

## Current architectural boundaries (verified)

| Boundary | Source |
|----------|--------|
| Shell owns UX, not generators/business engines | ADR-042 |
| Foundation → Hardening → Review → Next Capability | ADR-043 |
| Frontend → stable product services only; no direct Agents/MCP | ADR-044 |
| Teaching Intent owns goals; generators are capabilities | ADR-045 |
| One Artifact lifecycle for all types | ADR-046 |
| Outcome-first Prepare language | ADR-047 |
| Review Queue owns approval; Approved ≠ Published | ADR-048 |
| Continuous Context ≠ Teacher Memory ≠ School Context | PA Continuous Context · EDR-001 |
| Teacher/School Context read surface ≠ Teacher Memory | EBP-001.8 |
| Teacher OS evolves existing repos (no new app) | EBP-001 |
| Engineering Constitution v1.0 frozen | EBP-000 |

---

## Current deferred capabilities

| Capability | Status |
|------------|--------|
| Teacher Memory (durable preferences / personalization) | Deferred / not currently authorized as next Wave 1 AI Memory slice |
| Personalization / inferred preferences | Deferred |
| Agents | Deferred / not currently authorized (ADR-044 boundary) |
| MCP | Deferred / not currently authorized |
| Orchestration (full Prepare multi-artefact depth) | Deferred / later EBP; entry points only today |
| AI Assistant expansion | Placeholder / deferred |
| Publishing (assign/send after Approved) | Deferred relative to approval slices |
| Full Prepare orchestration | Deferred (EBP-001 out of scope depth) |
| Student OS / Parent OS / Principal OS | Out of Wave 1 scope |
| New generators / new model providers | Out of Wave 1 scope |
| Content SoR DB creation | **Not authorized** — under architecture review |

---

## Current risks

| Risk | Notes |
|------|-------|
| **EBP-001.9 persistence** | DB CHANGE REQUIRED; no authorized DB creation; production Review Queue durable SoR missing |
| Review Queue mock vs generators | Wave 1 acceptance gap — generate→queue not closed on durable path |
| Naming confusion | Teacher/School Context vs Teacher Memory; Continuous Context vs Memory |
| Dual chrome | Classic MainLayout + TeacherShell until Mission is default production landing |
| Discovery ≠ authorization | EBP-001.9 recommendations must not be treated as approved implementation without architecture approval |
| Premature platform jumps | Risk of introducing Agents/MCP/Orchestration/Memory/DB without ADR + EBP authorization |

---

## Related

- `AIEOS-ARCHITECTURE-JOURNEY.md`
- `AIEOS-DECISION-LEDGER.md`
- `AIEOS-ROADMAP.md`
