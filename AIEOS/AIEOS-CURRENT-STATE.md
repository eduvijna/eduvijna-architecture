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

The AIEOS platform ADR family is now canonically deposited under `decisions/ADR-AIEOS-*`. Teacher OS `ADR-042`–`ADR-048` remain a distinct ID family. Teacher OS **ADR-047** (Outcome-first Prepare Tomorrow) is not platform **ADR-AIEOS-047** (Production Workflow Plane Identity & Least-Privilege Contract). Teacher OS **ADR-048** (Review Queue owns approval) is not platform **ADR-AIEOS-048** (First-Production App Runtime & OCI Delivery Contract).

Historical ADR-AIEOS-023 remains Frozen / Approved (Identity/Tenant/Security), but its original body is unavailable. ADR-AIEOS-023R1 is now the canonical restated identity/tenant/security implementation baseline. This closes the architecture-catalogue identity/tenant/security gap. It does **not** authorize identity implementation or production promotion by itself.

Current Asset / platform implementation remains **NON_PRODUCTION**. Architecture catalogue synchronization does **not** authorize production deployment or mutation.

**Production infrastructure (architecture + execution status):** [ADR-AIEOS-037](../decisions/ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) is Frozen / Approved (DigitalOcean production cloud baseline). [ADR-AIEOS-038](../decisions/ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) is Frozen / Approved as a historical cross-cloud exception. [ADR-AIEOS-038R1](../decisions/ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) is Frozen / Approved as the current first-production Asset storage hosting direction (DigitalOcean-only). [ADR-AIEOS-039](../decisions/ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) is Frozen / Approved as the Asset BlobStore **software provider** (MinIO AIStor, AIEOS-operated). [ADR-AIEOS-040](../decisions/ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) remains Frozen / Approved as the **historical** 8×1 topology decision (originally First Production; empirically validated by PED-I10B7E-TV04-R2 in NON_PRODUCTION). [ADR-AIEOS-040R1](../decisions/ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) is Frozen / Approved as the **current** topology classification: Bootstrap (single-node AIStor Free; 6 × ~190 GiB Volumes; N=6 / K=3 / M=3; EC:3; BLR1) and Scale (ADR-AIEOS-040 8×1 distributed; EC:3). [ADR-AIEOS-041](../decisions/ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) remains Frozen / Approved as the base Asset Backup & Recovery Architecture. [ADR-AIEOS-041R1](../decisions/ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) is Frozen / Approved as the forward revision (PostgreSQL backup-job SoR; signed-manifest PG authority; no required Bootstrap Asset business events; seven-day Managed PostgreSQL PITR Phase-0). [ADR-AIEOS-042](../decisions/ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) is Frozen / Approved (API-mediated streaming; 32 MiB Bootstrap ceiling; image/document/audio; video not authorized). [ADR-AIEOS-043](../decisions/ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md) is Frozen / Approved (private-only AIStor service boundary; TLS trust; no Bootstrap LB; one primary bucket/env; Versioning OFF; Object Lock OFF). [ADR-AIEOS-044](../decisions/ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) remains Frozen / Approved as the historical Bootstrap pre-apply execution baseline (NATS/VPC/state/identity/TLS/admin freezes; commercial release condition). [ADR-AIEOS-044R1](../decisions/ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md) remains Frozen / Approved as the historical namespace-collision revision. [ADR-AIEOS-044R2](../decisions/ADR-AIEOS-044R2-aieos-production-state-region-availability-resolution.md) remains Frozen / Approved as the forward OpenTofu production-state location authority: **production workload region remains BLR1**; **production OpenTofu state region is SFO3**; production state bucket `eduvijna-aieos-tofu-state-prod-sfo3` = **CREATED / PRIVATE / VERSIONING ENABLED** (project AIEOS); permanent bucket-scoped `readwrite` state credential established outside Git; temporary fullaccess provisioning key destroyed. **Stage 1 = PASS / FORMALLY CLOSED.** **Stage 2 = PASS / FORMALLY CLOSED** (OpenTofu 1.12.5 S3 backend initialized against `https://sfo3.digitaloceanspaces.com` with `use_lockfile=true`). **Stage 3A = PASS / FORMALLY CLOSED** (bounded refresh-only production plan; native S3 live-lock cycle validated; zero managed resources; no DigitalOcean workload mutation; `DIGITALOCEAN_TOKEN` not used). **Stage 3B = PASS / FORMALLY CLOSED** (exact inspected refresh-only saved-plan apply; 0 added / 0 changed / 0 destroyed; first authoritative remote tfstate materialized). **Remote tfstate object = MATERIALIZED / AUTHORITATIVE** (key `environments/production/opentofu.tfstate`; initial state serial = **1**; managed resource count = **0**; `tofu state list` = **EMPTY**); current persistent lock object = **ABSENT**; native lock cycle = **VALIDATED**. Prior BLR1 target `eduvijna-aieos-tofu-state-prod-blr1` = **PLANNED / NEVER CREATED / SUPERSEDED**. NYC3 Space `eduvijna-aieos-tofu-state-prod` remains **UNATTRIBUTED / PRE-EXISTING / NON-AUTHORITATIVE / HOLD**. Bounded Stage 3A refresh-only plan and Stage 3B refresh-only saved-plan apply = **EXECUTED / CLOSED**; normal production workload plan / further production apply = **NOT AUTHORIZED**. `enable_cloud_resources` remains **false**. Production workload apply = **BLOCKED**. Commercial blocker = **IN FORCE**. DigitalOcean Spaces remains **REJECTED** as the authoritative primary Asset BlobStore. Amazon S3 is **no longer advancing for first production**. AWS-BOOT-01 is cancelled / closed without implementation. PED-I10B7C-TV02 is cancelled before AWS resource creation. Raw validation artifacts are not canonical repository files.

**Commercial envelope (architecture only):** Bootstrap operating target ≤ USD 240/month; hard ceiling USD 250/month. 2026-08-21 planning evidence projects ≈ USD 294.05/month pre-tax (**RED**). Architecture freeze does **not** authorize production spend; complete first-production DigitalOcean apply remains blocked until ADR-AIEOS-044 commercial release condition is met. `ARCHITECTURE FREEZE != PRODUCTION APPLY AUTHORIZATION.`

**Dispatcher tenant-candidate discovery:** [ADR-AIEOS-045](../decisions/ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) is **Frozen / Approved**. Pending-work candidate discovery from `integration.outbox_messages` and workflow intent queues; dedicated NOLOGIN NOBYPASSRLS candidate-reader identities (`aieos_event_candidate_reader`, `aieos_workflow_candidate_reader` conceptually); SECURITY DEFINER functions owned by candidate-reader not schema owner; dispatcher login remains NOBYPASSRLS with EXECUTE-only access; no cross-tenant payload visibility. Architecture freeze does **not** authorize dispatcher daemon implementation, database candidate-function migration, production candidate-reader role provisioning, or production deployment.

**Production event plane identity & least privilege:** [ADR-AIEOS-046](../decisions/ADR-AIEOS-046-aieos-production-event-plane-identity-least-privilege-contract.md) is **Frozen / Approved**.

| Concern | Frozen value |
|---------|--------------|
| Production stream | `AIEOS_EVENTS_PROD` |
| Production stream subjects | `io.eduvijna.aieos.>` |
| EVENT publisher | `io.eduvijna.aieos.content.>` |
| EVENT publisher SUB | `_INBOX.>` (+ publish ACK response semantics only) |
| EVENT credential | `AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS` |
| EVENT auth | JWT + NKey `.creds` / in-memory `user_jwt_cb` + `signature_cb` |
| Secret delivery | App Platform encrypted env |
| Stream administration | separate `streamadmin` |
| EVENT `$JS.API` stream-admin | **NONE** |

Historical ADR-025 modular-first examples (`AIEOS_EVENTS`, `aieos.event.v1.>`) are not current production authority.

**Backend EVENT dispatcher runtime source (PED-I11) = IMPLEMENTED / MERGED.** Evidence Backend `origin/main` at this WPI-SF01-A gate: `8e837d2ef723db468e18b0405cb8bbc039efa8c2`. PED-I11 is Backend source only — not deployed and not activated.

Production boundary preserved:

| Concern | Status |
|---------|--------|
| Production EVENT execution | **NOT AUTHORIZED** |
| Production NATS access | **NOT AUTHORIZED** |
| Production NATS credential issuance/injection | **NOT AUTHORIZED** |
| Production stream creation/mutation | **NOT AUTHORIZED** |
| Production DB access/migration | **NOT AUTHORIZED** |
| Production candidate-reader provisioning | **NOT AUTHORIZED** |
| DigitalOcean / OpenTofu / App Platform deployment | **NOT AUTHORIZED** |
| Production deployment | **NOT AUTHORIZED** |

Architecture freeze does **not** authorize production NATS provisioning, production credentials, production stream creation, DigitalOcean mutation, or deployment.

**Production workflow plane identity & least privilege:** [ADR-AIEOS-047](../decisions/ADR-AIEOS-047-aieos-production-workflow-plane-identity-least-privilege-contract.md) is **ARCHITECTURE FROZEN / APPROVED** (not implemented / provisioned / deployed / production-ready / activated). Distinct from Teacher OS ADR-047.

| Concern | Frozen value |
|---------|--------------|
| Production hosting | Temporal Cloud (specialization of ADR-AIEOS-026; ADR-026 not rewritten) |
| Connection mode | Temporal Cloud Namespace Endpoint (exact hostname = provisioning output) |
| Namespace topology | Environment-isolated; not per-tenant |
| Task queues | Capability-oriented; current `aieos.content.review` |
| Workflow type | `ContentReviewWorkflowV1` |
| Signal | `review_decision_recorded` |
| WORKFLOW_DISPATCHER auth | Distinct Temporal Cloud Service Account + API key |
| TEMPORAL_WORKER auth | Distinct Temporal Cloud Service Account + API key (`AIEOS_TEMPORAL_API_KEY` family preserved) |
| Dispatcher Temporal secret env | `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY` |
| Provider RBAC floor | Account Read + target Namespace Write |
| Custom Roles | Not required for first production (Pre-Release as of 2026-08-23) |
| TLS | Required + certificate verification; no plaintext fallback |

**WORKFLOW dispatcher Backend runtime = NOT YET AUTHORIZED.** **Temporal Cloud production provisioning = NOT AUTHORIZED.** **Production execution = NOT AUTHORIZED.** Architecture freeze does **not** authorize Temporal Cloud account/namespace/service-account/API-key creation, production Temporal access, WORKFLOW dispatcher Backend implementation, worker/dispatcher deployment, DigitalOcean mutation, OpenTofu apply, or deployment.

**First-production App runtime & OCI delivery:** [ADR-AIEOS-048](../decisions/ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) is **ARCHITECTURE FROZEN / APPROVED**. Distinct from Teacher OS ADR-048.

| Concern | Frozen value |
|---------|--------------|
| App Platform region / DC | `blr` / `blr1` |
| Production VPC | dedicated `aieos-prod-blr1` / `10.130.0.0/20` (default-blr1 reuse forbidden) |
| WORKFLOW_DISPATCHER app | `eduvijna-aieos-prod-workflow-dispatcher` (worker × 1) |
| TEMPORAL_WORKER app | `eduvijna-aieos-prod-temporal-worker` (worker × 1) |
| Instance size | `apps-s-1vcpu-1gb-fixed` (Preview; bounded Founder/Chief acceptance 2026-08-23) |
| OCI registry / repository | `eduvijna-registry` / `aieos-backend` |
| Artifact authority | immutable digest + governed CI/provenance + explicit promotion gate (`latest` forbidden) |
| Temporal secrets | workload-specific RUN_TIME SECRET (`AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY` / `AIEOS_TEMPORAL_API_KEY`) |
| Activation | independent VPC / AIStor / App Platform workload boundaries |

**App Platform creation / VPC creation / DOCR repository creation / OCI publication / secret injection / OpenTofu plan-apply / production deployment = NOT AUTHORIZED.**

**Still OPEN (not authorized by catalogue):** WORKFLOW dispatcher Backend runtime; Temporal Cloud production provisioning; App Platform / dedicated VPC / governed OCI publication for first-production workers ([ADR-AIEOS-048](../decisions/ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) architecture only); EVENT dispatcher production activation / provisioning / deployment (PED-I11 source = IMPLEMENTED / MERGED; production operating authority remains excluded); database candidate-function migration; production candidate-reader role provisioning; scheduled/reconciliation runtime ownership; production BlobStore runtime composition / production credential conformance; actual Asset HTTP implementation; backup worker implementation; backup schema/migrations; production entrypoints / App Platform composition (API + Temporal worker entrypoints + PED-I11 EVENT dispatcher runtime source implemented in backend; WORKFLOW dispatcher runtime source = NOT YET AUTHORIZED; EVENT production activation/deployment remains excluded); normal production workload plan; further production apply; VPC / AIStor / NATS / Managed PostgreSQL / App Platform / Temporal production creation; production schema-owner readiness; Asset production runtime composition; production deployment; commercial release (≤250 or new Founder ceiling); production migration; production mutation; PED-I03 Asset mutation activation; physical purge/retention/legal hold; Scale Production commercial purchase/entitlement execution; DOKS retirement. Catalogue deposition authorizes none of: purchase, infrastructure apply, AIStor production install, production BlobStore composition / credential activation, Asset HTTP implementation, backup execution, restore, normal workload plan, further apply, or production mutation.

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
