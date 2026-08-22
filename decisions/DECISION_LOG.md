# Decision Log

## Purpose

Maintain a chronological summary of approved architecture decisions for the EduVijna ecosystem.

## Relationship to ADRs

| | Decision Log | Architecture Decision Record (ADR) |
|---|--------------|--------------------------------------|
| Role | Chronological index and summary | Full decision record |
| Detail | Brief entry: identifier, title, date, status, link | Context, decision, consequences, and supporting detail |
| Authority | Points to approved decisions | Is the authoritative decision artefact |
| Granularity | One line or short entry per approved decision | One document per decision |

This file is **not** an ADR. It does not replace ADRs and must not contain the full decision rationale.

When an ADR is approved, add a summary entry here that links to the ADR.

---

## Approved decisions

| ID | Title | Date | Status | Record |
|----|-------|------|--------|--------|
| ADR-042 | Teacher OS Shell owns UX (not business capabilities) | 2026-08-10 | Accepted | [ADR-042](ADR-042-teacher-os-shell-owns-ux.md) |
| ADR-043 | Stable foundations before features | 2026-08-10 | Accepted | [ADR-043](ADR-043-stable-foundations-before-features.md) |
| ADR-044 | AI Platform behind stable product services | 2026-08-10 | Accepted | [ADR-044](ADR-044-ai-platform-behind-stable-services.md) |
| ADR-045 | Teaching Intent owns teacher goals (constitutional) · Implemented EBP-001.3 (UX) | 2026-08-10 | Accepted | [ADR-045](ADR-045-teaching-intent-owns-goals.md) |
| ADR-046 | Artifact Status Lifecycle (one lifecycle, no exceptions) | 2026-08-10 | Accepted | [ADR-046](ADR-046-artifact-status-lifecycle.md) |
| ADR-047 | Outcome-first teaching language (Prepare Tomorrow) | 2026-08-10 | Accepted | [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) |
| ADR-048 | Review Queue owns approval (teacher judgement only) · Implemented EBP-001.5 | 2026-08-10 | Accepted | [ADR-048](ADR-048-review-queue-owns-approval.md) |

`ADR-AIEOS-*` and Teacher OS `ADR-042`–`ADR-048` are distinct ID families.

Historical ADR-AIEOS-023 Identity/Tenant/Security remains Frozen / Approved; original body unavailable. ADR-AIEOS-023R1 is the transparent canonical restatement.

| ID | Title | Date | Status | Record |
|----|-------|------|--------|--------|
| ADR-AIEOS-022 | AIEOS Platform Technology Baseline | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) |
| ADR-AIEOS-023 | Historical Identity/Tenant/Security | — | Frozen / Approved | Original body unavailable — no fabricated file |
| ADR-AIEOS-023R1 | AIEOS Identity, Tenant & Security Canonical Restatement | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) |
| ADR-AIEOS-024 | AIEOS Data, Resource & SoR Implementation Baseline | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) |
| ADR-AIEOS-025 | AIEOS API Contract & Integration Implementation Baseline | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) |
| ADR-AIEOS-026 | AIEOS Workflow Implementation Baseline | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) |
| ADR-AIEOS-027 | AIEOS Generic Content Implementation Baseline | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) |
| ADR-AIEOS-028 | Security Audit & Mutation Accountability | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) |
| ADR-AIEOS-029 | Production Environment & Deployment Readiness Baseline | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) |
| ADR-AIEOS-030 | Production JWT Bearer | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md) |
| ADR-AIEOS-031 | Production Authorization Kernel | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) |
| ADR-AIEOS-032 | Governance Adapter Foundation | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) |
| ADR-AIEOS-033 | Asset/File Architecture | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) |
| ADR-AIEOS-034 | AIEOS Asset Current-Use Authority Decision Semantics | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) |
| ADR-AIEOS-035 | AIEOS Asset Mutation & Revision Activation Semantics | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) |
| ADR-AIEOS-036 | Asset Authorization & Transactional Security Audit Baseline | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) |
| ADR-AIEOS-036R1 | Asset Security-Audit Resource Revision Semantics | 2026-08-18 | Frozen / Approved | [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) |
| ADR-AIEOS-037 | AIEOS Production Infrastructure Baseline | 2026-08-19 | Frozen / Approved | [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) |
| ADR-AIEOS-038 | AIEOS Cross-Cloud Asset Object Storage Exception | 2026-08-19 | Frozen / Approved | [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) |
| ADR-AIEOS-038R1 | AIEOS DigitalOcean-Only Asset Storage Direction | 2026-08-19 | Frozen / Approved | [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) |
| ADR-AIEOS-039 | AIEOS Asset BlobStore Provider Selection | 2026-08-19 | Frozen / Approved | [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) |
| ADR-AIEOS-040 | AIEOS Asset BlobStore First-Production Topology | 2026-08-19 | Frozen / Approved | [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) |
| ADR-AIEOS-040R1 | AIEOS Asset BlobStore Bootstrap & Scale Production Topology | 2026-08-20 | Frozen / Approved | [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) |
| ADR-AIEOS-041 | AIEOS Asset Backup & Recovery Architecture | 2026-08-20 | Frozen / Approved | [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) |
| ADR-AIEOS-041R1 | AIEOS Asset Backup Execution, Manifest & Recovery Authority | 2026-08-20 | Frozen / Approved | [ADR-AIEOS-041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) |
| ADR-AIEOS-042 | AIEOS Asset Binary Delivery & Bootstrap Media Profile | 2026-08-20 | Frozen / Approved | [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) |
| ADR-AIEOS-043 | AIEOS Bootstrap AIStor Service Boundary & Primary Namespace | 2026-08-20 | Frozen / Approved | [ADR-AIEOS-043](ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md) |
| ADR-AIEOS-044 | AIEOS Bootstrap Production Pre-Apply Execution Baseline | 2026-08-21 | Frozen / Approved | [ADR-AIEOS-044](ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) |
| ADR-AIEOS-044R1 | AIEOS Production State Namespace Collision Resolution | 2026-08-21 | Frozen / Approved | [ADR-AIEOS-044R1](ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md) |
| ADR-AIEOS-044R2 | AIEOS Production State Region Availability Resolution | 2026-08-21 | Frozen / Approved | [ADR-AIEOS-044R2](ADR-AIEOS-044R2-aieos-production-state-region-availability-resolution.md) |

---

## Proposed decisions (not yet frozen)

| ID | Title | Date | Status | Record |
|----|-------|------|--------|--------|
| ADR-AIEOS-045 | AIEOS Dispatcher Tenant-Candidate Discovery Authority | 2026-08-22 | Proposed / Awaiting Chief Architect Freeze | [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) |
