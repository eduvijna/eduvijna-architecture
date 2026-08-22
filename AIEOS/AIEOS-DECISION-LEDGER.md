# AIEOS — Decision Ledger

**Purpose:** Consolidated index/summary of major architecture decisions for EduVijna AIEOS  
**Nature:** Index only — **does not replace ADRs or EDRs**

---

## Authority note

Conflict preference:

1. Approved architecture decisions (authoritative detail lives in ADR/EDR files)  
2. Approved product / engineering documents  
3. Current source code / contracts  
4. AIEOS orientation documents

Do not invent decision IDs. Use verified ADR/EDR IDs when available.

`ADR-AIEOS-*` and Teacher OS `ADR-042`–`ADR-048` are distinct ID families.

---

## Ledger

| ID | Decision | Status | Related EBP | Related ADR/EDR | Notes |
|----|----------|--------|-------------|-----------------|-------|
| ADR-042 | Teacher OS Shell owns UX, not business capabilities | Accepted | EBP-001.1 | ADR-042 · EDR-002 | Generators/reports/analytics stay in existing modules |
| ADR-043 | Stable foundations before features (Foundation → Hardening → Review → Next) | Accepted | EBP-001 / EBP-000 | ADR-043 | Avoid feature→feature→refactor |
| ADR-044 | AI Platform behind stable product services; frontend never calls Agents/MCP directly | Accepted | EBP-001 | ADR-044 | Mission/Intent/Artifact façades; provider independence |
| ADR-045 | Teaching Intent owns goals; generators are capabilities | Accepted | EBP-001.3 | ADR-045 | Constitutional for Intent UX |
| ADR-046 | One Artifact status lifecycle for every type | Accepted | EBP-001.5+ | ADR-046 | Draft→…→Archived; no exceptions |
| ADR-047 | Outcome-first teaching language (Prepare Tomorrow) | Accepted | EBP-001.3 | ADR-047 | Not generator-menu primary model |
| ADR-048 | Review Queue owns approval (teacher judgement only); Approved ≠ Published | Accepted | EBP-001.5 | ADR-048 | Queue does not own generation/orchestration |
| EDR-001 | Use React Context for session-scoped Continuous Context | Accepted | EBP-001.6 | EDR-001 | Implementation only; PA session semantics unchanged |
| EDR-002 | Teacher OS Shell foundation implementation choices | Accepted | EBP-001.1 | EDR-002 | Flag provider, keep MainLayout, additive routes |
| — | Teacher OS uses existing Quiz-React repository (no new app) | Approved (EBP-001) | EBP-001 | — | Evolve existing eduvijna-web |
| — | Teacher OS consumes stable product services | Approved | EBP-001 | ADR-044 | Mocks today; real APIs later |
| — | Frontend does not directly call Agents/MCP | Approved | EBP-001 | ADR-044 | Binding for Teacher OS |
| — | Continuous Context is distinct from Teacher OS Context (shell selection) and Teacher Memory | Approved (PA + EDRs) | EBP-001.6 / 001.8 | EDR-001 · EDR-002 | Different domains |
| — | Teacher/School Context is not Teacher Memory | Approved (EBP-001.8 package) | EBP-001.8 | — | Read surface only |
| — | Existing generators should remain operational | Approved (EBP-001) | EBP-001 | ADR-042 | Behind flags / legacy paths |
| — | EBP-001.9 needs durable Content SoR for production Review Queue | Discovery finding | EBP-001.9 | — | Scenario C: generic SoR missing |
| — | DB creation for Content SoR is NOT yet authorized | Under architecture review | EBP-001.9 | — | Persistence preflight: DB CHANGE REQUIRED; no migrate authorization |
| EBP-000 | Engineering Constitution v1.0 frozen | Frozen | EBP-000 → EBP-001 | — | Implementation standards; not an ADR |

---

## ADR catalogue (Teacher OS set)

Authoritative records: `eduvijna-architecture/decisions/`

| ADR | Title |
|-----|-------|
| ADR-042 | Teacher OS Shell owns UX |
| ADR-043 | Stable foundations before features |
| ADR-044 | AI Platform behind stable product services |
| ADR-045 | Teaching Intent owns goals |
| ADR-046 | Artifact Status Lifecycle |
| ADR-047 | Outcome-first Prepare Tomorrow |
| ADR-048 | Review Queue owns approval |

Chronological index: `decisions/DECISION_LOG.md`

---

## ADR catalogue (AIEOS platform set)

Authoritative records: `eduvijna-architecture/decisions/ADR-AIEOS-*.md`

Architecture status: **Frozen / Approved**. Not IMPLEMENTED / MERGED / DEPLOYED / PRODUCTION.

Historical ADR-AIEOS-023 remains Frozen / Approved (Identity/Tenant/Security); its original canonical body is unavailable and is not reconstructed. ADR-AIEOS-023R1 is the Frozen / Approved canonical restatement and the usable identity/tenant/security implementation baseline. Architecture catalogue deposition does not by itself authorize identity implementation or production promotion.

| ADR | Title | Status | Notes |
|-----|-------|--------|-------|
| [ADR-AIEOS-022](../decisions/ADR-AIEOS-022-aieos-platform-technology-baseline.md) | AIEOS Platform Technology Baseline | Frozen / Approved | Event broker and workflow engine deferred here; later ADRs own those selections |
| ADR-AIEOS-023 | Historical Identity/Tenant/Security | Frozen / Approved | Original canonical body unavailable; restated by ADR-AIEOS-023R1 |
| [ADR-AIEOS-023R1](../decisions/ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | AIEOS Identity, Tenant & Security Canonical Restatement | Frozen / Approved | Canonical future implementation baseline; does not reproduce lost original 023 text |
| [ADR-AIEOS-024](../decisions/ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | AIEOS Data, Resource & SoR Implementation Baseline | Frozen / Approved | |
| [ADR-AIEOS-025](../decisions/ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | AIEOS API Contract & Integration Implementation Baseline | Frozen / Approved | NATS JetStream; domain never publishes NATS directly |
| [ADR-AIEOS-026](../decisions/ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | AIEOS Workflow Implementation Baseline | Frozen / Approved | Temporal; workflow state ≠ domain state |
| [ADR-AIEOS-027](../decisions/ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | AIEOS Generic Content Implementation Baseline | Frozen / Approved | Approved ≠ Published |
| [ADR-AIEOS-028](../decisions/ADR-AIEOS-028-security-audit-mutation-accountability.md) | Security Audit & Mutation Accountability | Frozen / Approved | Audit ≠ event ≠ log ≠ authorization |
| [ADR-AIEOS-029](../decisions/ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Production Environment & Deployment Readiness Baseline | Frozen / Approved | Deploy/live/ready ≠ mutation enabled |
| [ADR-AIEOS-030](../decisions/ADR-AIEOS-030-production-jwt-bearer.md) | Production JWT Bearer | Frozen / Approved | Authentication only; principal_id only |
| [ADR-AIEOS-031](../decisions/ADR-AIEOS-031-production-authorization-kernel.md) | Production Authorization Kernel | Frozen / Approved | ALLOW/DENY; default DENY |
| [ADR-AIEOS-032](../decisions/ADR-AIEOS-032-governance-adapter-foundation.md) | Governance Adapter Foundation | Frozen / Approved | Governance ≠ authorization |
| [ADR-AIEOS-033](../decisions/ADR-AIEOS-033-asset-file-architecture.md) | Asset/File Architecture | Frozen / Approved | Provider-neutral BlobStore invariants; later ADRs select provider and topology |
| [ADR-AIEOS-034](../decisions/ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | AIEOS Asset Current-Use Authority Decision Semantics | Frozen / Approved | |
| [ADR-AIEOS-035](../decisions/ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | AIEOS Asset Mutation & Revision Activation Semantics | Frozen / Approved | No HTTP / audit / events in this ADR |
| [ADR-AIEOS-036](../decisions/ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) | Asset Authorization & Transactional Security Audit Baseline | Frozen / Approved | Asset remains NON_PRODUCTION |
| [ADR-AIEOS-036R1](../decisions/ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) | Asset Security-Audit Resource Revision Semantics | Frozen / Approved | Refines 036; does not replace it |
| [ADR-AIEOS-037](../decisions/ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | AIEOS Production Infrastructure Baseline | Frozen / Approved | DigitalOcean production cloud; Spaces not selected |
| [ADR-AIEOS-038](../decisions/ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | AIEOS Cross-Cloud Asset Object Storage Exception | Frozen / Approved | Historical approved cross-cloud exception; dormant for first production |
| [ADR-AIEOS-038R1](../decisions/ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | AIEOS DigitalOcean-Only Asset Storage Direction | Frozen / Approved | Current first-production Asset-storage hosting direction |
| [ADR-AIEOS-039](../decisions/ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | AIEOS Asset BlobStore Provider Selection | Frozen / Approved | MinIO AIStor provider selection |
| [ADR-AIEOS-040](../decisions/ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) | AIEOS Asset BlobStore First-Production Topology | Frozen / Approved | HISTORICAL topology decision; original 8×1 First Production classification; later reclassified by ADR-AIEOS-040R1 as Scale Production |
| [ADR-AIEOS-040R1](../decisions/ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) | AIEOS Asset BlobStore Bootstrap & Scale Production Topology | Frozen / Approved | CURRENT topology classification: Bootstrap + Scale Production |
| [ADR-AIEOS-041](../decisions/ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) | AIEOS Asset Backup & Recovery Architecture | Frozen / Approved | Base backup/recovery architecture; SFO3 Spaces Standard backup-only; non-authoritative; Versioning; verified ≤1h |
| [ADR-AIEOS-041R1](../decisions/ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) | AIEOS Asset Backup Execution, Manifest & Recovery Authority | Frozen / Approved | Forward revision: PG backup-job SoR; signed-manifest PG authority (JCS+Ed25519); no required Bootstrap Asset events; 7-day PITR Phase-0 |
| [ADR-AIEOS-042](../decisions/ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) | AIEOS Asset Binary Delivery & Bootstrap Media Profile | Frozen / Approved | API-mediated streaming; 32 MiB Bootstrap ceiling; image/document/audio; no Bootstrap video |
| [ADR-AIEOS-043](../decisions/ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md) | AIEOS Bootstrap AIStor Service Boundary & Primary Namespace | Frozen / Approved | Private-only AIStor; TLS trust; no Bootstrap LB; one primary bucket/env; Versioning OFF; Object Lock OFF |
| [ADR-AIEOS-044](../decisions/ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) | AIEOS Bootstrap Production Pre-Apply Execution Baseline | Frozen / Approved | Pre-apply freezes (NATS/VPC/state/identity/TLS/admin); commercial RED evidence ≈ USD 294.05; architecture freeze ≠ apply |
| [ADR-AIEOS-044R1](../decisions/ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md) | AIEOS Production State Namespace Collision Resolution | Frozen / Approved | Historical: superseded BLR1 literal `eduvijna-aieos-tofu-state-prod-blr1`; NYC3 collision HOLD |
| [ADR-AIEOS-044R2](../decisions/ADR-AIEOS-044R2-aieos-production-state-region-availability-resolution.md) | AIEOS Production State Region Availability Resolution | Frozen / Approved | Forward state authority = SFO3 / `eduvijna-aieos-tofu-state-prod-sfo3`; workload remains BLR1; Stages 1–2 complete; Stage 3A refresh-only/live-lock validation complete; Stage 3B first authoritative tfstate materialized (serial 1; zero managed resources); workload apply unauthorized |
| [ADR-AIEOS-045](../decisions/ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) | AIEOS Dispatcher Tenant-Candidate Discovery Authority | **Proposed / Awaiting Chief Architect Freeze** | Pending-work candidate discovery from outbox/intent queues; dedicated NOLOGIN NOBYPASSRLS candidate-reader identities; SECURITY DEFINER owned by candidate-reader not schema owner; no dispatcher BYPASSRLS; no cross-tenant payload visibility; does not authorize implementation or migration |

[ADR-AIEOS-037](../decisions/ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) remains Frozen / Approved (DigitalOcean production cloud baseline). [ADR-AIEOS-038](../decisions/ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) remains Frozen / Approved as a historical cross-cloud exception. [ADR-AIEOS-038R1](../decisions/ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) is Frozen / Approved as the current first-production Asset storage hosting direction. [ADR-AIEOS-039](../decisions/ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) is Frozen / Approved as the Asset BlobStore software provider (MinIO AIStor, AIEOS-operated). [ADR-AIEOS-040](../decisions/ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) remains Frozen / Approved as the **historical** 8×1 topology decision (originally First Production; empirically validated by PED-I10B7E-TV04-R2 in NON_PRODUCTION). [ADR-AIEOS-040R1](../decisions/ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) is Frozen / Approved as the **current** topology classification. [ADR-AIEOS-041](../decisions/ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) remains Frozen / Approved as the base Asset Backup & Recovery Architecture. [ADR-AIEOS-041R1](../decisions/ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) is Frozen / Approved as the forward revision for backup execution, signed-manifest, Bootstrap Asset events, and PITR Phase-0. [ADR-AIEOS-042](../decisions/ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) and [ADR-AIEOS-043](../decisions/ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md) are Frozen / Approved Bootstrap architecture-closure decisions. [ADR-AIEOS-044](../decisions/ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) remains Frozen / Approved as the historical Bootstrap pre-apply execution baseline. [ADR-AIEOS-044R1](../decisions/ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md) remains Frozen / Approved as the historical namespace-collision revision. [ADR-AIEOS-044R2](../decisions/ADR-AIEOS-044R2-aieos-production-state-region-availability-resolution.md) is Frozen / Approved as the forward revision moving OpenTofu production-state location to **SFO3**. DigitalOcean Spaces remains **REJECTED** as the authoritative primary Asset BlobStore; Spaces Standard SFO3 is approved for non-authoritative backup/recovery and (separately) for OpenTofu control-plane state. Amazon S3 is **no longer advancing for first production**.

**Current active summary:**

- **Bootstrap:** single-node AIStor Free; 6 × ~190 GiB Volumes; N=6 / K=3 / M=3; EC:3; BLR1; private-service-only; no dedicated AIStor LB
- **Binary delivery:** authenticated App Platform API → private AIStor; 32 MiB ceiling; `asset.image` / `asset.document` / `asset.audio`; video not authorized
- **Primary namespace:** one dedicated primary bucket per environment; Versioning OFF; Object Lock OFF; ordinary runtime delete forbidden
- **Scale:** ADR-AIEOS-040 8×1 distributed topology; EC:3
- **Backup:** SFO3 Spaces Standard; backup-only; non-authoritative; Versioning; verified ≤1h; PostgreSQL backup-job SoR; signed manifest (JCS + Ed25519) in PostgreSQL; 7-day Managed PostgreSQL PITR Phase-0
- **Production workload region:** **BLR1** (VPC / AIStor / NATS / PostgreSQL / App Platform remain BLR1 unless separately revised)
- **Production OpenTofu state region:** **SFO3** — bucket `eduvijna-aieos-tofu-state-prod-sfo3` = **CREATED / PRIVATE / VERSIONED** (project AIEOS); endpoint `https://sfo3.digitaloceanspaces.com`
- **Production OpenTofu backend:** **INITIALIZED** (OpenTofu 1.12.5; S3; `use_lockfile=true`); remote tfstate object = **MATERIALIZED / AUTHORITATIVE** (key `environments/production/opentofu.tfstate`; serial **1**; managed resources **0**; `tofu state list` **EMPTY**); current persistent lock object = **ABSENT**; native lock cycle = **VALIDATED**; bounded Stage 3A refresh-only plan and Stage 3B refresh-only saved-plan apply = **EXECUTED / CLOSED**; normal production workload plan / further production apply = **NOT AUTHORIZED**
- **Pre-apply execution (ADR-AIEOS-044 / 044R1 / 044R2):** NATS single-node Bootstrap; VPC `aieos-prod-blr1` / `10.130.0.0/20`; prior BLR1 state target `eduvijna-aieos-tofu-state-prod-blr1` = PLANNED / NEVER CREATED / SUPERSEDED; NYC3 Space `eduvijna-aieos-tofu-state-prod` = UNATTRIBUTED / PRE-EXISTING / NON-AUTHORITATIVE / HOLD; **Stage 1 = PASS / FORMALLY CLOSED**; **Stage 2 = PASS / FORMALLY CLOSED**; **Stage 3A = PASS / FORMALLY CLOSED**; **Stage 3B = PASS / FORMALLY CLOSED**; permanent bucket-scoped `readwrite` state credential established outside Git; temporary fullaccess provisioning key destroyed; no DigitalOcean workload mutation during Stages 3A/3B; `DIGITALOCEAN_TOKEN` not used; stable hostname→RFC1918 identity; Recovery Console admin; Smallstep offline CA; App Platform sizing implementation-gated
- **Commercial envelope (architecture):** Bootstrap operating target ≤ USD 240/month; hard ceiling USD 250/month. 2026-08-21 planning evidence projects ≈ USD 294.05/month pre-tax (**RED**). Architecture freeze does **not** authorize spend above the hard ceiling. Complete first-production DigitalOcean apply remains commercially blocked until release condition in ADR-AIEOS-044 is satisfied.

Still open (not authorized / not frozen by this catalogue): **dispatcher tenant-candidate discovery authority** ([ADR-AIEOS-045](../decisions/ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) proposed, not yet frozen); event/workflow dispatcher daemon implementation; scheduled/reconciliation runtime ownership; production BlobStore runtime composition / production credential conformance; actual Asset HTTP implementation; backup worker implementation; backup schema/migrations; production entrypoints / App Platform composition; normal production workload plan; further production apply; VPC / AIStor / NATS / Managed PostgreSQL / App Platform / Temporal production creation; production schema-owner readiness; Asset production runtime composition; production deployment; commercial release (≤250 or new Founder ceiling); production migration; production mutation; PED-I03 Asset mutation activation; physical purge/retention/legal hold; Scale Production commercial purchase/entitlement execution; DOKS retirement.

---

## EDR catalogue (implementation)

Authoritative records: `eduvijna-product/engineering/edrs/`

| EDR | Title |
|-----|-------|
| EDR-001 | Continuous Context via React Context |
| EDR-002 | Teacher OS Shell Foundation |

---

## Explicit non-decisions (do not treat as authorized)

| Topic | Status |
|-------|--------|
| Create generic `edu.contents` / Content SoR DB | Not authorized — under architecture review |
| Introduce Agents / MCP into Teacher OS frontend | Rejected by ADR-044 |
| Treat EBP-001.8 as Teacher Memory | Explicitly not |
| Treat discovery recommendations as ADR | Invalid — discovery ≠ ADR |
| Rename Teacher OS to AIEOS | Invalid — Teacher OS remains subsystem name |

---

## Related

- `AIEOS-MASTER-CONSTITUTION.md`
- `AIEOS-CURRENT-STATE.md`
- `AIEOS-ARCHITECTURE-JOURNEY.md`
