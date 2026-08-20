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
| [ADR-AIEOS-041](../decisions/ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) | AIEOS Asset Backup & Recovery Architecture | Frozen / Approved | SFO3 Spaces Standard backup-only; non-authoritative; Versioning; verified ≤1h |

[ADR-AIEOS-037](../decisions/ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) remains Frozen / Approved (DigitalOcean production cloud baseline). [ADR-AIEOS-038](../decisions/ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) remains Frozen / Approved as a historical cross-cloud exception. [ADR-AIEOS-038R1](../decisions/ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) is Frozen / Approved as the current first-production Asset storage hosting direction. [ADR-AIEOS-039](../decisions/ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) is Frozen / Approved as the Asset BlobStore software provider (MinIO AIStor, AIEOS-operated). [ADR-AIEOS-040](../decisions/ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) remains Frozen / Approved as the **historical** 8×1 topology decision (originally First Production; empirically validated by PED-I10B7E-TV04-R2 in NON_PRODUCTION). [ADR-AIEOS-040R1](../decisions/ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) is Frozen / Approved as the **current** topology classification. [ADR-AIEOS-041](../decisions/ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) is Frozen / Approved as Asset Backup & Recovery Architecture. DigitalOcean Spaces remains **REJECTED** as the authoritative primary Asset BlobStore; Spaces Standard SFO3 is approved only as non-authoritative backup/recovery. Amazon S3 is **no longer advancing for first production**.

**Current active summary:**

- **Bootstrap:** single-node AIStor Free; 6 × ~190 GiB Volumes; N=6 / K=3 / M=3; EC:3; BLR1
- **Scale:** ADR-AIEOS-040 8×1 distributed topology; EC:3
- **Backup:** SFO3 Spaces Standard; backup-only; non-authoritative; Versioning; verified ≤1h

Still open (not authorized / not frozen by this catalogue): production BlobStore adapter implementation; Asset HTTP/binary contract; Asset events/outbox implementation; physical purge/retention/legal hold; Asset schema-owner readiness; Asset production runtime composition; PED-I03 Asset mutation activation; production migration; production mutation; production deployment; OpenTofu apply; production credentials; actual cloud-resource creation; Scale Production commercial purchase/entitlement execution; Asset maximum size.

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
