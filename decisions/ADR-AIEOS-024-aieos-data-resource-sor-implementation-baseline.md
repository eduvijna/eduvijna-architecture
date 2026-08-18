---
id: ADR-AIEOS-024
title: "AIEOS Data, Resource & SoR Implementation Baseline"
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-024 — AIEOS Data, Resource & SoR Implementation Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md)

**Catalogue note:** Frozen / Approved is architecture status. It is not implemented, merged, production-authorized, or deployed.

---

## Context

AIEOS domains must share one persistence and resource-identity model without collapsing into a shared mega-table, legacy schema reuse, or silent cross-domain coupling. Mutation, event-publication intent, and required security-audit intent must be coordinable in the same authoritative transaction.

## Decision

### Ownership and SoR

- Domain-owned PostgreSQL schemas.
- One declared System of Record per material state.
- One PostgreSQL database initially.
- Strong schema/domain ownership.
- No cross-domain persistence bypass.
- No automatic cross-domain foreign keys.
- No universal resource mega-table.
- No legacy schema reuse.
- No legacy `edu.content` dependency as AIEOS Generic Content SoR.

### Identity and concurrency

- UUIDv7 is the default resource identity.
- Explicit `BIGINT` aggregate revision.
- Atomic expected-revision concurrency.
- `ResourceRef` is the cross-boundary resource contract.
- Business version is not aggregate revision.

### Tenancy and isolation

- Explicit `tenant_id` on tenant-owned state.
- Tenant-consistent same-domain relationships.
- PostgreSQL RLS as defense in depth.
- Transaction-local database security context.

### Persistence identities

Runtime database identity ≠ migration identity ≠ schema owner ≠ backup/restore identity.

### Persistence adapters

- SQLAlchemy persistence adapters.
- Application-owned Unit of Work.
- Repositories do not independently commit.
- Alembic-controlled migrations.
- No runtime schema creation.
- Large backfills are resumable and governed.

### Transactional mutation coordination

Authoritative business mutation, required event-publication intent, and required security-audit intent are transactionally coordinated where those intents are required.

### Generic Content SoR (data baseline)

- New Generic Content SoR.
- Stable Content identity.
- Immutable Content versions.

Generic Content domain rules are detailed in [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md).

### Derived and deletion state

- Search, vector, analytics, knowledge-graph, and cache state are derived and non-authoritative by default.
- Derived state carries provenance and freshness.
- Deletion completion requires lifecycle evidence.
- Restore requires governance reconciliation.
- No universal soft-delete pattern.

## Binding invariants (hardening)

- Atomic revision updates.
- Tenant-consistent relationships.
- Database-protected immutable versions.
- Transactional publication/audit intent.
- Cross-domain foreign keys require explicit review.
- Durable deletion evidence.
- Restore quarantine / reconciliation.
- Separate backup/restore authority.
- Resumable large backfills.

## Explicit non-goals / deferred decisions

- Production migration, mutation, and deployment remain independently unauthorized.
- Asset/File SoR specifics belong to [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) and later Asset ADRs.
- Identity/tenant/security **current-authority tables** are not reconstructed here; historical ADR-AIEOS-023 remains frozen but unavailable, and [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) is the canonical restatement.

## Consequences

- Later GCI/PED slices must preserve identity separation and UoW/commit ownership.
- Derived stores cannot be queried as business truth.
- A later cross-domain FK is an architecture exception, not a default.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) | Technology family (PostgreSQL, SQLAlchemy, Alembic) |
| ADR-AIEOS-023 | Historical Frozen / Approved Identity/Tenant/Security; original body unavailable |
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Canonical Identity, Tenant & Security restatement |
| [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | Generic Content SoR |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Security-audit intent |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Asset SoR |
