---
id: ADR-AIEOS-035
title: "AIEOS Asset Mutation & Revision Activation Semantics"
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-035 — AIEOS Asset Mutation & Revision Activation Semantics

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md)

**Catalogue note:** Frozen / Approved is architecture status. Asset remains NON_PRODUCTION. This ADR does not add authorization, audit, HTTP, events, provider selection, or production composition. [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) later adds auth/audit without rewriting these rules.

---

## Context

Asset aggregates need explicit create, immutable revision registration, and exact-revision activation so ingest cannot silently activate, and so PostgreSQL and BlobStore non-atomicity remains visible.

## Decision

### Create

- `lifecycle = active`
- `quarantine = clear`
- `current_revision = NULL`
- `aggregate_revision = 0`

Create replay with a stable caller-supplied AssetId:

- same AssetId + same immutable creation facts → return the existing Asset outcome
- same AssetId + different immutable creation facts → identity conflict

A valid create replay remains a replay even if mutable Asset state changed after original creation. It must **not** create another Asset.

### Registration

- Immutable revision.
- Pending safety / non-purged revision state.
- Monotonic revision number.
- Registration **does not** advance `aggregate_revision`.
- Registration allowed while `active` / `withdrawn` where otherwise valid.
- No implicit activation.
- Deleted Assets must not receive a new revision.

Revision registration replay:

- same `AssetRevisionId` + same immutable revision facts → return the existing revision
- same `AssetRevisionId` + different immutable revision facts → identity conflict

Replay does not allocate another revision number, increment `aggregate_revision`, or activate the revision.

### Activation

Frozen ordering:

1. read candidate authoritative facts
2. verify candidate `aggregate_revision` equals caller expected revision
3. `BlobStore.inspect`
4. verify observed size/hash
5. begin write UoW / transaction
6. lock Asset
7. re-read governing facts
8. verify expected revision and candidate still match
9. set exact `current_revision`
10. `aggregate_revision` +1 exactly once
11. commit

Stale expected aggregate revision must be rejected **before** `BlobStore.inspect`. There is no hidden retry that changes caller concurrency authority.

- Exact candidate revision.
- Safety must be `passed`.
- Bytes not purged.
- Missing, unavailable, or mismatching bytes prevent mutation.
- A withdrawn Asset may activate an otherwise valid revision.
- A quarantined Asset may activate an otherwise valid revision.
- [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) still blocks current use while withdrawn or quarantined.

### Lifecycle

- `active` ↔ `withdrawn`
- `active` → `deleted`
- `withdrawn` → `deleted`
- `deleted` is terminal
- Preserve `current_revision` (logical delete does not clear it automatically)
- Logical delete does not call `BlobStore.delete`
- Valid lifecycle transition increments aggregate by 1

### Quarantine

- `clear` ↔ `quarantined` before deletion
- Aggregate +1

### Safety

- `pending` → `passed`
- `pending` → `failed`
- `passed` → `failed`
- `failed` is terminal for that immutable revision
- Every listed safety state change increments parent aggregate
- Historical revision safety change still advances aggregate
- Pending may finalize after Asset deletion (does not reactivate or make usable)

### Concurrency

- Caller supplies expected aggregate revision.
- No hidden retry that changes caller concurrency authority.
- Stale expected revision is a conflict with zero mutation.

### Unit of Work

- Application owns the transaction.
- Repositories do not independently commit.

## Binding invariants

- Asset revision number ≠ aggregate revision.
- Registration is N→N on aggregate revision.
- Activation and listed governance transitions are N→N+1.
- PostgreSQL and BlobStore are not one ACID transaction; orphans are not auto-deleted.

## Explicit non-goals (this ADR)

- No production BlobStore / cloud provider selection
- No HTTP / binary contract
- No physical purge / retention / hold orchestration
- No security audit
- No events / outbox
- No Temporal
- No production runtime composition
- No PED-I03 Asset mutation activation
- No Asset schema-owner readiness closure

## Consequences

- Current-use evaluation ([ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md)) still applies after activation (withdrawn / quarantined Assets may have a current revision and still be unusable).
- Later auth/audit must not weaken these transition or concurrency rules.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Identities, vocabularies, BlobStore |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | Current-use after mutation |
| [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) | Authorization and audit added later |
