---
id: ADR-AIEOS-028
title: "Security Audit & Mutation Accountability"
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-028 — Security Audit & Mutation Accountability

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md)

**Catalogue note:** Frozen / Approved is architecture status. Implemented audit evidence may remain NON_PRODUCTION. This ADR does not authorize production mutation or deployment.

---

## Context

AIEOS protected mutations need an append-only security-accountability ledger that cannot be confused with domain truth, authorization decisions, integration events, application logs, workflow history, or SIEM projections.

## Decision

- Security audit is append-only audit authority for committed protected mutations.
- Tenant-bound audit records.
- Immutable audit evidence.
- PostgreSQL RLS / tenant isolation apply to audit evidence.
- Where a security audit is required, material successful mutation and required security-audit evidence are transactionally coordinated.
- Audit persistence failure must prevent the protected authoritative mutation from committing when that audit is required.
- Actor, provenance, resource, and revision context are retained.
- Audit is not a substitute for domain event / outbox ([ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md)).
- Audit is not a current authorization decision ([ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md)).

Security audit is distinct from:

| Concern | Not security audit |
|---------|--------------------|
| Domain business truth | Content / Asset aggregates |
| Authorization | Current Authorization Kernel |
| Integration events | Outbox / CloudEvents / NATS |
| Application logs | Operational logging |
| Temporal history | Workflow execution truth |
| SIEM projections | Derived observability |

## Binding invariants

- Required audit failure rolls back the protected mutation.
- Audit rows are not UPDATE/DELETE business APIs.
- Archive / purge of audit or business data remain separate governance concerns (not frozen here).

## Explicit non-goals / deferred decisions

- Current Alembic heads and later action-enum growth are implementation evidence, not this baseline’s body.
- Asset-specific audit vocabulary and ResourceRef revision semantics are refined by [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) and [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md).
- Production deployment of audit-bearing mutations remains independently unauthorized.

## Consequences

- GCI/SAI/PED slices may implement this ledger without becoming production authorization.
- Querying audit to authorize or to recover domain state is non-compliant.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | Transactional mutation + audit intent |
| [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | Content mutation coordination |
| [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) | Asset audit vocabulary |
| [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) | Asset audit ResourceRef refinement |
