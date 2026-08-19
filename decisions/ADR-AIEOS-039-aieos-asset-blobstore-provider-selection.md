---
id: ADR-AIEOS-039
title: AIEOS Asset BlobStore Provider Selection
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-19
last_updated: 2026-08-19
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-039 — AIEOS Asset BlobStore Provider Selection

**Status:** Frozen / Approved  
**Date:** 2026-08-19  
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) · [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md)

**Catalogue note:** Frozen / Approved is architecture status. This ADR selects the first-production Asset BlobStore **software provider**. It does **not** select production topology, DigitalOcean storage substrate, compute SKU, capacity, load-balancing layout, backup/DR, RPO/RTO, or MinIO commercial tier. It does **not** authorize purchase, DigitalOcean resources, OpenTofu apply, BlobStore adapter implementation, Asset HTTP, PED-I03 mutation, or production deployment.

---

## Context

[ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) froze a provider-neutral BlobStore boundary. [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) froze DigitalOcean as the production cloud and did not select DigitalOcean Spaces. [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) recorded a historical cross-cloud managed-store exception and allowed Amazon S3 to advance as a candidate only. [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) froze DigitalOcean as the sole first-production Asset-storage **hosting** boundary, made the ADR-AIEOS-038 cross-cloud path dormant for first production, and left provider selection open.

A first-production BlobStore software provider is now required so later topology, licensing, and adapter work cannot reopen provider choice. Provider fit must preserve the frozen BlobStore contract: atomic create-new-only, exact byte size, whole-object SHA-256 inspection without ordinary full-object GET, opaque `storage_key`, inspect `None` only for genuine absence, and PostgreSQL remaining Asset metadata/authority truth.

## Decision

### Provider

**MinIO AIStor** is selected as the first-production AIEOS Asset BlobStore software provider.

The provider is **AIEOS-operated**. Hosting remains inside the DigitalOcean boundary established by [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md). DigitalOcean remains the production cloud under [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md).

This ADR does **not** introduce Kubernetes. Preserve ADR-AIEOS-037: Kubernetes is not required for first production.

### Provider selection is closed

The following are **not** open implementation choices:

- DigitalOcean Spaces
- Garage
- Amazon S3
- another S3-compatible object store
- another cloud object-store provider

Provider substitution requires architecture review.

### Production distribution

Production AIStor **must** be distributed. A single-node / AIStor Free topology is not first-production.

### Erasure coding

Production must use **EC:3 or greater**.

EC:1 and EC:2 are not authorized for production.

This ADR does **not** freeze erasure-set size, node count, or drive layout.

### Ingest

First-production Asset ingest remains **SINGLE-PUT ONLY**.

Multipart Asset ingest remains unauthorized. Multipart is not an implementation fallback.

### Create-new-only correctness

Provider-side atomic `If-None-Match: *` is the authoritative create-new-only / no-overwrite primitive.

Do **not** substitute:

- read-before-write
- HEAD-before-PUT
- application locking
- caller-generated uniqueness alone

for provider atomic conditional creation.

Bucket-policy enforcement is defense-in-depth. It is **not** the binding correctness primitive for create-new-only.

### Object integrity / inspection

Authoritative BlobStore inspection requires provider-returned:

- whole-object SHA-256
- exact byte size

without an ordinary full-object GET.

ETag does not substitute for SHA-256. Caller metadata does not substitute for provider-returned SHA-256. Do **not** replace SHA-256 with MD5, CRC32, CRC32C, CRC64, or a user-metadata claim.

Preserve [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md): BlobStore unavailable is governance unavailable, not a deterministic “missing bytes” rejection.

### Mount safety

Where mounted data devices are used:

required storage absent → AIStor startup **must FAIL CLOSED**.

An existing empty mount directory must never silently become local/root storage.

### Validated topologies are not production selection

Any prior 4-node × 2-drive EC:3 validation demonstrates **capability only**.

It does **not** freeze:

- production node count
- production drive count
- production compute SKU
- production storage capacity
- production failure-domain model

### DigitalOcean Volumes

Prior validation of DigitalOcean Volumes demonstrates that they can function as a substrate.

It does **not** freeze DigitalOcean Volumes as the final production substrate.

Production topology and storage substrate remain a **separate** architecture decision.

### Authority boundary preserved

Preserve existing frozen separation:

- Asset PostgreSQL SoR → Asset metadata / lifecycle / authority truth
- BlobStore / AIStor → physical-byte storage truth
- Authorization Kernel → current business authority
- Security Audit → audit truth
- Infrastructure / provider credential ≠ AIEOS Principal ([ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md), [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md))

Frontend clients receive no provider credentials. No public Asset bytes. No presigned/signed URL architecture and no CDN are authorized by this ADR.

### Commercial boundary

Distributed production AIStor requires an appropriate MinIO commercial license.

This ADR does **not** select:

- Enterprise Lite vs Enterprise
- contract term
- support tier
- purchase price
- procurement authority

Commercial purchase remains separately governed.

## Binding invariants

- MinIO AIStor is the first-production AIEOS Asset BlobStore software provider and is AIEOS-operated on DigitalOcean.
- Provider substitution is closed without architecture review.
- Production AIStor is distributed and uses EC:3 or greater.
- First-production Asset ingest is single-PUT only; multipart ingest is unauthorized.
- Create-new-only is provider-side atomic `If-None-Match: *`.
- Authoritative inspect is provider-returned whole-object SHA-256 and exact byte size without ordinary full-object GET.
- Bucket policy is not the create-new-only correctness primitive.
- Required mounted data storage absent → fail closed; empty mount directories must not become node-local root storage.
- Topology, substrate, SKU, capacity, load-balancing, backup/DR, RPO/RTO, and commercial tier remain **unselected**.
- This ADR does not authorize purchase, infrastructure creation, adapter implementation, Asset HTTP, PED-I03, or production deployment.

## Explicit non-goals / deferred decisions

This ADR does **not** authorize or freeze:

- AIStor purchase
- trial activation as production
- MinIO quote acceptance
- DigitalOcean resource creation (Droplets, Volumes, VPC, load balancer, DNS, certificates)
- production credentials
- OpenTofu implementation or apply
- BlobStore production adapter implementation
- Asset HTTP upload or download
- multipart ingest
- database migrations
- PED-I03 activation
- production mutation
- production deployment
- artifact promotion
- production data movement
- production authority seeding
- ADR-AIEOS-040 topology selection

Still open after this ADR:

- DigitalOcean storage substrate
- node count
- data-device count
- erasure-set layout
- compute SKU
- RAM/vCPU sizing
- storage capacity
- load-balancing topology
- exact production licensing tier
- backup/DR topology
- RPO/RTO
- performance sizing

## Evidence / history

Recoverable from the canonical architecture catalogue:

| Option | Status recorded in catalogue | Source |
|--------|------------------------------|--------|
| DigitalOcean Spaces | **REJECTED** as authoritative production Asset BlobStore under the frozen integrity contract | [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) / [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md), citing accepted **PED-I10B7B-TV01** (ordinary PutObject overwrite; undocumented conditional create; no provider-verified whole-object SHA-256 via HEAD; GET+rehash unacceptable for ordinary current-use inspect) |
| Amazon S3 | Historically a **candidate** under ADR-AIEOS-038; **dormant** for first production under ADR-AIEOS-038R1 DigitalOcean-only hosting direction; **not** an open implementation choice after this ADR | ADR-AIEOS-038, ADR-AIEOS-038R1 |
| MinIO / AIStor | Named as a DigitalOcean-hosted investigation candidate in ADR-AIEOS-038R1; **not selected there** | ADR-AIEOS-038R1 |
| MinIO AIStor | **Accepted** first-production provider | This ADR |

Chief Architect accepted additional provider-validation evidence that is **not** present as canonical files in this architecture repository, including Garage rejection (create-new-only conditional semantics failed) and MinIO AIStor acceptance characteristics such as atomic create-new-only, SHA-256 inspection, inventory, distributed operation, degraded operation/healing, DigitalOcean VPC operation, TLS, non-public exposure, mount fail-closed behavior, and persistence.

Exact PED-I10B7D / TV01 / TV02A / TV02B / TV03 evidence packages are **not** deposited in this repository beyond the Spaces TV01 summary already recorded in ADR-AIEOS-038 / 038R1. Chief Architect accepted evidence exists **outside** the current canonical architecture catalogue and requires later evidence-package synchronization.

That evidence-catalogue gap does **not** reopen this already-frozen provider decision.

## Consequences

- First-production BlobStore work proceeds as DigitalOcean-hosted **MinIO AIStor** topology, commercial, and adapter design — not as Spaces, Garage, or Amazon S3 selection.
- [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) remains historically approved; its cross-cloud exception remains dormant for first production.
- [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) remains the hosting-boundary ADR; this ADR supplies the missing provider selection.
- Frozen BlobStore integrity semantics are unchanged.
- A later topology ADR is still required before any cluster, OpenTofu, or runtime composition.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Provider credential ≠ Principal |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Deploy/live/ready ≠ mutation; this ADR does not authorize production |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Authorization kernel unchanged |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral BlobStore invariants; this ADR supplies the provider |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | Whole-object SHA-256 inspect unchanged |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | Mutation/activation unchanged |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | DigitalOcean production cloud; Kubernetes not required |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Historical approved cross-cloud exception; dormant for first production |
| [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | DigitalOcean-only hosting boundary; provider not selected there |
