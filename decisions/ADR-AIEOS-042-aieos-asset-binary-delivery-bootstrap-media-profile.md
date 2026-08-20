---
id: ADR-AIEOS-042
title: AIEOS Asset Binary Delivery & Bootstrap Media Profile
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-20
last_updated: 2026-08-20
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-042 — AIEOS Asset Binary Delivery & Bootstrap Media Profile

**Status:** Frozen / Approved  
**Date:** 2026-08-20  
**Related:** [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) · [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) · [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) · [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) · [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) · [ADR-AIEOS-041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) · [ADR-AIEOS-043](ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md)

**Catalogue note:** Frozen / Approved is architecture status. Founder approval of the Bootstrap architecture closure is recorded on **2026-08-20**. This ADR closes Bootstrap Asset HTTP/binary-delivery and maximum-size architecture questions. It does **not** authorize Asset HTTP implementation, BlobStore adapter implementation, production credentials, OpenTofu apply, cloud resources, PED-I03, production deployment, or purchase.

---

## Context

[ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) through [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) froze Asset SoR, current-use, mutation, and authorization/audit semantics while leaving Asset HTTP/binary delivery and maximum size open. [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) forbids frontend provider credentials, public Asset bytes, presigned/signed provider URL architecture, and CDN. [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) freezes App Platform as primary stateless compute. [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) freezes Bootstrap topology.

A Bootstrap-first binary delivery contract and media profile are now required so implementation cannot reopen banned transport paths or unbounded object sizes.

---

## A. Delivery path

Bootstrap Asset bytes use **authenticated API-mediated streaming**:

```text
Client
→ AIEOS API on App Platform
→ private MinIO AIStor
```

The following are **not** authorized for Bootstrap normal Asset access:

- frontend provider credentials
- public AIStor endpoint for normal Asset access
- public Asset bytes
- presigned/signed provider URLs
- CDN dependency
- direct-to-Spaces ingest
- direct client-to-AIStor ingest

Those paths require later architecture review.

---

## B. Ingest

Binding:

- Authorization must succeed before accepting authoritative Asset mutation.
- Byte handling must be **streaming**; do not require whole-file application spooling.
- Provider ingest remains **SINGLE-PUT ONLY** under [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md).
- Multipart provider ingest remains unauthorized.
- Provider-side atomic `If-None-Match: *` remains create-new-only authority.
- Opaque `storage_key` remains the physical lookup key.
- BlobStore/provider errors must preserve existing governance semantics ([ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md)).

---

## C. Bootstrap size profile

Global Bootstrap maximum Asset size:

**32 MiB**

This is an **AIEOS Bootstrap admission guardrail**, not a claim about the technical maximum of DigitalOcean App Platform or MinIO AIStor.

Eligible Bootstrap Asset families within that ceiling:

- `asset.image`
- `asset.document`
- `asset.audio`

`asset.video` byte ingest:

**NOT AUTHORIZED** for Bootstrap first production.

A later architecture decision may authorize video and/or increase the ceiling.

---

## D. Byte read / download

Binding:

- authenticated AIEOS API path
- current-use authority must succeed before bytes are released
- quarantine/current-use fail-closed rules remain binding
- BlobStore unavailable must not be represented as deterministic missing bytes
- provider ETag is not integrity authority
- exact AIEOS SHA-256 / byte-size truth remains governed by the existing Asset/BlobStore architecture
- Range request support is **not** required for Bootstrap v1
- CDN is not required

Exact HTTP paths, header syntax, framework mechanics, and streaming-buffer sizes are implementation/API-contract details provided they preserve this ADR.

---

## E. Precedence

Preserve:

- [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md)
- [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md)
- [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md)
- [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) / [036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md)
- [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md)
- [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md)

This ADR closes the previously open Bootstrap Asset HTTP/binary-delivery and maximum-size architecture questions.

It does **not** authorize implementation or production deployment.

---

## Explicit non-authorization

ADR-AIEOS-042 Frozen / Approved does **not** authorize:

- Asset HTTP implementation
- BlobStore production adapter implementation
- production credentials
- OpenTofu apply
- DigitalOcean resource creation
- PED-I03 Asset mutation activation
- production deployment
- purchase
- video Asset ingest
- raising the 32 MiB Bootstrap ceiling without a later architecture decision

Architecture freeze ≠ implementation authorization ≠ production authorization.

---

## Binding invariants

- Bootstrap Asset bytes traverse authenticated App Platform API → private AIStor only.
- No frontend provider credentials, public Asset bytes, presigned URLs, CDN, direct-to-Spaces, or direct client-to-AIStor for normal Bootstrap Asset access.
- Streaming ingest; single-PUT; `If-None-Match: *`; opaque `storage_key`.
- Global Bootstrap max size = 32 MiB; image/document/audio only; video not authorized.
- Download requires current-use success; BlobStore unavailable ≠ missing bytes; ETag not integrity authority.
- Range not required for Bootstrap v1.

## Consequences

- Catalogue current-summary surfaces treat Bootstrap Asset HTTP/binary delivery and Bootstrap maximum size as architecture-closed.
- Implementation must not reopen banned transport paths or admit video / >32 MiB without a later ADR.
- Commercial and cloud provisioning remain separately gated.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral Asset/BlobStore; HTTP was open |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | Current-use / governance unavailable preserved |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | Mutation semantics unchanged |
| [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) / [036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) | Auth/audit before authoritative mutation |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | App Platform primary stateless compute |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | Single-PUT; no public/presigned/CDN |
| [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) | Bootstrap topology class |
| [ADR-AIEOS-043](ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md) | Private AIStor service boundary |
| [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) / [041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) | Backup after authoritative Asset commit |
