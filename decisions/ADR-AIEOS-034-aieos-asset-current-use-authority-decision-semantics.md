---
id: ADR-AIEOS-034
title: AIEOS Asset Current-Use Authority Decision Semantics
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-034 — AIEOS Asset Current-Use Authority Decision Semantics

**Status:** Frozen / Approved
**Date:** 2026-08-18
**Related:** [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) · [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md)

**Catalogue note:** Frozen / Approved is architecture status. Current-use evaluation is not production composition, not Asset HTTP, and not a BlobStore provider selection. Later [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) preserves authoritative physical whole-object SHA-256 inspection; it does not change these semantics.

---

## Context

`AssetUseAuthority` must return deterministic current-use decisions so Content cannot overload missing bytes, purge facts, or storage unavailability onto unrelated rejection codes. A PostgreSQL adapter’s emission set must not shrink the frozen contract vocabulary.

## Decision

### Exact rejection vocabulary

- `NOT_FOUND`
- `TENANT_INACCESSIBLE`
- `REVISION_NOT_FOUND`
- `WITHDRAWN`
- `DELETED`
- `QUARANTINED`
- `SAFETY_PENDING`
- `SAFETY_FAILED`
- `BYTES_PURGED`
- `BYTES_MISSING`
- `INTEGRITY_MISMATCH`

`TENANT_INACCESSIBLE` remains on the contract even if a given PostgreSQL provider does not emit it.

Do not introduce overloaded substitutes such as `BLOB_MISSING`, `STORAGE_UNAVAILABLE`, `UNKNOWN`, `ERROR`, `CORRUPT`, `UNAVAILABLE`, or `BLOB_ERROR` as current-use reasons.

Forbidden overloads:

- missing blob → `NOT_FOUND` / `DELETED` / `SAFETY_FAILED`
- bytes purged → `DELETED` / `SAFETY_FAILED`
- integrity mismatch → `SAFETY_FAILED`

**Forbidden:** BlobStore unavailable → deterministic unusable assessment (`usable=false` with a rejection reason).
**Required:** BlobStore unavailable → governance unavailable (`GovernanceUnavailableError` / equivalent fail-closed unavailable semantics). It must **not** become a deterministic unusable rejection reason.

### Precedence / current-use semantics

1. Tenant-hidden typed Asset → `NOT_FOUND` (no existence oracle). Type mismatch → `NOT_FOUND`.
2. `deleted` before `withdrawn`.
3. `withdrawn` before quarantine / safety / physical checks.
4. `quarantined` blocks current use.
5. Missing pinned or current revision → `REVISION_NOT_FOUND`.
6. Exact revision identity / type / ownership validation; missing exact revision → `REVISION_NOT_FOUND`.
7. Missing or malformed authoritative revision state → governance unavailable (do not manufacture `SAFETY_PENDING`).
8. Safety failed → `SAFETY_FAILED`.
9. Safety pending → `SAFETY_PENDING`.
10. `bytes_purged` authority before physical inspect → `BYTES_PURGED`.
11. BlobStore unavailable → governance unavailable.
12. Inspect `None` → `BYTES_MISSING`.
13. Size / hash mismatch → `INTEGRITY_MISMATCH`.
14. Only then a positive current-use result.

Additional frozen rules:

- `ACTIVE` alone never implies usable.
- Positive current-use decision requires stable authority across physical inspection.
- Unpinned `current_revision` change during inspect requires re-evaluation.
- Optimistic retry is bounded; persistent governing-state churn → governance unavailable.
- No positive cross-request cache.

### Assessment metadata

- When an Asset is successfully resolved / evaluated, `authority_revision` represents the governing Asset `aggregate_revision`.
- `NOT_FOUND` has `authority_revision = None`.
- `observed_at` is timezone-aware.
- `authority_revision` is **not** `AssetRevisionNumber`.

## Binding invariants

- Physical existence must not override a purge fact.
- Governance unavailable is not DENY and not a usable=`true` result.
- Authorization remains a separate kernel concern ([ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md)).

## Explicit non-goals / deferred decisions

- Mutation commands — [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md)
- Production BlobStore provider — **not selected**
- Asset HTTP, events, purge orchestration — **not frozen**
- Production runtime composition — **open**

## Consequences

- Content adapters consume usable / unusable plus this vocabulary; they do not invent Asset SQL checks.
- Later vocabulary expansion requires a governed ADR, not adapter convenience.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) | AssetUseAuthority port |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Identities, lifecycle, BlobStore |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | How revisions become current |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Later exception preserves whole-object SHA-256 inspect; S3 not selected here |
