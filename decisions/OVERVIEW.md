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

## Exclusions

- Informal chat decisions without recorded process
- Standards text itself (see `standards/`)
- Detailed design specifications unless captured as a decision record
- Discovery findings that have not been converted into decisions
- Engineering implementation choices that do **not** change architecture (see product repo `engineering/edrs/` — EDRs)
