# AIEOS — Architecture Journey

**Purpose:** Chronological record of AIEOS architecture evolution  
**Rule:** Do not invent historical facts. Where evidence is missing: *Not established by current repository evidence.*

---

## Authority note

Conflict preference:

1. Approved architecture decisions  
2. Approved product / engineering documents  
3. Current source code / contracts  
4. AIEOS orientation documents

---

## 1. AIEOS product vision

| Field | Content |
|-------|---------|
| **Objective** | Establish EduVijna as an Artificial Intelligence Engineering Education Operating System (AIEOS) — complete product identity — with Teacher OS as a subsystem. |
| **Architectural reason** | Orient the ecosystem as an education OS, not a feature collection; keep architecture ahead of implementation. |
| **What was implemented** | Product vision / Teacher OS PA docs, North Star, Engineering Constitution, ADRs 042–048, EBP-001 blueprint; AIEOS orientation folder (this permanent knowledge foundation). |
| **What was deliberately NOT implemented** | Treating Teacher OS as the entire product; premature Agents/MCP/Orchestration/Memory platforms. |
| **Governing decisions** | Approved project identity (AIEOS); ADR-042…048; EBP-000; PA-001 / TLM-001 foundations. |
| **Current status** | Vision and governance active; AIEOS orientation documents established in `eduvijna-architecture/AIEOS/`. |

---

## 2. Teacher OS foundation

| Field | Content |
|-------|---------|
| **Objective** | Define Teacher OS as the teacher-centric operating subsystem (Mission, Intent, Memory, School Context, Daily Loop, Review Queue). |
| **Architectural reason** | Replace tool-zoo mental model with Intent → orchestration → Review Queue; preserve existing capability engines. |
| **What was implemented** | Product architecture package PA-001; feature boundaries; Continuous Context model; Artifact model; EBP-001 Wave 1 blueprint targeting `Quiz-React` + `eduvijna-api`. |
| **What was deliberately NOT implemented** | New Teacher OS application repository; Student/Parent/Principal OS in Wave 1; major DB redesign in Wave 1 scope. |
| **Governing decisions** | PA-001; FEATURE_BOUNDARIES; ENGINEERING_CONSTITUTION; later ADR-042…048. |
| **Current status** | Foundation approved as product/architecture direction; Wave 1 delivery in progress. |

---

## 3. EBP-001.1 — Teacher OS Shell

| Field | Content |
|-------|---------|
| **Objective** | Ship flag-gated Teacher OS chrome (shell, nav, routes, extension slots) without breaking classic EduVijna. |
| **Architectural reason** | Shell owns UX only; additive rollout; reusable foundation before features (ADR-042 / ADR-043 / EDR-002). |
| **What was implemented** | TeacherShell, TeacherOsRoutes, TeacherNavigation, TeacherOsFlagProvider, TeacherOsContext selections, telemetry façade; API `teacher_os_enabled` flag exposure. |
| **What was deliberately NOT implemented** | Generators inside shell; Mission as forced production default landing; Review badge live data; AI/Memory. |
| **Governing decisions** | ADR-042; EDR-002; EBP-000; EBP-001 Sprint 0. |
| **Current status** | APPROVED / CLOSED (EBP-001.9 discovery status). |

---

## 4. EBP-001.2 — Today's Mission

| Field | Content |
|-------|---------|
| **Objective** | Provide Today's Mission landing as the teacher briefing experience. |
| **Architectural reason** | Mission-first home vs module tile menu; compose day briefing through MissionService façade (ADR-044 service boundary). |
| **What was implemented** | TodayMissionPage and Mission cards; mock MissionService adapter path. |
| **What was deliberately NOT implemented** | Full ERP/timetable mission aggregate API as hard dependency; durable AI-prepared kits. |
| **Governing decisions** | PA Today's Mission; ADR-044; EBP-001 Sprint 1. |
| **Current status** | APPROVED / CLOSED as Mission UX slice; Mission data still mock-backed (partial vs blueprint ERP depth). |

---

## 5. EBP-001.3 — Teaching Intent

| Field | Content |
|-------|---------|
| **Objective** | Capture teacher goals via Teaching Intent experience (Prepare outcome language). |
| **Architectural reason** | Teaching Intent owns goals; generators are capabilities (ADR-045 / ADR-047). |
| **What was implemented** | Intent landing + wizard UX; mock Intent adapter. |
| **What was deliberately NOT implemented** | Orchestration; Review Queue; Agents; MCP; APIs; DB; AI generation. |
| **Governing decisions** | ADR-045; ADR-047; EBP-001.3. |
| **Current status** | APPROVED / CLOSED (UX). |

---

## 6. EBP-001.4 — Intent → Preparing Bridge

| Field | Content |
|-------|---------|
| **Objective** | Bridge Continue from Intent into Preparing Kit / Review Queue entry experience. |
| **Architectural reason** | Keep Intent → kit → Review path coherent without claiming full orchestration. |
| **What was implemented** | PreparingKitPage and Review Queue entry bridge; mock artifacts. |
| **What was deliberately NOT implemented** | Real content generation; full Review Queue (landed as 001.5); custom free-text Intent (deferred). |
| **Governing decisions** | ADR-045; EBP-001.4 review package notes. |
| **Current status** | APPROVED / CLOSED as bridge UX; generation not wired. |

---

## 7. EBP-001.5 — Review Queue

| Field | Content |
|-------|---------|
| **Objective** | Deliver Review Queue as approval cockpit (teacher judgement). |
| **Architectural reason** | Review Queue owns approval; Approved ≠ Published; one queue for all Artifact types (ADR-048 / ADR-046). |
| **What was implemented** | Review Queue list/detail UX; approve / reject / request-changes actions on mock Artifact service. |
| **What was deliberately NOT implemented** | Publish/assign; generation ownership; orchestration; durable Content SoR wiring. |
| **Governing decisions** | ADR-048; ADR-046; EBP-001.5. |
| **Current status** | APPROVED / CLOSED as UX/semantics slice; **not** durable generator integration. |

---

## 8. EBP-001.6 — Continuous Context

| Field | Content |
|-------|---------|
| **Objective** | Preserve session/Intent work thread across Preparing Kit and Review Queue navigation. |
| **Architectural reason** | Continuous Context is session continuity — distinct from Teacher Memory and School Context (PA + EDR-001). |
| **What was implemented** | ContinuousContext provider mounted once at Teacher OS layout; threadId shared across SPA navigation. |
| **What was deliberately NOT implemented** | Teacher Memory; AI; durable persistence; Content AI session binding depth. |
| **Governing decisions** | EDR-001; PA Continuous Context; EBP-001.6. |
| **Current status** | APPROVED / CLOSED as session React Context; not durable Memory. |

---

## 9. EBP-001.7 — Mission Service Hardening

| Field | Content |
|-------|---------|
| **Objective** | Harden MissionService as stable read-composition façade and Mission UX behaviours. |
| **Architectural reason** | Stable product services before deeper AI (ADR-043 / ADR-044); Mission must not own engines. |
| **What was implemented** | MissionService hardening (mock adapter), Continuous Context snapshot plumbing, outcome CTA / empty-state fixes evidenced in tests. |
| **What was deliberately NOT implemented** | Backend Mission aggregate as mandatory; Agents/MCP; ERP deep integration. |
| **Governing decisions** | ADR-043; ADR-044; EBP-001.7. |
| **Current status** | APPROVED / CLOSED. |

---

## 10. EBP-001.8 — Teacher / School Context

| Field | Content |
|-------|---------|
| **Objective** | Provide Teacher/School Context **read surface** using existing school identity APIs. |
| **Architectural reason** | Institutional context inheritance without claiming Teacher Memory; reuse existing `my-school` authorization/tenancy. |
| **What was implemented** | School name hydration via existing `GET /api/v1/school-management/my-school`; School/Teacher Context cards; no new DB/API. |
| **What was deliberately NOT implemented** | Teacher Memory; preferences; inferred personalization; MissionService/ContinuousContext redesign; Agents/MCP/Orchestration. |
| **Governing decisions** | EBP-001.8 package; ADR-042 shell context boundary; PA School Context vs Memory distinction. |
| **Current status** | Implemented — awaiting architecture review per product package; discovery status lists APPROVED / CLOSED for Wave 1 slice tracking. **Not** Teacher Memory. |

---

## 11. EBP-001.9 — Discovery / preflight

| Field | Content |
|-------|---------|
| **Objective** | Determine the next correct Wave 1 engineering slice after 001.8 from blueprint + repository evidence; inspect content/review/persistence contracts. |
| **Architectural reason** | Do not invent roadmap; close Wave 1 acceptance gaps before premature AI/Memory/Agents; respect ADR-044 service boundary. |
| **What was implemented** | Discovery only: Phase 0 alignment; open-question resolution; content contract preflight; persistence preflight (**DB CHANGE REQUIRED**); Content SoR verification (Scenario C — no generic Content SoR). |
| **What was deliberately NOT implemented** | Code, APIs, flags, migrations, DB creation, ADRs/EDRs, Review Queue durable integration. |
| **Governing decisions** | Existing ADR-042…048; EBP-001 acceptance criteria; discovery recommendations **not** equal to implementation authorization. |
| **Current status** | **Discovery / architecture review.** Recommended next feature: Review Queue ↔ Existing Generators Integration. **Persistence architecture is under architecture review. DB creation is NOT authorized.** |

---

## 12. ADR-AIEOS-048 — First-production App runtime & OCI delivery

| Field | Content |
|-------|---------|
| **Objective** | Freeze first-production App Platform worker topology, dedicated BLR1 VPC, Preview compute exception, OCI digest authority, and Temporal secret destinations for WORKFLOW_DISPATCHER and TEMPORAL_WORKER. |
| **Architectural reason** | Prevent improvisation from default-blr1 reuse, mutable `latest` tags, shared Temporal keys, or Preview SKUs without bounded Founder/Chief acceptance. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-048](../decisions/ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) deposited; catalogue/ledger/current-state updated. Distinct from Teacher OS ADR-048. |
| **What was deliberately NOT implemented** | App Platform apps; VPC creation; DOCR `aieos-backend` repository; OCI publication; secret injection; OpenTofu plan/apply; production deployment. |
| **Governing decisions** | ADR-AIEOS-048; binding prior ADR-AIEOS-022/026/029/037/040R1/044R2/045/046/047. |
| **Current status** | **Architecture Frozen / Approved** as historical/base App runtime contract. Current App Platform **naming** authority is [ADR-AIEOS-048R1](../decisions/ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md). Current App Platform **ownership/deployment** authority is [ADR-AIEOS-048R2](../decisions/ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md). Cloud / App / OCI / deployment mutation **not authorized**. |

---

## 13. Workflow plane source & Temporal provisioning status reconciliation

| Field | Content |
|-------|---------|
| **Objective** | Keep journey/current-state aligned after PED-I12 Backend merge and WPI-A01 Temporal Cloud first-production apply without rewriting ADR-AIEOS-047 historical freeze text. |
| **Architectural reason** | Historical ADR freeze status ≠ current implementation/provisioning status. |
| **What was implemented** | WORKFLOW_DISPATCHER Backend runtime source (**PED-I12**) = **IMPLEMENTED / MERGED** at Backend `8f4dd172e6a0ba8b4ad944b0ae22060442356342`. Temporal Cloud Namespace `eduvijna-aieos-prod.w97q1` + WORKFLOW_DISPATCHER and TEMPORAL_WORKER service accounts (**WPI-A01**) = **PROVISIONED / CONFORMED / FORMALLY CLOSED**. |
| **What was deliberately NOT implemented** | Temporal runtime API-key issuance/injection; App Platform worker deployment; dedicated VPC creation; governed production OCI publication; production workflow execution. |
| **Governing decisions** | ADR-AIEOS-047 (architecture freeze retained); ADR-AIEOS-048 (App runtime architecture freeze); separately authorized PED-I12 / WPI-A01 execution gates. |
| **Current status** | Source + Namespace/SA provisioning closed; runtime keys, App Platform deployment, and production execution **not authorized**. |

---

## 14. ADR-AIEOS-048R1 — Provider-compliant App Platform naming

| Field | Content |
|-------|---------|
| **Objective** | Correct first-production App Platform application names so they satisfy the DigitalOcean App Platform naming constraint without changing any other ADR-AIEOS-048 contract. |
| **Architectural reason** | WPI-AP-I01 implementation review found ADR-AIEOS-048 names `eduvijna-aieos-prod-*` incompatible with the provider naming constraint. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-048R1](../decisions/ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md) deposited. CURRENT names `aieos-prod-workflow-dispatcher` (length 30) and `aieos-prod-temporal-worker` (length 26). ADR-AIEOS-048 historical body not rewritten. |
| **What was deliberately NOT implemented** | Infrastructure PR #12 modification or merge; App Platform apps; VPC creation; DOCR / OCI publication; secret injection; OpenTofu plan/apply; production deployment. |
| **Governing decisions** | ADR-AIEOS-048R1 (current naming); ADR-AIEOS-048 (historical/base non-naming authority). |
| **Current status** | **Architecture Frozen / Approved.** Current naming authority. Cloud / App / OCI / deployment mutation **not authorized**. |

---

## 15. ADR-AIEOS-048R2 — App Platform runtime ownership boundary

| Field | Content |
|-------|---------|
| **Objective** | Reject production OpenTofu ownership of DigitalOcean App Platform applications after empiric proof that provider 2.99.1 materializes out-of-band encrypted secrets into plan/state; move App lifecycle to a governed state-free deployment plane while preserving ADR-AIEOS-048/048R1 topology and naming. |
| **Architectural reason** | WPI-AP-SV01 / WPI-AP-SV01R1 disposable validation classified **B. FAIL_OPEN_TOFU_SECRET_MATERIAL**: refresh-only plan JSON contained `EV[...]` for an out-of-band `SECRET` while HCL had zero env blocks. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-048R2](../decisions/ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md) deposited. Production OpenTofu `digitalocean_app` ownership REJECTED. Encrypted `EV[...]` classified as secret material. State-free deployment plane required. ADR-048/048R1 historical bodies not rewritten. |
| **What was deliberately NOT implemented** | Infrastructure reconciliation; deployment-plane implementation; App Platform apps; VPC creation; DOCR / OCI publication; secret injection; OpenTofu plan/apply; production deployment. |
| **Governing decisions** | ADR-AIEOS-048R2 (current ownership/deployment); ADR-AIEOS-048R1 (current naming); ADR-AIEOS-048 (historical/base topology). |
| **Current status** | **Architecture Frozen / Approved.** WPI-AP-SV01/R1 = FORMALLY CLOSED — FAIL_OPEN_TOFU_SECRET_MATERIAL. WPI-AP-I02 Infrastructure reconciliation = FORMALLY CLOSED. Detailed deployment-plane behavior = [ADR-AIEOS-049](../decisions/ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md). Cloud / App / OCI / deployment mutation **not authorized**. |

---

## 16. ADR-AIEOS-049 — App Platform state-free deployment plane

| Field | Content |
|-------|---------|
| **Objective** | Freeze the WPI-AP-DP01 design for the governed state-free App Platform deployment plane required by ADR-AIEOS-048R2: direct DigitalOcean REST release controller with one App per process/release, transient in-memory secret/`EV[...]` handling, exact allowlist reconciliation, durable per-App lease plus double-read fence, independent dispatcher/worker credentials and secrets, immutable OCI digest authority, native rollback, and redacted evidence. |
| **Architectural reason** | After OpenTofu `digitalocean_app` ownership was rejected and removed (WPI-AP-I02), production App lifecycle requires an explicit state-free controller contract before any implementation. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-049](../decisions/ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md) deposited. Design frozen/approved. Historical ADR-048 / 048R1 / 048R2 bodies not rewritten. |
| **What was deliberately NOT implemented** | Deployment-controller implementation; durable lease implementation; secret-delivery product selection; disposable live validation; production credentials; App/VPC/OCI/Temporal-key mutation; production deployment. |
| **Governing decisions** | ADR-AIEOS-049 (current detailed deployment-plane behavior); ADR-AIEOS-048R2 (ownership boundary); ADR-AIEOS-048R1 (naming); ADR-AIEOS-048 (base topology). |
| **Current status** | **Architecture Frozen / Approved.** Design frozen; implementation and disposable empirical validation = **REQUIRED / NOT AUTHORIZED**. Production App Platform deployment **not authorized**. Release-controller implementation architecture = [ADR-AIEOS-050](../decisions/ADR-AIEOS-050-aieos-app-platform-release-controller-implementation-architecture.md). |

---

## 17. ADR-AIEOS-050 — App Platform release controller implementation architecture

| Field | Content |
|-------|---------|
| **Objective** | Freeze the WPI-AP-DP02 release-controller implementation architecture required to realize ADR-AIEOS-049: Infrastructure-isolated Python 3.14 / `uv` tool; GitHub-hosted `ubuntu-24.04` only; two fixed `workflow_dispatch` workflows and Environments; per-App durable lease via Actions concurrency; direct DigitalOcean v2 REST via controlled `httpx` with zero automatic mutation retries; closed typed mutation allowlist; four PAT classes; Environment secret-delivery boundary; sanitized `release-receipt.json` (90-day retention); credential-free offline CI; mandatory WPI-AP-DP-TV01 before production. |
| **Architectural reason** | ADR-AIEOS-049 froze deployment-plane behavior but left placement, runtime, workflow/Environment identities, lease technology, secret delivery, HTTP posture, credential scopes, evidence sink, and offline CI proof areas unselected; those must be frozen before separated implementation gates. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-050](../decisions/ADR-AIEOS-050-aieos-app-platform-release-controller-implementation-architecture.md) deposited. WPI-AP-DP02 design complete/frozen. Historical ADR-048 / 048R1 / 048R2 / 049 bodies not rewritten. |
| **What was deliberately NOT implemented** | Controller source; workflow YAML; GitHub Environments/secrets; DigitalOcean PATs; Temporal keys; WPI-AP-DP-TV01; production App bootstrap; any cloud/state mutation. |
| **Governing decisions** | ADR-AIEOS-050 (current release-controller implementation architecture); ADR-AIEOS-049 (deployment-plane behavior); ADR-AIEOS-048R2 (ownership boundary); ADR-AIEOS-048R1 (naming); ADR-AIEOS-048 (base topology). |
| **Current status** | **Architecture Frozen / Approved.** WPI-AP-DP02 = **DESIGN COMPLETE / FROZEN**; implementation = **NOT AUTHORIZED**; WPI-AP-DP-TV01 = **AUTHORIZED BUT PAUSED ON OCI MANIFEST DIGEST** (see ADR-AIEOS-051); production App deployment **not authorized**. |

---

## 18. ADR-AIEOS-051 — Backend production OCI build, provenance & first-publication architecture

| Field | Content |
|-------|---------|
| **Objective** | Freeze the production Backend OCI build / provenance / first-publication architecture: one common `eduvijna-registry` / `aieos-backend` image for WORKFLOW_DISPATCHER and TEMPORAL_WORKER; exact clean Backend Git SHA source; future `Dockerfile.backend-runtime` (Python 3.14.7 / uv 0.12.4 / non-root / fail-closed default); OCI identity labels; sanitized provenance receipt; source-SHA tag convenience with immutable digest authority; separated WPI-OCI-I01 (source/offline) and WPI-OCI-P01 (live publication) gates. |
| **Architectural reason** | ADR-048/049/050 require immutable OCI digest authority for App Platform release, but no production OCI build/provenance/publication architecture was frozen; TV01 is blocked on that digest. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-051](../decisions/ADR-AIEOS-051-aieos-backend-production-oci-build-provenance-first-publication-architecture.md) deposited. Historical ADR-048 / 048R1 / 048R2 / 049 / 050 bodies not rewritten. |
| **What was deliberately NOT implemented** | Production Dockerfile; provenance tooling; offline CI; registry login; publication credential; OCI push/promote; TV01 App CREATE; App Platform mutation; production deployment. |
| **Governing decisions** | ADR-AIEOS-051 (CURRENT Backend production OCI architecture); ADR-AIEOS-048 (base OCI delivery); ADR-AIEOS-049 / 050 (digest-consuming release plane). |
| **Current status** | **Architecture Frozen / Approved.** Production Backend OCI architecture = **DESIGN FROZEN**. **WPI-OCI-I01** = AUTHORIZED **SOURCE / OFFLINE IMPLEMENTATION ONLY** (after deposition merge). **WPI-OCI-P01** = **NOT AUTHORIZED**. **WPI-AP-DP-TV01** = **AUTHORIZED BUT PAUSED ON OCI MANIFEST DIGEST**. Production deployment **not authorized**. |

---

## 19. ADR-AIEOS-052 — Preparation kit & multi-artifact generation architecture

| Field | Content |
|-------|---------|
| **Objective** | Freeze architecture for Teacher OS TOS-DEV04 Prepare Tomorrow: one outcome action → `education.generate_preparation_kit` → atomic six-artifact Generic Content materialization; provenance V2 with mandatory `artifact_kind`; capability/revision-aware GenerationRun fences (DEV03 worksheet coexistence); FAILED terminal replay vs stale RUNNING reclaim; no PreparationKit aggregate; no AI payload staging; no `generation_artifacts` canonical bridge. |
| **Architectural reason** | Teacher OS ADR-047 deferred multi-artifact Prepare Tomorrow orchestration pending architecture review; TOS-DEV03 proved single-worksheet path but cannot express one execution → six governed artifacts without material architecture decisions. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-052](../decisions/ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) deposited and Frozen / Approved **2026-08-28**. |
| **What was deliberately NOT implemented** | Backend; Frontend; migrations; OpenAPI; live provider proof; production Content catalog activation. |
| **Governing decisions** | ADR-AIEOS-052 (CURRENT multi-artifact Prepare Tomorrow architecture); ADR-AIEOS-027 (Content authority); ADR-AIEOS-026 (no workflow as SoR); Teacher OS ADR-044–048 (outcome-first, review semantics). |
| **Current status** | **Architecture Frozen / Approved.** Multi-artifact Prepare Tomorrow architecture = **DESIGN FROZEN**. **TOS-DEV04 implementation = NOT AUTHORIZED / NOT IMPLEMENTED.** Live provider proof requires separate gate (**DEV04-I10**). |

---

## 20. ADR-AIEOS-053 — Teaching Assignment & Classroom Delivery Authority

| Field | Content |
|-------|---------|
| **Objective** | Freeze architecture for Teacher OS TOS-DEV06: Publication ≠ Assignment; TeachingAssignment Teaching-domain intent SoR; immutable exact ContentVersion bind with published_version_id precondition; ClassRef via AIEOS School Context façade; command Idempotency-Key semantics; ACTIVE/CLOSED/CANCELLED; no LMS delivery-attempt persistence in DEV06 core; narrow clarification of ADR-046 / ADR-AIEOS-052 delivery wording only. |
| **Architectural reason** | Implemented Teacher OS publish path is governance eligibility, not classroom delivery; Product/ADR wording that collapses Published with assign/send must not remain current precedence; Class/Roster remain Admin/ERP/SIS masters. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-053](../decisions/ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) Frozen / Approved by Founder **2026-08-31**. |
| **What was deliberately NOT implemented** | Backend; Frontend; Product; migrations; OpenAPI; School Context provider; LMS connector; deployment; production mutation. |
| **Governing decisions** | ADR-AIEOS-053 (Frozen / Approved); ADR-AIEOS-027 (Content / Publication); ADR-AIEOS-052 (preparation; delivery wording clarified); ADR-046 (lifecycle vocabulary; Published wording clarified); ADR-048 (Review Queue). |
| **Current status** | **Architecture Frozen / Approved.** Founder approved **2026-08-31**. Architecture freeze ≠ implementation. **DEV06-I01+ implementation = NOT AUTHORIZED.** |

---

## 21. ADR-AIEOS-046R1 — Production Event Plane Multi-Domain Publisher Scope Revision

| Field | Content |
|-------|---------|
| **Objective** | Narrow forward revision of ADR-AIEOS-046: expand production EVENT publisher PUB authority from Content-only to a closed multi-domain set (Content + Teaching) required by ADR-AIEOS-053 TeachingAssignment events; preserve all other ADR-AIEOS-046 invariants; forbid platform-wide `io.eduvijna.aieos.>` publisher wildcard. |
| **Architectural reason** | ADR-AIEOS-053 requires TeachingAssignment mutation events under `io.eduvijna.aieos.teaching....`; ADR-AIEOS-046 A46-INV-03 limited publisher scope to Content only; minimum Teaching-domain permission added without granting unrelated domain authority. |
| **What was implemented** | Architecture source only: [ADR-AIEOS-046R1](../decisions/ADR-AIEOS-046R1-aieos-production-event-plane-multi-domain-publisher-scope-revision.md) Frozen / Approved by Founder **2026-08-31**. |
| **What was deliberately NOT implemented** | Backend; Infrastructure; NATS mutation; credential creation/regeneration; stream creation/update; DigitalOcean mutation; deployment; production EVENT execution; TeachingAssignment application/API; OpenAPI; Frontend; LMS integration. |
| **Governing decisions** | ADR-AIEOS-046R1 (Frozen / Approved); ADR-AIEOS-046 (historical/base); ADR-AIEOS-053 (TeachingAssignment event requirement); ADR-AIEOS-025 (outbox/CloudEvents). |
| **Current status** | **Architecture Frozen / Approved.** Founder approved **2026-08-31**. ADR-AIEOS-046 historical body unchanged. **TOS-DEV06-I03 = NOT AUTHORIZED.** |

---

## Gaps / missing chronology

Where older pre-Teacher-OS platform history (earlier Platform AI packages, ERP modules, etc.) is relevant but not part of this AIEOS journey spine: **Not established by current repository evidence** as a fully sequenced AIEOS chronology in this folder — treat as adjacent capability history under product/API repos.
