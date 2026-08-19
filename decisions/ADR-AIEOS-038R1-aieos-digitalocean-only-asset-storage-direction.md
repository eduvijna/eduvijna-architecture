---
id: ADR-AIEOS-038R1
title: AIEOS DigitalOcean-Only Asset Storage Direction
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-19
last_updated: 2026-08-19
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-038R1 — AIEOS DigitalOcean-Only Asset Storage Direction

**Status:** Frozen / Approved  
**Date:** 2026-08-19  
**Related:** [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md)

**Catalogue note:** Frozen / Approved is architecture status. This ADR is a **forward refinement** of [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md). It does **not** delete, reject, or rewrite ADR-AIEOS-038. ADR-AIEOS-038 remains an approved historical decision. This ADR is the current first-production Asset-storage **hosting direction**. It does **not** select a BlobStore provider. Later [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) selects MinIO AIStor as the first-production BlobStore software provider. It does **not** authorize DigitalOcean object-storage resources, MinIO/AIStor installation, AWS resources, OpenTofu apply, Asset HTTP, or implementation.

---

## Context

[ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) authorized a narrow cross-cloud Asset-storage exception and allowed Amazon S3 to advance as a provider **candidate**. That historical authorization remains recorded.

First-production cloud placement for authoritative Asset object storage is now closed to that cross-cloud path. DigitalOcean remains the AIEOS production compute/database cloud under [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md). DigitalOcean Spaces remains rejected under accepted PED-I10B7B-TV01 evidence. Provider fit must not weaken the frozen BlobStore contract.

## Decision

### DigitalOcean-only first-production Asset storage hosting boundary

DigitalOcean is the sole first-production cloud boundary for AIEOS, including the hosting boundary for authoritative Asset object storage.

Preserve [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md):

- DigitalOcean production cloud
- BLR1 initial production locality
- App Platform for primary stateless compute
- DigitalOcean Managed PostgreSQL 18
- AIEOS-operated NATS on DigitalOcean
- Temporal Cloud remains the separately selected workflow service

“DigitalOcean-only Asset storage” refers to the Asset storage **hosting boundary**. This ADR does **not** cancel already-frozen Temporal Cloud.

### Cross-cloud storage path is not active for first production

For first production, the [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) cross-cloud Asset object-storage exception is **not active**.

- Amazon S3 does **not** advance to provider validation or implementation.
- AWS account bootstrap is cancelled.
- PED-I10B7C-TV02 is cancelled.
- No AWS account, S3 bucket, IAM identity, credential, SDK, or adapter is required for the first-production Asset-storage path.

A later architecture decision may reopen cross-cloud storage only with new explicit Chief Architect authorization.

### DigitalOcean Spaces remains rejected

DigitalOcean Spaces remains **REJECTED** for the authoritative AIEOS Asset BlobStore under the current frozen contract.

Preserve accepted PED-I10B7B-TV01 evidence:

- ordinary PutObject overwrites
- conditional create behavior depended on undocumented capability
- multipart create-if-absent was not established
- provider-verified whole-object SHA-256 was not available through HEAD
- ordinary authoritative inspect would require full GET+rehash
- that operational model is unacceptable for [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) current-use

Do not reopen Spaces merely because DigitalOcean-only direction is now frozen. Provider fit must not weaken architecture.

### Frozen BlobStore invariants preserved

This ADR preserves without amendment:

- atomic create-new-only
- no overwrite
- opaque `storage_key`
- exact `byte_size`
- whole-object SHA-256
- authoritative physical inspect
- inspect `None` only for genuine object absence
- infrastructure/config/permission uncertainty ≠ missing
- non-destructive reconciliation
- provider credential ≠ AIEOS Principal
- no frontend storage authority
- no public Asset bytes

Do **not** replace SHA-256 with MD5, ETag, CRC32, CRC32C, CRC64, or a provider metadata claim merely to fit an implementation.

### Next provider class is investigation only

The next architecture investigation may evaluate AIEOS-operated object storage hosted on DigitalOcean infrastructure.

Candidate technologies may include:

- MinIO / AIStor
- another production-capable provider-neutral object store

**No product is selected by this ADR.** Do not state that MinIO or AIStor is the production provider.

### Kubernetes remains not required

Preserve [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md): Kubernetes is **not** required for first production.

Do not introduce DOKS/Kubernetes merely because a candidate storage product supports or prefers Kubernetes. The next architecture preflight must first evaluate a safe **non-Kubernetes** DigitalOcean-hosted topology. If Kubernetes becomes materially necessary, that requires a separate architecture decision.

### Operational requirement for any DigitalOcean-hosted store

The DigitalOcean-hosted storage architecture must explicitly evaluate:

- HA topology
- failure domains
- storage disks/volumes
- erasure coding or equivalent
- backup/recovery
- upgrades
- disk/node replacement
- monitoring
- TLS
- private VPC connectivity
- credential isolation
- capacity planning
- integrity/checksum semantics
- atomic no-overwrite semantics
- multipart behavior
- operator burden
- realistic first-production cost

Do not hide self-hosting operational cost.

### Development policy

Preserve the approved development plan.

- Local development: target $0 / ₹0 cloud infrastructure.
- Do not require a production object-storage cluster for normal local development.
- Provider validation should use local test doubles/emulators where valid, plus temporary explicitly authorized real-provider tests where needed.

### No provider selection

This ADR does **not** select DigitalOcean Spaces, MinIO, AIStor, Ceph, or another S3-compatible implementation. It freezes only the first-production **hosting direction**. A separate provider architecture decision is still required.

## Binding invariants

- DigitalOcean is the sole first-production hosting boundary for authoritative Asset object storage.
- The ADR-AIEOS-038 cross-cloud exception is dormant for first production; Amazon S3 does not advance.
- DigitalOcean Spaces remains rejected as the authoritative Asset BlobStore.
- Frozen BlobStore integrity semantics (create-new-only, no overwrite, whole-object SHA-256, inspect `None` only for genuine absence) are unchanged.
- Kubernetes is not required for first production and is not introduced by this ADR.
- No BlobStore provider is selected here.

## Explicit non-goals / deferred decisions

This ADR does **not** authorize:

- DigitalOcean Droplet creation
- DigitalOcean Volume creation
- DigitalOcean load balancer
- DOKS/Kubernetes
- MinIO/AIStor installation
- object-storage credentials
- OpenTofu apply
- Asset provider adapter
- Asset HTTP
- upload/download
- signed URL
- CDN
- Asset events
- purge
- retention
- legal hold
- PED-I03 mutation activation
- production migration
- production deployment

## Consequences

- First-production Asset-storage work proceeds as a DigitalOcean-hosted object-storage architecture preflight, not as AWS/S3 validation.
- AWS-BOOT-01 and PED-I10B7C-TV02 are cancelled for this first-production path.
- A later provider ADR is still required before any adapter, cluster, or runtime composition.
- [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) remains historically approved; its cross-cloud exception is not current first-production authority.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral BlobStore; no provider selected here |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | Whole-object SHA-256 inspect unchanged |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | Production cloud, BLR1, App Platform, PostgreSQL, NATS, Temporal Cloud, Kubernetes not required |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Historical approved cross-cloud exception; dormant for first production |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | Later first-production BlobStore provider selection (MinIO AIStor); this ADR remains hosting-only |
