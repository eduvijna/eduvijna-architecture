---
id: ADR-AIEOS-040
title: AIEOS Asset BlobStore First-Production Topology
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-19
last_updated: 2026-08-19
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-040 — AIEOS Asset BlobStore First-Production Topology

**Status:** Frozen / Approved  
**Date:** 2026-08-19  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) · [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md)

**Catalogue note:** Frozen / Approved is architecture status. This ADR freezes the first-production **AIStor topology** only. It does **not** reopen provider selection ([ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md)). It does **not** freeze production compute sizing, production Volume capacity, commercial purchase, object backup/DR, Asset maximum size, BlobStore adapter implementation, Asset HTTP, PED-I03, OpenTofu, or production deployment.

---

## Context

[ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) froze DigitalOcean as the production cloud baseline and BLR1 as initial production locality. [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) recorded a historical cross-cloud Asset-storage exception. [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) is the current DigitalOcean-only first-production Asset hosting direction. [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) selected MinIO AIStor as the first-production Asset BlobStore software provider but left topology, substrate, compute SKU, and capacity open.

A first-production AIStor topology is now required so later infrastructure, adapter, and runtime work cannot silently reopen node count, erasure-set geometry, storage substrate, or private service topology.

---

## Decision chain (not reopened here)

| ADR | Role |
|-----|------|
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | DigitalOcean production cloud baseline |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Historical cross-cloud Asset-storage exception |
| [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | Current DigitalOcean-only first-production Asset hosting direction |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | MinIO AIStor software provider selected |
| **ADR-AIEOS-040 (this ADR)** | First-production AIStor topology selected |

Provider selection is closed. This ADR does **not** substitute Spaces, Garage, Amazon S3, or another provider.

---

## A. Frozen topology decisions

### Cluster geometry

First-production AIStor topology:

- **8 AIStor nodes**
- **× exactly 1 dedicated AIStor data device per node**
- **Hosted in DigitalOcean BLR1**
- **Provider: MinIO AIStor**
- **Operation: AIEOS-operated**
- **Deployment style: distributed**

One VM/Droplet failure domain maps to **exactly one** AIStor data device.

**4×2** and **6×2** layouts are **not** active first-production alternatives. They are rejected for this topology decision because one VM failure removes two AIStor data devices. Historical or capability evidence for other layouts may be recorded but does not reopen this freeze.

### First pool erasure coding

Binding first pool:

| Parameter | Value |
|-----------|-------|
| Total AIStor data devices | 8 |
| Erasure sets | **one** |
| N | 8 |
| Parity | **EC:3** |
| K (data shards) | 5 |
| M (parity shards) | 3 |
| EC efficiency | 5/8 = 62.5% |

The production initialization gate **must positively establish** one set, N=8, K=5, M=3 before production Asset bytes are admitted. Do **not** assume geometry purely from drive count. If actual initialized geometry differs, production activation **must stop**. Do **not** silently adapt ADR-040 to a different set geometry.

### Storage substrate

Binding first-production substrate:

- **DigitalOcean Volumes**
- **Exactly 1 dedicated DigitalOcean Volume per AIStor node**
- The operating-system/root disk **must not** be used as AIStor data storage
- The Volume is the **only** AIStor data device on that node

DigitalOcean Volumes are **network-attached block storage**. They are **not** direct-attached JBOD, local NVMe, independent from DigitalOcean storage fabric, or a separate durability domain equivalent to an independent physical disk.

Using DigitalOcean Volumes is an **accepted first-production deviation** from MinIO's preferred direct-attached/JBOD production guidance. Residual correlated DigitalOcean block-storage/fabric risk remains. This decision does **not** assert that underlying DigitalOcean/Ceph replication is an independent AIStor erasure failure domain.

### Filesystem and mount identity

Binding:

- **XFS**
- Stable filesystem/device identity must be used
- Required data mount startup behavior is **FAIL CLOSED**

AIStor **must not** start if any of the following is true:

- data Volume is missing
- mount path is only an empty root-disk directory
- wrong filesystem is mounted
- wrong XFS filesystem/Volume identity is mounted
- expected filesystem label/identity does not match
- expected device count does not match
- data path resolves to root/OS disk
- mandatory mount validation fails

Do **not** use `nofail` or equivalent semantics that permit required AIStor data mounts to disappear silently. Exact implementation mechanism remains an infrastructure implementation detail; the fail-closed behavior is binding.

### Network topology

Binding architecture:

- AIStor remains **PRIVATE**
- Region: **BLR1**
- Network: **DigitalOcean VPC**
- Client/service path: AIEOS runtime → private/internal AIStor service endpoint → AIStor cluster
- Preferred/frozen endpoint topology: **internal regional DigitalOcean load balancer with TLS passthrough**
- AIStor node ↔ AIStor node: **direct private TLS**
- **No public AIStor S3 endpoint**
- **No plaintext fallback**

The load balancer is **not** part of AIStor storage quorum. If the private service endpoint is unavailable, BlobStore is unavailable and [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) governance-unavailable / fail-closed behavior applies. The AIEOS BlobStore adapter is **not** required to maintain all eight node addresses as a client-side failover topology.

### Stable node identity

Production AIStor node identity/configuration must use stable logical identities. Cluster correctness must not depend directly on an ephemeral Droplet private IP remaining unchanged forever. Exact mechanism remains open to infrastructure design (for example private DNS, stable logical naming, or an equivalent approved internal identity mechanism). This ADR does **not** freeze a specific DNS product.

### Create-new-only / inspect invariants (inherited)

This ADR inherits and does **not** redefine [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) and [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) storage invariants:

- atomic create-new-only: `If-None-Match: *`
- duplicate create: provider precondition failure; no overwrite fallback
- authoritative inspect: whole-object SHA-256 + exact byte size without ordinary full-object GET during routine inspect
- ETag alone is not the authoritative content digest
- caller-supplied metadata alone is not the authoritative content digest
- **Multipart remains UNAUTHORIZED**

### Expansion model

The first N=8 set is **not** a drive-at-a-time mutable capacity shape. Expansion is through additional AIStor server pool(s) under a separately validated/frozen capacity design. Adding one Volume to the existing N=8 set is **not** the production scaling model.

### Failure-domain semantics (frozen topology implications)

| Unavailable devices | Topology implication |
|---------------------|----------------------|
| 1 node | 1 AIStor device unavailable |
| 2 nodes | 2 devices unavailable; within validated read/write operating margin |
| 3 devices | EC:3 protection/quorum boundary |
| 4 devices | beyond frozen parity; storage operations must fail safely |

Empirical evidence that three unavailable devices continued reads/writes **must not** be interpreted as routine maintenance headroom. Production operational policy should normally perform planned maintenance **one node at a time** and restore healthy protection before the next planned node operation. This ADR does **not** freeze a policy that intentionally operates continuously at the three-device boundary.

### Security / authority boundary (preserved)

- AIStor infrastructure/runtime credential ≠ AIEOS Principal ([ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md), [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md))
- AIStor IAM does not decide tenant, teacher, learner, Asset lifecycle, or business authorization
- Production runtime identity must **not** be AIStor root/admin
- No production secrets in Git, OCI images, test fixtures, logs, or OpenTofu source
- Do not add delete/purge permission merely because future purge capability may exist

---

## B. Empirically validated behavior

**PED-I10B7E-TV04-R2** provided empirical **NON_PRODUCTION** validation of this frozen topology. Chief Architect accepted the following evidence. Raw temporary validation artifacts are **not** deposited as canonical files in this architecture repository; this ADR records the accepted empirical result only.

| Evidence area | Accepted result |
|---------------|-----------------|
| Baseline | 8 AIStor nodes; 1 dedicated DigitalOcean Volume/node; XFS; all 8 nodes healthy |
| Erasure geometry | Single erasure set positively reported: N=8, K=5, M=3 |
| FD01 process stop | Reads/writes continued |
| FD02 one node down | 7/8 available; reads/writes continued |
| FD03 two nodes down | 6/8 available; reads/writes continued |
| FD04 three nodes down | 5/8 available; reads/writes continued at EC:3 boundary |
| FD05 four nodes down | 4/8 available; PUT/GET failed safely; no split-brain write acceptance observed |
| FD06 node + unrelated Volume | 6/8 available; missing-Volume node refused startup; reads/writes continued |
| FD07 missing mount | AIStor refused startup |
| FD08 wrong XFS label | AIStor refused startup |
| FD09 node replacement | Replacement node rejoined; integrity remained correct |
| FD10 Volume replacement | Replacement Volume reconstructed from surviving EC:3 members; healthy operation returned |
| Network | Private internal LB with TLS passthrough worked; direct private internode TLS worked; no public S3 endpoint required |
| Create-new-only | `If-None-Match: *` first create succeeded; duplicate different-byte create returned 412; original bytes unchanged |
| Inspect | Provider SHA-256 + exact byte-size inspection proven without routine ordinary GET |
| Cleanup | Zero surviving billable TV04-R2 resources |

**Validation references (not canonical evidence files here):**

| PED | Role |
|-----|------|
| PED-I10B7E-TD | Topology design |
| PED-I10B7E-LIC01 | Temporary Enterprise Trial entitlement gate |
| PED-I10B7E-TV04 | Initial license-blocked attempt |
| PED-I10B7E-TV04-R1 | Account-tier / GP-4×16 blocked attempt; **not** topology proof |
| PED-I10B7E-TV04-R2 | Successful functional topology validation |

R1 did **not** prove topology. R2 used functional validation compute only (see deferred section).

---

## C. Accepted deviations / residual risks

| Topic | Accepted deviation / residual risk |
|-------|-----------------------------------|
| DigitalOcean Volumes vs MinIO JBOD preference | Network-attached Volumes accepted as first-production substrate; correlated DO block-storage/fabric risk remains |
| DO replication vs AIStor erasure | Underlying DO/Ceph replication is **not** treated as an independent AIStor erasure failure domain |
| Replacement private IP change | TV04-R2 demonstrated replacement-node private IP change; stable logical identity mechanism required in production |
| Cluster-wide restart during replacement | TV04-R2 required cluster-wide MinIO restart after node replacement; production runbooks should aim to avoid unnecessary cluster-wide restart |
| Explicit heal CLI | TV04-R2 Volume replacement succeeded via implicit EC reconstruct; production runbook for supported heal procedure remains open |

---

## D. Explicitly deferred / not frozen

### Compute sizing — explicitly not frozen

TV04-R2 used `g-2vcpu-8gb` (2 dedicated vCPU, 8 GiB RAM) **solely as a functional validation SKU**. It **must not** appear in this ADR as production minimum, recommendation, capacity baseline, or SLA baseline. It may be recorded only as functional validation environment evidence.

Production vCPU, RAM, network throughput, instance class, healing headroom, concurrency sizing, and performance/capacity thresholds remain **OPEN** and require a separate production capacity/performance gate. Vendor recommendations may be referenced as context but must not be silently transformed into frozen AIEOS production sizing.

### Volume size / capacity — not frozen

TV04-R2 used 100 GiB Volumes only for temporary validation. **100 GiB is not production Volume sizing.**

This ADR freezes **one Volume/node** but does **not** freeze Volume GiB/TiB, production raw capacity, usable capacity, ingest forecast, retention, growth, object count, or capacity reserves. These remain a later capacity-design gate.

### Commercial posture — not frozen

Production requires a valid paid AIStor entitlement permitting distributed operation. **Enterprise** remains the Chief Architect preferred production commercial posture. This ADR does **not** authorize or freeze quote, price, paid order, purchase, contract execution, or subscription activation. The temporary Enterprise Trial used in TV04 is **not** a production license. Commercial purchase/tier execution remains a separate gate.

### Object backup / DR — not frozen

**EC is NOT backup.**

This ADR does **not** freeze Asset-object RPO, RTO, cross-region replication, object backup, accidental-delete recovery, or regional disaster recovery. Object backup/DR remains a required separate production-readiness architecture decision before production activation. Absence of that later ADR does **not** invalidate this topology freeze; it **does** block final production readiness.

### Asset maximum size — not frozen

Single-PUT remains binding. Multipart remains unauthorized. Future maximum Asset size must be frozen at or below the provider's supported non-multipart single-PUT limit and any stricter AIEOS HTTP/spooling/runtime limit. This ADR does **not** choose the number.

### Operational readiness — not fully closed

The following remain later production-readiness work:

- exact supported node replacement runbook
- exact supported Volume replacement/healing runbook
- avoiding unnecessary cluster-wide restart during replacement
- mount guard implementation
- TLS certificate lifecycle
- private DNS/node identity implementation
- capacity alarm thresholds
- healing gates
- quorum gates
- license expiry alerting
- Volume latency/IOPS alerts
- cluster health routing
- observability backend/routing
- upgrade procedure
- backup/DR
- compute sizing

TV04-R2 replacement evidence is sufficient for **topology freeze** only.

### Future pool expansion — not frozen

Next pool size, node count, parity, and future capacity are **not** frozen.

---

## E. Explicit non-authorization

ADR-AIEOS-040 Frozen / Approved does **not** authorize:

- DigitalOcean production resource creation
- AIStor production cluster creation
- OpenTofu apply or OpenTofu implementation
- paid AIStor purchase, quote acceptance, or production license activation
- production DNS, certificates, or credentials
- BlobStore adapter implementation
- Asset HTTP implementation
- multipart
- Asset maximum-size freeze
- capacity sizing freeze
- production compute sizing freeze
- object backup/DR design
- schema-owner changes
- database migrations
- PED-I03 activation
- production mutation
- production deployment

Architecture status ≠ implementation authority ≠ production authority.

---

## Binding invariants

- First-production AIStor topology is **8 nodes × 1 dedicated DigitalOcean Volume/node × XFS × one erasure set N=8 EC:3 (K=5, M=3)** in DigitalOcean BLR1, AIEOS-operated, distributed, private.
- One VM failure domain = one AIStor data device.
- OS/root disk must not be AIStor data storage.
- Required mount validation is fail-closed; `nofail` for required data mounts is forbidden.
- Private internal LB with TLS passthrough is the preferred frozen service endpoint topology; no public S3; no plaintext fallback.
- Create-new-only, SHA-256 inspect, and multipart-unauthorized invariants are inherited unchanged from ADR-033/039.
- EC is not backup.
- Compute SKU, Volume capacity, commercial purchase, backup/DR, adapter, Asset HTTP, and production deployment remain unselected/unauthorized.

## Consequences

- First-production BlobStore infrastructure design proceeds against this frozen topology, not against open node-count or substrate choice.
- [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) provider selection is unchanged and closed.
- Production readiness still requires separate gates for compute/capacity sizing, commercial entitlement, backup/DR, runbooks, adapter, Asset HTTP, and explicit production authorization.
- Kubernetes is not introduced by this ADR.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) | Platform technology baseline |
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Infra credential ≠ Principal |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | PostgreSQL Asset metadata SoR |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Deploy/live/ready ≠ mutation; this ADR does not authorize production |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Authorization kernel unchanged |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral BlobStore invariants; this ADR supplies topology |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | BlobStore unavailable = governance unavailable |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | Mutation/activation unchanged |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | DigitalOcean production cloud baseline; BLR1 |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Historical cross-cloud exception; dormant for first production |
| [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | DigitalOcean-only hosting direction |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | MinIO AIStor provider selection; topology was open there |
