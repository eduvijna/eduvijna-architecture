---
id: ADR-AIEOS-040R1
title: AIEOS Asset BlobStore Bootstrap & Scale Production Topology
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-20
last_updated: 2026-08-20
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-040R1 — AIEOS Asset BlobStore Bootstrap & Scale Production Topology

**Status:** Frozen / Approved  
**Date:** 2026-08-20  
**Related:** [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) · [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) · [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) · [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md)

**Catalogue note:** Frozen / Approved is architecture status. Founder commercial/risk approval preceded this freeze. This ADR is a **forward revision** of topology classification relative to [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md). It does **not** delete, reject, or rewrite ADR-AIEOS-040. ADR-AIEOS-040 remains Frozen / Approved as the historical decision that originally classified the 8-node × 1-device distributed AIStor topology as First Production. This ADR reclassifies that topology as **Scale Production** and establishes Founder-approved **Bootstrap Production**. It does **not** reopen [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) provider selection. It does **not** authorize Droplet/Volume creation, OpenTofu, credentials, BlobStore adapter implementation, PED-I03, production deployment, commercial purchase, or Scale Production purchase.

---

## Context

[ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) selected MinIO AIStor as the authoritative Asset BlobStore software provider and stated that production AIStor must be distributed. [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) froze the empirically validated 8×1 distributed topology as First Production.

Commercial and risk review now requires an explicit two-class topology model: a temporary low-cost **Bootstrap Production** class, and a **Scale Production** class that preserves the historical ADR-AIEOS-040 distributed topology. This ADR freezes that classification without inventing new provider semantics or erasing historical decisions.

---

## Precedence

| ADR | Role after this freeze |
|-----|------------------------|
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | Remains Frozen / Approved for provider selection and BlobStore correctness invariants |
| [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) | Remains Frozen / Approved as **historical** topology evidence; original First Production classification |
| **ADR-AIEOS-040R1 (this ADR)** | **Current** topology classification: Bootstrap Production + Scale Production |

Where topology classification conflicts:

- later specific **ADR-AIEOS-040R1** governs topology classification
- [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) statement that production AIStor must be distributed is superseded **ONLY** for the **Bootstrap Production** class
- for **Scale Production**, distributed AIStor remains required
- all other ADR-AIEOS-039 provider and correctness invariants remain unchanged

This is a targeted precedence rule, not a provider-selection reopening.

---

## A. Bootstrap Production

Frozen Bootstrap Production topology:

| Parameter | Value |
|-----------|-------|
| Cloud | DigitalOcean |
| Region | BLR1 |
| Authoritative Asset BlobStore software | MinIO AIStor Free |
| Operation | AIEOS-operated |
| Topology | single dedicated AIStor node |
| Compute baseline | `s-2vcpu-4gb` |
| Storage | 6 dedicated DigitalOcean Volumes |
| Nominal Volume size | approximately 190 GiB each |
| Erasure geometry | N = 6, K = 3, M = 3, **EC:3** |

### Availability posture (explicitly accepted)

- no node-level HA
- no distributed quorum
- node outage may make primary BlobStore unavailable
- AIStor Free has no production SLA
- these limitations are explicitly accepted for Bootstrap Production

The six Volumes provide erasure protection across data devices. They do **not** create node-level high availability.

### Device / mount safety (fail closed)

Required device/mount safety remains fail closed. Do **not** permit:

- root-disk fallback
- missing-volume startup
- empty mount-directory fallback
- wrong filesystem identity
- wrong expected device count

---

## B. Commercial envelope

Frozen Bootstrap Production DigitalOcean operating envelope:

| Threshold | Rule |
|-----------|------|
| Operating target | ≤ USD 240/month |
| Hard ceiling | USD 250/month |
| Crossing USD 240/month | immediate cost/capacity review required |
| Crossing USD 250/month | new Founder commercial authorization required |

Accepted planning range that led to this decision:

approximately **USD 232.67–237.67/month**, including the known DigitalOcean estate assumptions used by the Founder gate.

That planning estimate is **not** a guaranteed invoice.

---

## C. Capacity / migration posture

Bootstrap is **demand-led** and **temporary**.

Planning target: approximately **60-day** two-tenant BASE runway to the **65% utilization** planning threshold.

Migration from Bootstrap to Scale is **telemetry-driven**.

Migration is **not**:

- arbitrary calendar-driven
- implementation discretion
- automatic because a fixed tenant count was reached unless later frozen

Crossing capacity, latency, availability, operational, or commercial thresholds may trigger a scale review.

This ADR does **not** invent new numeric thresholds beyond those already frozen here.

---

## D. Scale Production

**Scale Production** is the historical [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) distributed topology class.

Scale target:

- MinIO AIStor distributed
- 8 nodes
- 1 dedicated AIStor data device per node
- DigitalOcean BLR1
- EC:3
- historical N=8 / K=5 / M=3 topology semantics from ADR-AIEOS-040
- dedicated data-device model
- private service topology inherited from ADR-AIEOS-040
- paid distributed entitlement
- Enterprise Lite or Enterprise as applicable
- capacity sized from measured telemetry

[ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) remains canonical historical evidence and is **not** deleted.

This ADR reclassifies ADR-AIEOS-040 from:

**FIRST PRODUCTION** → **SCALE PRODUCTION**

Empirical-validation history in ADR-AIEOS-040 is retained unchanged.

---

## E. Accepted Bootstrap risk

Recorded explicitly:

1. Single-node failure can make authoritative Asset bytes temporarily unavailable.
2. Six Volumes on one node do not equal node-level HA.
3. Bootstrap has a lower availability posture than Scale Production.
4. Bootstrap acceptance is deliberate commercial/risk optimization under the Founder-approved budget.
5. Bootstrap does **not** weaken:
   - Asset PostgreSQL SoR authority
   - BlobStore create-new-only correctness
   - SHA-256 integrity
   - current-use fail-closed governance
   - authorization
   - transactional security audit
   - Asset lifecycle semantics

---

## F. Inherited provider / correctness invariants (unchanged)

From [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md), remaining active and unchanged:

- MinIO AIStor as authoritative Asset BlobStore software provider
- AIEOS-operated within DigitalOcean hosting boundary
- provider substitution closed without architecture review
- single-PUT ingest only
- multipart unauthorized
- provider-side atomic `If-None-Match: *`
- no overwrite fallback
- whole-object SHA-256 inspection
- exact byte-size inspection
- ETag not authoritative digest
- caller metadata not authoritative digest
- fail-closed required storage
- BlobStore/provider credentials ≠ AIEOS business authority

---

## G. Explicit non-authorization

ADR-AIEOS-040R1 Frozen / Approved does **not** authorize:

- creating a Droplet
- creating/resizing Volumes
- DigitalOcean resource mutation
- production credentials
- OpenTofu implementation/apply
- BlobStore production adapter
- production Asset data
- schema migration
- PED-I03 mutation activation
- production runtime composition
- production deployment
- commercial purchase
- Scale Production purchase
- production migration
- artifact promotion

Architecture freeze ≠ implementation authorization ≠ production authorization ≠ purchase authorization.

---

## Binding invariants

- Current topology classification is Bootstrap Production + Scale Production under this ADR.
- Bootstrap Production: single-node AIStor Free, BLR1, `s-2vcpu-4gb`, 6 × ~190 GiB Volumes, N=6 / K=3 / M=3, EC:3.
- Scale Production: historical ADR-AIEOS-040 8×1 distributed topology, EC:3, N=8 / K=5 / M=3.
- ADR-AIEOS-040 remains historical Frozen / Approved evidence; it is not erased.
- ADR-AIEOS-039 “must be distributed” is superseded only for Bootstrap Production.
- Provider/correctness invariants from ADR-AIEOS-039 remain active.
- Bootstrap commercial envelope: target ≤ USD 240/month; hard ceiling USD 250/month.
- Bootstrap → Scale migration is telemetry-driven, not arbitrary calendar/discretion.
- This ADR authorizes no cloud mutation, credentials, OpenTofu, adapter, PED-I03, deployment, or purchase.

## Consequences

- Catalogue current-summary surfaces must treat ADR-AIEOS-040R1 as the active topology classification.
- Infrastructure and adapter work must not treat ADR-AIEOS-040’s original First Production label as the active Bootstrap class.
- Backup/recovery remains a separate architecture decision ([ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md)); EC is not backup.
- Founder-approved Bootstrap risk and commercial envelope are explicit and cannot be silently reopened by implementation convenience.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral BlobStore invariants preserved |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | BlobStore unavailable = governance unavailable |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | DigitalOcean production cloud; BLR1 |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Historical cross-cloud exception; dormant for first production |
| [ADR-AIEOS-038R1](ADR-AIEOS-038R1-aieos-digitalocean-only-asset-storage-direction.md) | DigitalOcean-only hosting direction |
| [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) | Provider/correctness; distributed rule superseded only for Bootstrap |
| [ADR-AIEOS-040](ADR-AIEOS-040-aieos-asset-blobstore-first-production-topology.md) | Historical 8×1 topology; reclassified as Scale Production |
| [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) | Asset backup & recovery architecture |
