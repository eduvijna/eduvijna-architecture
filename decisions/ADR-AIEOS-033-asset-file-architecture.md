---
id: ADR-AIEOS-033
title: "Asset/File Architecture"
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-033 — Asset/File Architecture

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md)

**Catalogue note:** Chief Architect confirms this ADR is **Frozen / Approved**. Later current-use, mutation, and audit semantics belong to ADR-AIEOS-034 / 035 / 036 / 036R1. This ADR does **not** select a production BlobStore provider.

---

## Context

AIEOS binary assets need identities, a typed ResourceRef family, and a provider-neutral storage boundary so Content can reference files without owning blobs, and so cloud-provider choice is not smuggled into the domain.

## Decision

- Stable Asset identity.
- Immutable AssetRevision identity / history.
- Canonical typed Asset `ResourceRef` family:
  - `asset.image`
  - `asset.document`
  - `asset.audio`
  - `asset.video`
- Asset revision number is distinct from aggregate revision.
- Lifecycle: `active` · `withdrawn` · `deleted`
- Quarantine: `clear` · `quarantined`
- Revision safety: `pending` · `passed` · `failed`
- PostgreSQL domain-owned Asset SoR.
- Provider-neutral BlobStore boundary.
- Opaque `storage_key` (never parsed as business identity).
- Safe pre-persistence ingest.
- Inspect / reconciliation semantics.
- Physical store and PostgreSQL store are explicitly non-atomic.
- Orphan physical objects are not auto-destructively cleaned up.
- No automatic cross-domain foreign keys.
- Content references Assets through `ResourceRef`.
- **No production BlobStore provider selection in ADR-AIEOS-033.**

## Binding invariants

- Content must not persist Asset bytes or storage internals as Content truth.
- Compensation must not treat `BlobStore.delete` as automatic orphan cleanup.
- Production cloud provider (including S3 / Azure / GCS / MinIO) is **not selected** and **not authorized** by this ADR.

## Explicit non-goals / deferred decisions

Do not treat later slices as originally owned by this ADR:

- Current-use rejection vocabulary — [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md)
- Mutation / activation semantics — [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md)
- Authorization / transactional audit — [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md)

Still open / not frozen here:

- Asset HTTP / binary delivery contract
- Asset events / outbox
- Physical purge / retention / legal hold
- Asset schema-owner readiness
- Asset production runtime composition
- Production deployment / mutation / migration

## Consequences

- Implementation may add Asset tables and BlobStore ports without production composition.
- A later provider choice requires a new architecture decision.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | Domain-owned schemas; no automatic cross-domain FK |
| [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | Content references Assets via ResourceRef |
| [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) | AssetUseAuthority port |
