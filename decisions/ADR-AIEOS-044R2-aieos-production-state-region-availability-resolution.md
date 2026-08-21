---
id: ADR-AIEOS-044R2
title: AIEOS Production State Region Availability Resolution
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-21
last_updated: 2026-08-21
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-044R2 — AIEOS Production State Region Availability Resolution

**Status:** Frozen / Approved  
**Date:** 2026-08-21  
**Related:** [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) · [ADR-AIEOS-041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) · [ADR-AIEOS-044](ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) · [ADR-AIEOS-044R1](ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md)

**Catalogue note:** Frozen / Approved is architecture status. This ADR is a **forward architecture revision** of the OpenTofu production-state location after [ADR-AIEOS-044R1](ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md). It does **not** delete, rewrite, or mark ADR-AIEOS-044 or ADR-AIEOS-044R1 rejected. Those remain historical authority. This ADR supersedes **only** the forward production-state **region**, **literal bucket identity**, and **regional endpoint**. It does **not** authorize cloud mutation, Spaces key/bucket creation, infrastructure or backend repository change, OpenTofu init/plan/apply, or Stage 1 resumption by itself.

---

## Context

[ADR-AIEOS-044](ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) froze OpenTofu production state on DigitalOcean Spaces Standard. [ADR-AIEOS-044R1](ADR-AIEOS-044R1-aieos-production-state-namespace-collision-resolution.md) superseded the literal identity to:

| Item | ADR-AIEOS-044R1 forward value |
| --- | --- |
| Bucket | `eduvijna-aieos-tofu-state-prod-blr1` |
| Location | BLR1 |

Stage 1 execution then determined that **new Spaces creation in BLR1 is unavailable** for the authorized EduVijna DigitalOcean account, while SFO3 remains available.

---

## DigitalOcean BLR1 creation-unavailable evidence

Provider evidence (authenticated DigitalOcean Control Panel):

- Spaces Standard subscription is **retained**
- **SFO3** is available for Spaces bucket creation
- **BLR1** is displayed but **disabled**
- provider UI states: **"Creates in this datacenter region are disabled at this time"**
- no BLR1 state bucket was created
- no permanent AIEOS state credential exists
- no OpenTofu production remote-state initialization occurred

Additionally, two prior S3-compatible `CreateBucket` attempts using newly-created DigitalOcean Spaces `fullaccess` credentials returned **AccessDenied**. Those failures **did not** create durable resources and must **not** be treated alone as proven global namespace collision for the BLR1 target.

---

## Decision

### A. SFO3 state-region exception

The AIEOS production OpenTofu remote-state object store moves from **BLR1** to **SFO3** because DigitalOcean currently disables new Spaces creation in BLR1 for the authorized account.

This is a **CONTROL-PLANE STATE LOCATION** exception.

It does **NOT** change the AIEOS primary production **workload** region.

### B. Unchanged runtime BLR1 region

AIEOS first-production **workload** region remains **BLR1**, including (unless separately revised):

- production VPC
- AIStor
- NATS
- Managed PostgreSQL
- App Platform
- other BLR1 production compute / stateful workload resources

### C. New production state identity

Supersede the forward intended production-state bucket:

| | Value |
| --- | --- |
| **Previous (044R1)** | `eduvijna-aieos-tofu-state-prod-blr1` in BLR1 |
| **New (authoritative planned)** | `eduvijna-aieos-tofu-state-prod-sfo3` |
| **Region** | SFO3 |
| **Endpoint** | `https://sfo3.digitaloceanspaces.com` |

Do **not** retain a `-blr1` bucket suffix for an SFO3 resource.

### D. State security invariants (unchanged)

| Item | Value |
| --- | --- |
| Service | DigitalOcean Spaces Standard |
| Backend | OpenTofu S3 backend |
| Locking | `use_lockfile = true` |
| Versioning | **ON** |
| Public access | **FORBIDDEN** |
| CDN | OFF / unused |
| State credential | dedicated bucket-scoped restricted Spaces credential |
| Permanent permission | `readwrite` |
| Credential separation | must not be shared with application runtime, AIStor, backup worker, or control-plane deployment identity |
| Credentials | outside Git |

### E. Cross-region control-plane posture

AIEOS **runtime production** remains **BLR1** while OpenTofu **control-plane state** is stored in **SFO3**.

This is acceptable because the production state bucket is:

- infrastructure control-plane state
- not application runtime storage
- not on the learner/teacher request path
- not an AIStor primary-object dependency
- accessed during governed infrastructure operations only

SFO3 loss must **not** directly stop normal AIEOS application request serving. Infrastructure mutation may be blocked while remote state is unavailable — acceptable **fail-closed** behavior.

### F. Backup region relationship

[ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) / [ADR-AIEOS-041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) use SFO3 for backup/recovery object storage.

Do **not** merge the production OpenTofu state bucket with any backup bucket. The state bucket must remain a dedicated object-storage authority with:

- separate literal bucket
- separate credentials
- separate object namespace
- separate lifecycle/security governance

Co-location in SFO3 does **not** imply shared credentials or shared bucket.

### G. Previous BLR1 state name

`eduvijna-aieos-tofu-state-prod-blr1` =

**PLANNED / NEVER CREATED / SUPERSEDED BY ADR-AIEOS-044R2**

It is **not** an existing cloud resource. No deletion is required.

### H. NYC3 HOLD — unchanged

`eduvijna-aieos-tofu-state-prod` (NYC3 / `first-project`) remains:

**UNATTRIBUTED / PRE-EXISTING / NON-AUTHORITATIVE / HOLD**

No authority to adopt, delete, reassign, version, configure, or use as state.

### I. Legacy state — unchanged

`eduvijna-terraform-state` remains:

**LEGACY / UNVERIFIED / NOT AUTHORITATIVE FOR NEW AIEOS PRODUCTION STATE**

### J. Creation-time collision rule

Target `eduvijna-aieos-tofu-state-prod-sfo3` must be checked immediately before eventual creation.

If already present or unavailable: **STOP.**

Do **not**:

- add numeric suffixes
- add timestamps
- generate random names
- switch to NYC3
- switch back to BLR1
- adopt another bucket

Return to Chief Architect.

### K. Stage 1 status

**STAGE 1 PRODUCTION-STATE BOOTSTRAP IS SUSPENDED PENDING ADR-AIEOS-044R2 DEPOSIT AND SOURCE RECONCILIATION.**

Stage 1 may resume **only** after:

1. ADR-AIEOS-044R2 is merged and post-merge verified;
2. infrastructure references are reconciled from `eduvijna-aieos-tofu-state-prod-blr1` / BLR1 to `eduvijna-aieos-tofu-state-prod-sfo3` / SFO3, and that infrastructure PR is merged and post-merge verified;
3. Chief Architect issues a **fresh** exact-source Stage 1 **SFO3** execution authorization.

**ADR-AIEOS-044R2 itself grants NO cloud mutation authority and does NOT resume Stage 1.**

---

## Commercial boundary

Unchanged:

- retained Spaces Standard subscription already exists
- expected incremental state-bucket base subscription charge approximately USD 0, subject to creation-time verification
- Founder DigitalOcean hard ceiling = **USD 250/month** pre-tax
- full AIEOS production compute composition remains commercially blocked
- no billable production compute is authorized by this ADR

---

## Explicit non-authorizations

This ADR does **NOT** authorize:

- DigitalOcean mutation of any kind
- Spaces bucket or credential creation (SFO3, BLR1, or otherwise)
- NYC3 HOLD or legacy bucket mutation
- infrastructure or backend repository change
- `tofu init` / `plan` / `apply` / `destroy`
- VPC, AIStor, NATS, PostgreSQL, App Platform, Temporal Cloud, DNS, TLS, backup bucket, DOKS change, or deployment
- Stage 1 cloud execution by deposit alone

---

## Consequences

- Forward catalogues and future Stage 1 authorizations must use `eduvijna-aieos-tofu-state-prod-sfo3` in **SFO3** as the planned AIEOS production OpenTofu state bucket.
- Production **workload** region remains **BLR1**.
- ADR-AIEOS-044 / 044R1 remain historical; 044R2 supersedes only forward state region, literal, and endpoint.
- Stage 1 remains suspended until the release conditions above are met under a fresh exact-source SFO3 authorization.
- Backup SFO3 Spaces and OpenTofu state SFO3 Spaces remain separate authorities.
