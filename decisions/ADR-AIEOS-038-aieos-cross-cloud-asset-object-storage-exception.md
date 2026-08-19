---
id: ADR-AIEOS-038
title: AIEOS Cross-Cloud Asset Object Storage Exception
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-19
last_updated: 2026-08-19
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-038 — AIEOS Cross-Cloud Asset Object Storage Exception

**Status:** Frozen / Approved  
**Date:** 2026-08-19  
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md)

**Catalogue note:** Frozen / Approved is architecture status. This ADR does **not** select Amazon S3 as the production BlobStore. It does **not** authorize AWS resources, credentials, SDKs, adapters, Asset HTTP, signed URLs, CDN, or implementation. DigitalOcean remains the production cloud under [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md).

---

## Context

PED-I10B7B-TV01 established that DigitalOcean Spaces cannot satisfy the current frozen BlobStore architecture without an unacceptable integrity/runtime compromise. [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) already froze DigitalOcean as the production cloud and explicitly did **not** select Spaces.

AIEOS still needs a path for authoritative Asset binary storage that preserves [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) / [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) / [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md). This ADR authorizes a **narrow** exception that permits an external managed-cloud object-store **candidate** for that purpose only. It is **not** the final BlobStore provider selection.

## Decision

### DigitalOcean remains primary cloud

[ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) remains authoritative. DigitalOcean remains the production baseline for:

- application compute
- managed PostgreSQL
- AIEOS-operated NATS infrastructure
- primary runtime environment

This ADR does **not** reopen the production cloud decision.

### Why the exception exists

Accepted TV01 binding evidence:

- ordinary PutObject overwrites an existing key
- `If-None-Match: *` worked experimentally but is undocumented by DigitalOcean
- multipart conditional create was not proven
- HeadObject did not expose provider-verified whole-object SHA-256
- current authoritative SHA-256 would require full GET+rehash
- full GET+rehash is unacceptable as the ordinary [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) current-use inspect path for potentially large Asset objects
- Spaces therefore remains **rejected** under the current architecture

This ADR does **not** amend ADR-AIEOS-033 / 034 / 035.

### Managed cross-cloud exception

A managed external object-storage service may be used for authoritative Asset binary storage if it satisfies all frozen BlobStore invariants.

This is an explicit exception to the first-production preference for same-cloud infrastructure. The exception is limited to Asset object storage. It does **not** authorize an additional general-purpose application cloud.

### AWS S3 candidate status

Amazon S3 is the **sole provider candidate** authorized to advance to the controlled production-provider validation/design gate under this exception.

**This ADR does not yet select Amazon S3 as the production BlobStore.**

S3 remains a candidate until:

- controlled provider validation passes
- exact provider architecture is frozen separately

No AWS bucket, account, credential, SDK, or adapter is authorized by this ADR alone.

### Rejected / non-selected alternatives

Under the currently frozen SHA-256 BlobStore contract:

| Option | Status |
|--------|--------|
| DigitalOcean Spaces | **REJECTED** as preferred authoritative production BlobStore based on accepted TV01 evidence |
| Google Cloud Storage | Does not advance: current provider-native object checksum semantics do not provide the required AIEOS whole-object SHA-256 inspection contract |
| Azure Blob Storage | Does not advance for the same SHA-256 contract reason |
| Self-managed MinIO/AIStor | Remains a **fallback** option, not first-production recommendation, because first-production operational burden is materially higher |

These statuses do not alter the broader DigitalOcean cloud baseline.

### Integrity is not weakened

This ADR explicitly preserves:

- atomic create-new-only
- no overwrite
- exact byte size
- whole-object SHA-256
- authoritative physical inspect
- infrastructure unavailable ≠ object missing
- non-destructive reconciliation

Do **not** replace SHA-256 with MD5, CRC32, CRC32C, CRC64, ETag, or a user-metadata claim merely to fit a provider.

### Cross-cloud security

External object storage must remain:

- private
- backend controlled
- non-public
- isolated between production and non-production
- accessed over authenticated TLS
- least privilege

External cloud credentials are infrastructure credentials. They are never:

- AIEOS Principal
- tenant authority
- membership authority
- business capability
- frontend authority

Preserve [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) and [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md).

### No frontend / provider coupling

- Frontend clients receive no provider credentials.
- No public object URLs are created by this exception.
- No presigned/signed URL architecture is authorized.
- No CDN is authorized.
- Content publication does not imply public Asset bytes.
- `ResourceRef` does not become a provider URL.

### Scope of AWS exception

If S3 is ultimately frozen by a later provider ADR, the AWS exception is confined to Asset binary object storage and its necessary object-storage control plane.

This ADR does **not** authorize AWS as the provider for: application compute, PostgreSQL, NATS, Temporal, frontend hosting, observability, DNS, generic secret management, or other AIEOS domain stores. Any expansion requires a later architecture decision.

### Cross-cloud networking

DigitalOcean → external object-store communication may use authenticated public TLS where no cross-provider private-network path exists. This is compatible with ADR-037's private-where-supported rule. The external provider endpoint is infrastructure detail and not an Asset public-delivery URL.

### Operational rationale

Managed cross-cloud object storage is preferred over introducing a self-managed object-storage cluster solely to preserve single-cloud purity when the managed external provider demonstrably satisfies AIEOS integrity semantics.

The architecture optimizes for correctness, integrity, operational simplicity, and a small production team rather than artificial cloud purity.

### Development policy

This ADR does not require AWS or another production object store for local development.

Approved development operating posture remains:

- local development: target $0 / ₹0 cloud infrastructure
- real-provider tests: temporary, explicitly gated, disposable

No permanent AWS DEV storage is authorized by this ADR.

### Provider validation prerequisite

Before final S3 provider freeze, a controlled disposable AWS validation must prove at least:

- concurrent conditional PutObject no-overwrite
- If-None-Match enforcement
- bucket-policy enforcement where used
- PutObject whole-object SHA-256
- HeadObject ChecksumMode exact SHA-256 retrieval
- missing object vs missing bucket handling
- 403/error mapping
- versioning-off delete
- LIST → HEAD inventory
- bounded spool compatibility
- same-attempt ambiguous outcome recovery
- region suitability

This validation does not itself authorize implementation.

### Size / multipart boundary

This ADR does not weaken whole-object SHA-256 for multipart uploads.

If a provider only provides composite SHA-256 for multipart objects, that composite checksum must **not** be substituted for the AIEOS whole-object `sha256`.

Large-object / multipart integrity may require a later bounded architecture decision.

## Binding invariants

- DigitalOcean remains the production AIEOS cloud ([ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md)).
- DigitalOcean Spaces remains rejected as the preferred authoritative production Asset BlobStore under the current frozen contract.
- Amazon S3 is a **candidate only** until a later provider ADR freezes it.
- Frozen BlobStore invariants (create-new-only, no overwrite, exact size, whole-object SHA-256, inspect `None` only for genuine absence) are unchanged.
- External object-store credentials are infrastructure credentials, never AIEOS Principal.

## Explicit non-goals / deferred decisions

This ADR does **not** authorize:

- S3 as final production provider yet
- AWS infrastructure creation
- AWS account/project creation
- S3 buckets
- IAM users/roles
- credentials
- boto3/botocore
- provider adapter
- Asset runtime composition
- Asset HTTP
- Asset upload/download
- public delivery
- signed/presigned URLs
- CDN
- Asset events
- purge
- retention
- legal hold
- PED-I03 mutation activation
- production migration
- production deployment

## Consequences

- Architecture may proceed to a controlled S3 validation/design gate without reopening DigitalOcean as production cloud.
- A later BlobStore provider ADR is still required before any adapter or runtime composition.
- Integrity architecture (SHA-256, no-overwrite) is not revised by this exception.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Infra credential ≠ Principal |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Authorization kernel unchanged |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral BlobStore invariants preserved |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | Authoritative physical SHA-256 inspect preserved |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | Mutation/activation unchanged |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | Production cloud baseline; Spaces not selected there |
