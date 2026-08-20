---
id: ADR-AIEOS-041R1
title: AIEOS Asset Backup Execution, Manifest & Recovery Authority
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-20
last_updated: 2026-08-20
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-041R1 — AIEOS Asset Backup Execution, Manifest & Recovery Authority

**Status:** Frozen / Approved  
**Date:** 2026-08-20  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) · [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) · [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) · [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md)

**Catalogue note:** Frozen / Approved is architecture status. Founder approval of the Bootstrap architecture closure is recorded on **2026-08-20**. This ADR is a **forward revision / extension** of [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md). It does **not** delete, rewrite, or mark ADR-AIEOS-041 rejected. ADR-AIEOS-041 remains Frozen / Approved. This ADR supplies missing execution/authority semantics. It does **not** authorize backup-worker implementation, schema migration, Spaces bucket creation, credentials, OpenTofu, PITR restore execution, production deployment, or purchase.

---

## Context

[ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) froze backup provider (SFO3 Spaces Standard), non-authoritative role, verification model, RPO/escalation, and recovery chain. Execution authority for durable backup intent, signed-manifest custody, first-production Asset events, and Bootstrap PITR retention acceptance remained open.

This ADR closes those execution/authority gaps without reopening Spaces as primary BlobStore.

---

## A. Backup job authority

**PostgreSQL is the durable backup-job authority.**

When an authoritative Asset transaction establishes the Asset/storage reference requiring backup, the durable backup intent must be committed **transactionally** with that authoritative state.

Conceptual pipeline:

```text
Asset authoritative transaction
→ durable PostgreSQL backup job
→ asynchronous worker
→ SFO3 Spaces
→ full GET verification
→ SHA-256 + exact byte-size verification
→ exact VersionId
→ signed manifest
→ VERIFIED
```

**NATS** may be wake-up/transport optimization only. NATS is **never** backup-job truth.

**Temporal** may coordinate appropriate scheduling/workflow mechanics. Temporal history is **never** backup job/business SoR.

---

## B. Job lifecycle

Freeze conceptual states:

- `PENDING`
- `IN_PROGRESS`
- `VERIFIED`

plus explicit retryable failure and governed terminal/quarantine semantics.

Implementation must provide:

- exclusive worker claim
- crash/restart recovery
- retry
- idempotency
- no double-VERIFIED corruption

Exact SQL locking syntax, lease duration, and retry constants are EDR/implementation details.

---

## C. VERIFIED authority

`VERIFIED` is durable **PostgreSQL** truth and may be set only after all [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) integrity requirements succeed.

Upload success alone is insufficient. Provider ETag alone is insufficient.

---

## D. Signed manifest

Canonical backup-manifest authority is **PostgreSQL**.

Use:

- **RFC 8785** JSON Canonicalization Scheme (JCS)
- Signature: **Ed25519**
- Dedicated manifest-signing identity

The signer must be distinct from:

- AIStor runtime credential
- Spaces credential
- PostgreSQL password/identity
- DigitalOcean infrastructure credential

Manifest includes at minimum:

- manifest format/version
- `asset_id`
- Asset revision identity / aggregate revision as applicable
- opaque `storage_key`
- SHA-256
- exact `byte_size`
- Spaces backup bucket identity
- Spaces object identity
- exact verified Spaces VersionId
- verification timestamp
- source Asset authoritative revision/commit identity as represented by the implementation contract
- signing algorithm
- signing key identifier/version
- signature

Canonical bytes + signature + signing-key identity must remain recoverably associated with the PostgreSQL authority record.

Spaces may hold a manifest copy but Spaces alone is **NOT** manifest authority.

---

## E. Key rotation

Signing keys must be independently rotatable.

New signatures identify the active signing-key version.

Verification material for historical signed manifests must remain available after rotation for the required recovery lifetime.

Exact secret product/custody implementation remains governed implementation design, preserving [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) secret-injection requirements.

---

## F. Asset events

For Bootstrap first production:

**NO Asset domain business event is required for correctness.**

Therefore:

- backup job state is not an Asset business event
- security audit remains security audit
- backup wake-up is not business truth
- no Asset outbox event is required merely because NATS exists

A later real integration/consumer requirement may introduce Asset business events through architecture review.

This closes the currently open first-production Asset-events question.

---

## G. PITR

Accept DigitalOcean Managed PostgreSQL current **seven-day** PITR recovery window as Bootstrap Phase-0 baseline.

Restore continues to create recovery state that requires governance reconciliation.

**Restore ≠ automatic authority restoration.**

Longer-than-provider PITR retention is not required for Bootstrap day-0 and would require later architecture if introduced.

---

## H. ADR-041 preserved

Preserve all [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) requirements:

- SFO3 Spaces Standard
- backup/recovery only
- non-authoritative
- private dedicated bucket
- Versioning enabled
- full GET verification
- AIEOS SHA-256
- exact byte size
- verified VersionId
- ≤1h VERIFIED RPO
- warning 30m
- page 60m
- severe unresolved at 6h
- monthly full scrub
- signed-manifest + VersionId + PG PITR recovery chain
- same-DigitalOcean provider/account Phase-0 residual risk acceptance
- restore ≠ automatic authority restoration

---

## I. Spaces IAM documentation uncertainty

Do **not** add a new architecture assertion that provider-level backup delete capability is necessarily unavoidable beyond what [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) already records.

Current provider documentation exposes inconsistent permission descriptions.

Record instead:

Before production backup credential creation, perform an explicit provider-capability validation of the selected bucket-scoped credential profile for:

- PUT
- GET
- version access
- DELETE

If a narrower no-delete production capability is empirically available and preserves backup requirements, use least privilege.

If provider behavior still grants delete capability, ADR-AIEOS-041 residual-risk acceptance and mitigations remain binding.

This validation does **not** reopen the backup provider decision.

---

## Explicit non-authorization

ADR-AIEOS-041R1 Frozen / Approved does **not** authorize:

- backup worker implementation
- backup schema/migrations
- Spaces bucket creation/change
- production access keys
- OpenTofu apply
- DigitalOcean mutation
- Managed PostgreSQL changes
- PITR restore operation
- production deployment
- purchase
- Asset business-event implementation merely to occupy NATS

Architecture freeze ≠ implementation authorization ≠ production authorization.

---

## Binding invariants

- PostgreSQL is durable backup-job and VERIFIED authority; NATS/Temporal are not job SoR.
- Backup intent is transactionally committed with authoritative Asset state that requires backup.
- Job states include PENDING, IN_PROGRESS, VERIFIED plus retryable failure and governed quarantine/terminal semantics.
- Signed manifest: RFC 8785 JCS + Ed25519; PG canonical authority; dedicated signer distinct from storage/infra credentials.
- Bootstrap first production requires no Asset domain business events for correctness.
- Managed PostgreSQL seven-day PITR is Bootstrap Phase-0 baseline; restore ≠ automatic authority restoration.
- All ADR-AIEOS-041 backup provider/integrity/RPO requirements remain binding.

## Consequences

- Catalogue current-summary surfaces treat backup durable-intent authority, signed-manifest authority, Bootstrap Asset events, and Bootstrap PITR retention as architecture-closed.
- Implementation proceeds against PG job SoR and PG manifest authority without inventing NATS/Temporal as backup truth.
- Credential creation still requires later authorization and explicit Spaces capability validation.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) | Base backup/recovery architecture; preserved and extended |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | PostgreSQL domain SoR family |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | NATS is transport; not backup SoR |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Temporal ≠ domain/business SoR |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Audit remains audit |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | Secret injection; Managed PG PITR class |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | Primary provider unchanged |
| [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) | Bootstrap class |
| [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) | Authoritative Asset commit precedes backup intent |
