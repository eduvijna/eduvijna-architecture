---
id: ADR-AIEOS-043
title: AIEOS Bootstrap AIStor Service Boundary & Primary Namespace
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-20
last_updated: 2026-08-20
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-043 — AIEOS Bootstrap AIStor Service Boundary & Primary Namespace

**Status:** Frozen / Approved  
**Date:** 2026-08-20  
**Related:** [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) · [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) · [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) · [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) · [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md)

**Catalogue note:** Frozen / Approved is architecture status. Founder approval of the Bootstrap architecture closure is recorded on **2026-08-20**. This ADR freezes Bootstrap AIStor private service boundary, TLS trust invariants, and primary namespace posture. It does **not** authorize Droplet/Volume creation, certificates, credentials, DNS changes, AIStor install, OpenTofu apply, or deployment.

---

## Context

[ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) freezes Bootstrap topology (single-node AIStor Free, 6 × ~190 GiB, N=6/K=3/M=3, EC:3, BLR1). [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) requires private, authenticated, non-public Asset byte storage. [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) requires App Platform → private AIStor delivery.

Service-boundary, TLS trust, and primary-namespace posture must be frozen so infrastructure design cannot reopen public exposure, plaintext fallback, or ambiguous primary Versioning/Object Lock.

---

## A. Service boundary

Bootstrap AIStor is **PRIVATE-SERVICE-ONLY** inside the DigitalOcean production network boundary.

Normal AIEOS runtime access:

```text
AIEOS App Platform / authorized workers
→ DigitalOcean private-network path
→ AIStor Bootstrap node
```

No normal public AIStor S3 service exposure.

---

## B. Service identity

Applications use a **stable logical AIStor service identity** mapped to the private endpoint.

This ADR does **not** freeze a specific DigitalOcean DNS product unless current provider capability actually requires it.

The implementation may use DNS, service discovery, or equivalent configuration as an EDR provided:

- application business configuration is not tied to ephemeral node IP
- replacement can safely remap service identity
- private-only semantics remain preserved

---

## C. TLS

Authenticated encrypted transport is mandatory.

Require:

- TLS certificate validation
- explicit AIEOS-controlled trust anchor
- independently rotatable certificate/trust material
- no plaintext fallback
- no `verify=false` / certificate bypass

Exact CA/certificate tooling is EDR unless it weakens these invariants.

---

## D. Load balancer

Bootstrap receives **no dedicated AIStor load balancer**.

A load balancer does not remove the single-node Bootstrap failure domain and is not required by the frozen Bootstrap architecture.

[ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) historical Scale topology retains its own distributed/LB semantics.

---

## E. Primary namespace

Production primary AIStor storage uses:

**one dedicated primary bucket per environment.**

Production and non-production buckets must remain isolated.

Literal bucket names: **EDR / configuration — NOT frozen by this ADR.**

`storage_key`: opaque; no tenant/business identity parsing.

| Posture | Bootstrap freeze |
|---------|------------------|
| Primary Versioning | **OFF** |
| Object Lock | **OFF** |
| Automatic lifecycle expiration | **NONE** until physical purge/retention/legal-hold architecture exists |

---

## F. Delete authority

Ordinary application runtime delete authority: **FORBIDDEN**.

Ordinary runtime must not possess general administrative bucket authority.

Break-glass/admin physical-delete authority:

- separate identity
- governed
- auditable
- not embedded in App Platform normal runtime

Future physical purge/retention/legal-hold work may revise the lifecycle model through a later architecture decision.

---

## G. Preserved provider correctness

Preserve [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md):

- MinIO AIStor
- atomic `If-None-Match: *`
- provider-returned whole-object SHA-256
- exact byte size
- single-PUT
- fail-closed required storage

Preserve [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md):

- Bootstrap single-node AIStor Free
- 6 × ~190 GiB Volumes
- N=6 / K=3 / M=3
- EC:3
- BLR1

This ADR does **not** authorize resources, certificates, credentials, DNS changes, install, OpenTofu, or deployment.

---

## Explicit non-authorization

ADR-AIEOS-043 Frozen / Approved does **not** authorize:

- creating a Droplet or Volumes
- AIStor production install
- certificate issuance/purchase
- DNS changes
- production credentials
- OpenTofu apply
- production deployment
- purchase

Architecture freeze ≠ implementation authorization ≠ production authorization.

---

## Binding invariants

- Bootstrap AIStor is private-service-only on the DigitalOcean private-network path from App Platform / authorized workers.
- Stable logical service identity; not ephemeral IP as business configuration.
- TLS with AIEOS-controlled trust anchor; no plaintext; no verify bypass.
- No dedicated Bootstrap AIStor load balancer.
- One dedicated primary bucket per environment; Versioning OFF; Object Lock OFF; no auto-expiry lifecycle until purge architecture exists.
- Opaque `storage_key`; ordinary runtime delete forbidden; break-glass delete is separate/governed/auditable.

## Consequences

- Catalogue current-summary surfaces treat Bootstrap AIStor service boundary and primary namespace posture as architecture-closed.
- Infrastructure EDRs may choose DNS product, cert tooling, and literal bucket names without reopening public exposure or Versioning/Object Lock.
- Scale Production continues to follow ADR-AIEOS-040 distributed/LB semantics separately.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | DigitalOcean production cloud; App Platform |
| [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | DigitalOcean-only hosting |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | Provider correctness preserved |
| [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) | Historical Scale LB/private topology |
| [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) | Bootstrap topology class |
| [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) | API → private AIStor delivery |
| [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) | Backup remains non-primary Spaces |
