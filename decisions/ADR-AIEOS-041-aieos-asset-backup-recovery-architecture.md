---
id: ADR-AIEOS-041
title: AIEOS Asset Backup & Recovery Architecture
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-20
last_updated: 2026-08-20
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-041 — AIEOS Asset Backup & Recovery Architecture

**Status:** Frozen / Approved  
**Date:** 2026-08-20  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) · [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) · [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) · [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md)

**Catalogue note:** Frozen / Approved is architecture status. This ADR freezes Asset **backup / recovery** architecture only. It does **not** reopen [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) provider selection. DigitalOcean Spaces remains **REJECTED** as the authoritative primary Asset BlobStore. Spaces is approved here solely as a **non-authoritative backup/recovery** copy. This ADR does **not** authorize Spaces bucket creation, production keys, backup-worker implementation, restore execution, OpenTofu, DigitalOcean mutation, Managed PostgreSQL changes, PITR restore operation, production deployment, or purchase.

---

## Context

[ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) selected MinIO AIStor as the authoritative Asset BlobStore software provider. [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) freezes Bootstrap and Scale Production topology classes. Erasure coding is **not** backup.

A separate backup/recovery architecture is required so Asset bytes can be recovered without treating backup storage as primary authority, and without reopening rejected primary-provider choices.

---

## A. Backup authority

Frozen backup storage:

| Parameter | Value |
|-----------|-------|
| Provider | DigitalOcean Spaces Standard |
| Region | SFO3 |
| Role | BACKUP / RECOVERY ONLY |
| Authority | NON-AUTHORITATIVE |
| Bucket posture | dedicated, private, Versioning enabled |

DigitalOcean Spaces remains **REJECTED** as the authoritative primary Asset BlobStore.

This backup selection does **not** reopen ADR-AIEOS-039.

Primary authority remains:

| Layer | Authority |
|-------|-----------|
| MinIO AIStor | physical authoritative Asset byte storage |
| PostgreSQL Asset SoR | Asset metadata / lifecycle / business authority |
| Spaces | backup / recovery copy only |

---

## B. Backup pipeline

Frozen conceptual flow:

```text
Authoritative Asset commit
    ↓
durable asynchronous backup intent
    ↓
backup worker
    ↓
SFO3 Spaces
    ↓
full-object GET
    ↓
AIEOS SHA-256 verification
+
exact byte-size verification
    ↓
record exact verified Spaces VersionId
    ↓
signed backup manifest
    ↓
VERIFIED
```

A provider upload success alone is **NOT** sufficient to mark a backup **VERIFIED**.

Provider ETag alone is **NOT** AIEOS recovery-integrity proof.

---

## C. Backup integrity

Every backup must be verified using:

- full-object GET
- AIEOS SHA-256
- exact byte size

The verified provider VersionId must be recorded.

**Monthly:** full backup scrub.

The monthly scrub must validate recoverability/integrity according to this frozen backup model.

Do **not** invent an alternative checksum.

---

## D. RPO / escalation

Frozen timing:

| Gate | Value |
|------|-------|
| Primary Asset commit → backup VERIFIED | ≤ 1 hour |
| Warning | 30 minutes |
| Incident / page | 60 minutes |

**60 minutes** is the RPO violation boundary.

Unresolved for **6 hours:** **SEVERE BACKUP ESCALATION**.

Do **not** reinterpret 6 hours as the first alert.

---

## E. Recovery chain

Frozen recovery chain:

- signed backup manifest
- verified Spaces VersionId
- Managed PostgreSQL PITR

→ metadata/byte disaster-recovery chain

Recovery must preserve the broader AIEOS rule:

**restore ≠ automatic authority restoration**

After restore, governance reconciliation is required before authoritative service restoration where applicable.

Restoring backup bytes does **not** automatically make an Asset safe, current, or authorized.

---

## F. Same-provider / account residual risk

Recorded explicitly as Founder-accepted Phase-0 risk:

Primary BlobStore, Managed PostgreSQL, and backup storage remain within the same DigitalOcean provider/account failure domain.

Therefore sufficiently privileged DigitalOcean account compromise can destroy both primary and backup copies.

This is **ACCEPTED** for Bootstrap Production.

It is **NOT** equivalent to independent-provider disaster isolation.

---

## G. Backup credential residual risk

The backup Limited credential necessarily retains provider-level ability sufficient to delete object versions when appropriately privileged.

Mitigations:

- dedicated bucket scope
- private bucket
- Versioning
- immutable application-generated backup keys
- exact verified VersionId recorded
- application policy never normally deletes backup objects
- backup deletion is not ordinary application behavior

This ADR does **not** claim DigitalOcean MFA Delete exists unless separately evidenced and frozen.

---

## H. Explicit non-authorization

ADR-AIEOS-041 Frozen / Approved does **not** authorize:

- Spaces bucket creation/change
- production access keys
- credential rotation implementation
- backup worker implementation
- outbox schema changes
- new migrations
- actual backup execution
- restore execution
- production data copying
- OpenTofu
- DigitalOcean mutation
- Managed PostgreSQL changes
- PITR restore operation
- production deployment
- production mutation
- resource purchase

Architecture freeze ≠ implementation authorization ≠ production authorization ≠ purchase authorization.

---

## Binding invariants

- Backup provider is DigitalOcean Spaces Standard in SFO3, backup/recovery only, non-authoritative.
- Spaces remains rejected as authoritative primary Asset BlobStore.
- MinIO AIStor remains physical authoritative Asset byte storage; PostgreSQL remains Asset SoR authority.
- VERIFIED requires full-object GET + AIEOS SHA-256 + exact byte size + recorded VersionId + signed manifest.
- Provider upload success or ETag alone is insufficient.
- RPO: commit → VERIFIED ≤ 1 hour; warn 30m; page 60m; severe escalation at 6 hours unresolved.
- Recovery chain: signed manifest + verified VersionId + Managed PostgreSQL PITR; restore ≠ automatic authority restoration.
- Same DigitalOcean account failure domain is Founder-accepted Phase-0 residual risk for Bootstrap Production.
- This ADR authorizes no bucket/keys/worker/restore/OpenTofu/cloud mutation/deployment/purchase.

## Consequences

- Catalogue current-summary surfaces must show SFO3 Spaces Standard backup-only with verified ≤1h RPO.
- Implementation must not treat Spaces as a primary BlobStore substitute.
- Backup/DR is no longer an open architecture selection for the frozen model described here; implementation and production execution remain unauthorized.
- Residual account and credential risks remain visible and cannot be softened by catalogue wording.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | PostgreSQL Asset metadata SoR |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Deploy/live/ready ≠ mutation; this ADR does not authorize production |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral BlobStore invariants; backup does not replace primary |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | Current-use / governance fail-closed preserved |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | DigitalOcean production cloud; Managed PostgreSQL baseline |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Historical Spaces rejection as authoritative primary |
| [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | DigitalOcean-only hosting; Spaces still rejected as primary |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | AIStor primary provider; not reopened by backup selection |
| [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) | Historical Scale topology; EC is not backup |
| [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) | Current Bootstrap + Scale topology classification |
