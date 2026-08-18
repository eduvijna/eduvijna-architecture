---
id: ADR-AIEOS-026
title: AIEOS Workflow Implementation Baseline
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-026 — AIEOS Workflow Implementation Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md)

**Catalogue note:** Frozen / Approved is architecture status. It is not implemented, merged, production-authorized, or deployed.

---

## Context

Long-running AIEOS processes need a durable workflow runtime that cannot become a second System of Record, a hidden authorization store, or a substitute for domain mutation. The workflow engine is selected here; it was **not** selected by [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md).

## Decision

- Temporal is the durable workflow runtime.
- Official Temporal Python SDK only.
- Python 3.14.x compatibility is validated; SDK patch pins used during validation are **not** permanent architecture pins.
- Temporal workflow state ≠ AIEOS domain state.
- NATS event state ≠ workflow state.
- UUIDv7 AIEOS workflow identity.
- Stable Temporal Workflow ID.
- Temporal Run ID is separate.
- Durable workflow-start intent.
- Idempotent workflow starts.
- Deterministic workflow definitions.
- No direct database, HTTP, NATS, or AI-provider effects in workflow code.
- Material effects occur through normal durable Activities.
- Stable `activity_operation_id`.
- Explicit Activity timeouts.
- Bounded retry.
- Domain / external idempotency.
- Retry attempt number is not business identity.
- Current authorization is revalidated before sensitive effects.
- Delegation expiry is respected.
- Tenant suspension is respected.
- Break-glass authority does not persist automatically.
- Workflow history / provenance is not perpetual permission.
- AIEOS-authoritative approval artifacts remain in AIEOS, not in Temporal history as authority.
- Durable approval / workflow-command delivery.
- Idempotent workflow commands.
- Authority is revalidated after approval.
- Cancellation ≠ rollback.
- Termination ≠ cancellation.
- Saga-style compensation; compensation failure is visible.
- Explicit workflow major-version evolution.
- History replay is a release gate.
- Continue-As-New for bounded history.
- Payload minimization.
- No secrets in workflow history.
- PII-minimized search metadata.
- Environment isolation.
- Capability-oriented task queues.
- No per-tenant namespace / task-queue baseline.
- No Temporal SDK in domains.
- No Temporal SDK in frontend.
- Target OCI / runtime compatibility is revalidated before production.

## Binding invariants

- Workflow execution truth is not Generic Content, Asset, or authorization truth.
- Temporal may coordinate and wait; it is not approval authority ([ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)).
- Activities invoke normal application commands; they do not bypass current authorization.

## Explicit non-goals / deferred decisions

- Production Temporal hosting, namespace operations, and deployment remain independently unauthorized.
- Compatibility smoke success on a developer/CI runtime is not the production-runtime gate.
- Asset workflows / Asset events are not frozen by this ADR.

## Consequences

- Domain packages remain free of Temporal types.
- Replay and Continue-As-New are release and history-hygiene requirements, not optional optimizations.
- A frozen SDK validation version (for example a specific `temporalio` release used in a gate) is evidence, not an ADR pin.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) | Python family; workflow engine was deferred there |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | Event ≠ workflow |
| [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | Content approval remains AIEOS authority |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Current authorization revalidation |
