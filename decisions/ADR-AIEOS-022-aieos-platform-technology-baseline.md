---
id: ADR-AIEOS-022
title: AIEOS Platform Technology Baseline
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-022 — AIEOS Platform Technology Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md)

**Catalogue note:** Frozen / Approved is architecture status. It is not implemented, merged, production-authorized, or deployed.

---

## Context

AIEOS requires a single platform technology baseline so later domain, API, workflow, and production-readiness decisions do not silently adopt incompatible runtimes, persistence styles, or delivery identities. Later workstreams must not treat convenience libraries or legacy EduVijna surfaces as the AIEOS target boundary.

## Decision

AIEOS adopts the following **family** baseline. Exact dependency versions are locked in owning repositories, not as ADR patch pins.

### Backend / runtime

- Python 3.14
- FastAPI
- Pydantic 2
- uv
- SQLAlchemy 2.0
- Alembic

### Database

- PostgreSQL 18
- PostgreSQL sits behind an explicit AIEOS domain/application API boundary
- **No PostgREST** as the AIEOS target application boundary

### Frontend

- TypeScript 6
- React 19
- Vite 8
- Node 24 LTS
- pnpm 11

### Testing

- pytest
- Vitest-family
- Playwright

### Delivery / provenance

- GitHub Actions
- OCI artifacts
- GitHub Container Registry
- immutable artifact digests
- artifact attestations

### Observability

- OpenTelemetry traces and metrics
- OTLP
- W3C Trace Context
- governed structured JSON logs

### Infrastructure language

- OpenTofu

## Binding invariants

- No legacy API, UI, runtime, or schema dependency as the AIEOS target.
- Schema change is explicit Alembic migration only; no runtime schema creation.
- PostgreSQL is not a public application façade; it sits behind application/domain contracts.
- Production artifacts must be attributable to immutable source identity.
- Distributed infrastructure is introduced only by owning workstream evidence and its own ADRs.
- Exact patch/minor dependency versions are locked in repository lockfiles, not in this ADR.

## Explicit non-goals / deferred decisions

Deferred at this architecture point (not selected by ADR-AIEOS-022):

- cloud provider
- Kubernetes / runtime orchestrator
- production secret-manager vendor
- Redis
- event broker
- workflow engine
- policy engine
- vector / knowledge platform

Later selections such as NATS JetStream ([ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md)) and Temporal ([ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md)) are **not** back-edited into this baseline.

## Consequences

- GCI/PED implementation may use this family; it does not become production authorization.
- PostgREST remaining on other EduVijna surfaces does not make it the AIEOS application boundary.
- New distributed components require their own frozen decisions before they are treated as platform baseline.

## Related ADRs

| ID | Relationship |
|----|----------------|
| ADR-AIEOS-023 | Frozen Identity/Tenant/Security decision; canonical body **not** deposited in EA-SYNC-01A |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | Data / Resource / SoR baseline |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | API / events / NATS |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Temporal workflow runtime |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Production readiness; deployment still not authorized |
