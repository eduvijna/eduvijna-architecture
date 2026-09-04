# Decisions — Overview

## Purpose

Record significant architecture and engineering decisions for the EduVijna ecosystem in a durable, reviewable form.

## Scope

- Architecture Decision Records (ADRs) governed by EAO process
- The chronological decision log summarising approved decisions
- Traceability from decisions to architecture, standards, and roadmap work

## Ownership

EduVijna Enterprise Architecture Office (EAO). Decision authors may include domain architects; publication remains under EAO stewardship.

## Contents

- `DECISION_LOG.md` — chronological summary of approved architecture decisions
- Individual ADRs when authorised and approved

`ADR-AIEOS-*` and Teacher OS `ADR-042`–`ADR-048` are **distinct ID families**. Do not renumber or merge them.

### Teacher OS (2026-08-10)

| ID | Title |
|----|-------|
| [ADR-042](ADR-042-teacher-os-shell-owns-ux.md) | Shell owns UX — not generators/business logic |
| [ADR-043](ADR-043-stable-foundations-before-features.md) | Foundation → Hardening → Review → Next Capability |
| [ADR-044](ADR-044-ai-platform-behind-stable-services.md) | AI Platform behind stable product services |
| [ADR-045](ADR-045-teaching-intent-owns-goals.md) | Teaching Intent owns goals — generators are capabilities (**constitutional**) |
| [ADR-046](ADR-046-artifact-status-lifecycle.md) | Artifact Status Lifecycle — one lifecycle for every artifact |
| [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) | “Help me prepare tomorrow” — outcome-first language |
| [ADR-048](ADR-048-review-queue-owns-approval.md) | Review Queue owns approval — teacher judgement only |

### AIEOS platform

Architecture status: **Frozen / Approved**. Not production authorized.

Historical ADR-AIEOS-023 Identity/Tenant/Security remains Frozen / Approved; original body unavailable. ADR-AIEOS-023R1 is the transparent canonical restatement.

| ID | Title |
|----|-------|
| [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) | AIEOS Platform Technology Baseline |
| ADR-AIEOS-023 | Historical Identity/Tenant/Security (original body unavailable — no fabricated file) |
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | AIEOS Identity, Tenant & Security Canonical Restatement |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | AIEOS Data, Resource & SoR Implementation Baseline |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | AIEOS API Contract & Integration Implementation Baseline |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | AIEOS Workflow Implementation Baseline |
| [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | AIEOS Generic Content Implementation Baseline |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Security Audit & Mutation Accountability |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Production Environment & Deployment Readiness Baseline |
| [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md) | Production JWT Bearer |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Production Authorization Kernel |
| [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) | Governance Adapter Foundation |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Asset/File Architecture |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | AIEOS Asset Current-Use Authority Decision Semantics |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | AIEOS Asset Mutation & Revision Activation Semantics |
| [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) | Asset Authorization & Transactional Security Audit Baseline |
| [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) | Asset Security-Audit Resource Revision Semantics (refines 036; does not replace it) |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | AIEOS Production Infrastructure Baseline |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | AIEOS Cross-Cloud Asset Object Storage Exception (historical approved; dormant for first production) |
| [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | AIEOS DigitalOcean-Only Asset Storage Direction (current first-production hosting direction; no provider selected) |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | AIEOS Asset BlobStore Provider Selection (MinIO AIStor) |
| [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) | AIEOS Asset BlobStore First-Production Topology (historical 8×1; later reclassified as Scale Production by ADR-AIEOS-040R1) |
| [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) | AIEOS Asset BlobStore Bootstrap & Scale Production Topology (current: Bootstrap single-node Free + Scale 8×1) |
| [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) | AIEOS Asset Backup & Recovery Architecture (SFO3 Spaces Standard backup-only; verified ≤1h) |
| [ADR-AIEOS-041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) | AIEOS Asset Backup Execution, Manifest & Recovery Authority (PG job SoR; signed manifest; 7-day PITR Phase-0) |
| [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) | AIEOS Asset Binary Delivery & Bootstrap Media Profile (API-mediated; 32 MiB; no Bootstrap video) |
| [ADR-AIEOS-043](ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md) | AIEOS Bootstrap AIStor Service Boundary & Primary Namespace (private-only; primary bucket/env; Versioning OFF) |
| [ADR-AIEOS-044](ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) | AIEOS Bootstrap Production Pre-Apply Execution Baseline (commercial RED evidence; NATS/VPC/state/identity freezes; architecture freeze ≠ apply) |
| [ADR-AIEOS-044R1](ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md) | AIEOS Production State Namespace Collision Resolution (historical BLR1 replacement target `eduvijna-aieos-tofu-state-prod-blr1`; NYC3 collision remains HOLD; forward state authority later superseded by ADR-AIEOS-044R2) |
| [ADR-AIEOS-044R2](ADR-AIEOS-044R2-aieos-production-state-region-availability-resolution.md) | AIEOS Production State Region Availability Resolution (state authority → SFO3 / `eduvijna-aieos-tofu-state-prod-sfo3`; workload remains BLR1; Stage 1 bucket bootstrap + Stage 2 backend initialization completed; Stage 3A refresh-only/live-lock validation completed; Stage 3B first authoritative state materialization completed with zero managed resources; normal workload apply remains unauthorized) |
| [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) | AIEOS Dispatcher Tenant-Candidate Discovery Authority (Frozen / Approved — pending-work candidate discovery; dedicated NOBYPASSRLS candidate-reader boundary; no dispatcher BYPASSRLS; no cross-tenant payload visibility; does not authorize implementation or migration) |
| [ADR-AIEOS-046](ADR-AIEOS-046-aieos-production-event-plane-identity-least-privilege-contract.md) | AIEOS Production Event Plane Identity & Least-Privilege Contract (Frozen / Approved — historical/base production EVENT plane contract; EVENT PUB `io.eduvijna.aieos.content.>` superseded for current publisher-domain scope by ADR-AIEOS-046R1) |
| [ADR-AIEOS-046R1](ADR-AIEOS-046R1-aieos-production-event-plane-multi-domain-publisher-scope-revision.md) | AIEOS Production Event Plane Multi-Domain Publisher Scope Revision (Frozen / Approved — Founder approved 2026-08-31; CURRENT EVENT PUB closed set `io.eduvijna.aieos.content.>` + `io.eduvijna.aieos.teaching.>`; TeachingAssignment outbox events implemented in TOS-DEV06-I03; production EVENT activation / NATS provisioning NOT authorized) |
| [ADR-AIEOS-052](ADR-AIEOS-052-aieos-preparation-kit-multi-artifact-generation-architecture.md) | AIEOS Preparation Kit & Multi-Artifact Generation Architecture (Frozen / Approved — TOS-DEV04 native implementation COMPLETE; Backend `06e05277e73e0c71172cae4904efb37d771c3fad`; live provider proof DEV04-I10 separate gate) |
| [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) | AIEOS Teaching Assignment & Classroom Delivery Authority (Frozen / Approved — Founder approved 2026-08-31; TOS-DEV06 native implementation + Product E2E COMPLETE; external LMS / Student OS delivery deferred) |
| [ADR-AIEOS-054](ADR-AIEOS-054-aieos-teaching-execution-observation-authority.md) | AIEOS Teaching Execution & Observation Authority (Frozen / Approved — Founder approved 2026-09-01; TOS-DEV07; HYBRID Option D; TeachingExecution SoR; Assigned ≠ Taught; **TOS-DEV07 implementation complete / DEV07-I01–I04 formally closed**) |
| [ADR-AIEOS-055](ADR-AIEOS-055-aieos-assessment-learning-evidence-authority.md) | AIEOS Assessment & Learning Evidence Authority (Frozen / Approved — Founder approved **2026-09-03**; TOS-DEV08; OPTION D class-level-first ClassroomAssessment; Taught ≠ Assessed ≠ Mastered; **TOS-DEV08 implementation complete / DEV08-I01–I04 formally closed**) |
| [ADR-AIEOS-056](ADR-AIEOS-056-aieos-improve-remediation-authority.md) | AIEOS Improve & Remediation Authority (Proposed / Freeze Candidate — TOS-DEV09; OPTION B TeachingWork + `remediate_class` + immutable remediation origin; class-level only; **DEV09 implementation not authorized**) |

## Exclusions

- Informal chat decisions without recorded process
- Standards text itself (see `standards/`)
- Detailed design specifications unless captured as a decision record
- Discovery findings that have not been converted into decisions
- Engineering implementation choices that do **not** change architecture (see product repo `engineering/edrs/` — EDRs)
