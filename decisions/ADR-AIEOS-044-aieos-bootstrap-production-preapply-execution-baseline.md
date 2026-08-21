---
id: ADR-AIEOS-044
title: AIEOS Bootstrap Production Pre-Apply Execution Baseline
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.1
created: 2026-08-21
last_updated: 2026-08-21
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-044 — AIEOS Bootstrap Production Pre-Apply Execution Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-21  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-039](ADR-AIEOS-039-aieos-asset-blobstore-provider-selection.md) · [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) · [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) · [ADR-AIEOS-041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) · [ADR-AIEOS-042](ADR-AIEOS-042-aieos-asset-binary-delivery-bootstrap-media-profile.md) · [ADR-AIEOS-043](ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md)

**Catalogue note:** Frozen / Approved is architecture status only.

```text
ARCHITECTURE FREEZE != PRODUCTION APPLY AUTHORIZATION.
```

This ADR freezes Bootstrap **pre-apply execution baseline** decisions required for later staged authorization. It does **not** authorize DigitalOcean resource creation, OpenTofu state initialization, OpenTofu apply, production credentials, Temporal Cloud purchase/namespace creation, DNS mutation, TLS issuance, PED-I03, or production deployment.

Evidence SHAs at deposit (read-only gate):

- Architecture `origin/main`: `5aa0bdfdba4f237f603b4c8456dfddceb6da24d2`
- Backend `origin/main`: `0040e1121f19f0b6177e87a736d32f8ccc926440`
- Infrastructure `origin/main`: `a3f76ddfc0a6b472e3f6e8dba997b2992b818e5f`

---

## Context

[ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) freezes the complete first-production component set (App Platform, Managed PostgreSQL 18, Temporal Cloud, AIEOS-operated NATS JetStream, AIStor, backup, private networking). Preflight evidence (2026-08-21) closed many implementation-level EDRs but showed that the **complete** first-production DigitalOcean composition, when added to the retained EduVijna DOKS estate, exceeds the Founder DigitalOcean service-charge hard ceiling.

This ADR records:

1. Founder commercial authority and the current RED commercial evidence  
2. Execution freezes that are safe to decide without authorizing billable apply  
3. Explicit commercial release conditions and non-authorizations  

---

## A. Commercial authority

Founder decision (exact):

| Item | Value |
| --- | --- |
| DigitalOcean service-charge operating target | ≤ **USD 240/month** pre-tax |
| DigitalOcean service-charge hard ceiling | **USD 250/month** pre-tax |
| Statutory taxes including Indian GST | Tracked **separately**; do **not** consume the USD 250 service-charge ceiling |
| Steady-state DigitalOcean composition > USD 250/month | **NOT AUTHORIZED** without a new Founder commercial decision |
| Temporal Cloud and other non-DigitalOcean services | **Separate** commercial commitments; **not** included in the USD 250 DigitalOcean ceiling |

---

## B. Current commercial evidence (2026-08-21 planning)

Recorded as verified planning evidence, **not** immutable provider pricing forever:

| Line | Approx USD/month pre-tax |
| --- | --- |
| Retained DigitalOcean estate | ≈ **82.90** |
| New complete AIEOS first-production DigitalOcean requirement (conservative candidates) | ≈ **211.15** |
| Projected total | ≈ **294.05** |
| Classification | **RED** |
| Shortfall against Founder ceiling (USD 250) | ≈ **44.05** |

**CURRENT COMPLETE FIRST-PRODUCTION COMPOSITION IS NOT COMMERCIALLY AUTHORIZED.**

Architecture freeze does **not** authorize spend.

Further evidence: deleting ≈ USD 3.80 orphan Volumes plus moving App Platform from USD 40 to the unproven USD 24 candidate still leaves approximately **USD 274.25/month**. Therefore the ceiling **cannot** currently be met through those low-risk optimizations alone.

---

## C. Legacy DOKS classification

`eduvijna-cluster` is:

**RETAINED — LEGACY BUT CURRENTLY NEEDED**

Evidence indicates it currently hosts live EduVijna product workloads, including existing application/API/web/quiz/paper-grader and supporting services.

It is **not** an [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) first-production AIEOS dependency.

DOKS retirement is **not** authorized by this ADR. DOKS/LB/PVC retirement or migration requires separate Founder/product authorization and its own migration evidence.

AIEOS must **not** silently destroy or repurpose the legacy DOKS estate to satisfy the commercial ceiling.

---

## D. NATS JetStream Bootstrap topology

Freeze AIEOS-operated NATS JetStream Bootstrap topology:

| Item | Value |
| --- | --- |
| Nodes | **one** dedicated DigitalOcean node |
| Region | BLR1 |
| Candidate compute | `s-1vcpu-2gb` |
| Storage | one dedicated ≈ **50 GiB** persistent Volume |
| Network | private production VPC only |
| Co-location | **no** DOKS; **no** AIStor; **no** PostgreSQL |

**Classification:** SINGLE-NODE / NON-HA BOOTSTRAP

Explicit consequences:

- node outage means broker unavailability  
- single-node storage failure can require JetStream reconstruction/recovery  
- PostgreSQL transactional outbox remains business truth  
- NATS availability is **not** business-transaction authority  
- replay/recovery procedure is required before production readiness  
- this topology makes **no** HA/SLA claim  

A later scale/HA gate may move to a 3-node quorum.

---

## E. Production VPC

| Item | Value |
| --- | --- |
| Name | `aieos-prod-blr1` |
| Region | `blr1` |
| CIDR | `10.130.0.0/20` |

Evidence at freeze: no collision with discovered `default-blr1 10.122.0.0/20`, DOKS pod `10.123.0.0/16`, or DOKS service `10.122.32.0/19`.

**Binding:** CIDR collision **MUST** be revalidated immediately before VPC creation.

`default-blr1` remains **forbidden** for AIEOS production authority.

---

## F. OpenTofu production state

| Item | Value |
| --- | --- |
| Object store | DigitalOcean Spaces Standard |
| Bucket | `eduvijna-aieos-tofu-state-prod` |
| Location | BLR1 |
| Backend | OpenTofu S3 backend |
| Locking | `use_lockfile = true` |
| Versioning | **ON** |
| Public access | **FORBIDDEN** |
| CDN | OFF / unused |
| Credential | dedicated restricted Spaces state credential |

State credential must **not** be shared with DigitalOcean control-plane deployment, application runtime, AIStor, or backup worker.

Backend credentials remain **outside Git**.

`eduvijna-terraform-state` remains:

**LEGACY / UNVERIFIED / NOT AUTHORITATIVE FOR NEW AIEOS PRODUCTION STATE**

---

## G. AIStor backup

Preserve [ADR-AIEOS-041](ADR-AIEOS-041-aieos-asset-backup-recovery-architecture.md) / [ADR-AIEOS-041R1](ADR-AIEOS-041R1-aieos-asset-backup-execution-manifest-recovery-authority.md) and freeze execution locality:

- DigitalOcean Spaces Standard  
- region **SFO3**  
- dedicated private backup bucket  
- Versioning **ON**  

Backup remains **NON-AUTHORITATIVE**. AIStor primary bytes remain authoritative physical object storage. PostgreSQL Asset SoR remains business authority.

Literal backup bucket name may remain an execution EDR until uniqueness is verified at creation time.

---

## H. Stable AIStor service identity

Bootstrap mechanism:

```text
stable DNS hostname → private RFC1918 AIStor VPC address
```

Application configuration **MUST** reference the hostname, never the ephemeral Droplet IP. The stable hostname is the TLS server identity.

If no private authoritative DNS service is available, a publicly resolvable DNS A record containing the RFC1918 address is **ACCEPTED** for Bootstrap.

Tradeoff:

- hostname is globally discoverable  
- RFC1918 address may be visible in public DNS  
- the service remains unroutable from the public internet  
- public DNS publication is not itself service exposure  

Droplet recreation requires a governed DNS update after the new private IP is verified.

Exact literal hostname and DNS provider/zone authority remain execution EDRs before TLS issuance.

---

## I. AIStor administration

| Path | Freeze |
| --- | --- |
| Primary Bootstrap recovery/admin | DigitalOcean Recovery Console |
| Optional SSH | Allowed only through separately governed firewall configuration using an operator-supplied CIDR/value **not** committed into Git |

No developer/home IP is durable architecture authority. No normal public S3 ingress.

---

## J. TLS / private CA

Bootstrap PKI approach:

- Smallstep tooling / `step` CLI  
- AIEOS-controlled offline root CA  
- Issuance intermediate separated from root authority  

Root private key, intermediate/private issuance authority, and AIStor server private key: **outside Git** and **outside OpenTofu state**; root not injected into runtime.

Runtime trust: AIEOS CA bundle supplied through encrypted App Platform configuration.

Server certificate must cover the stable AIStor hostname.

Forbidden: `verify=false`, plaintext fallback, private keys in `.tfvars`, private keys in cloud-init committed to Git.

---

## K. AIStor Bootstrap (preserved)

Preserve existing frozen topology ([ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) / [ADR-AIEOS-043](ADR-AIEOS-043-aieos-bootstrap-aistor-service-boundary-primary-namespace.md)):

| Item | Value |
| --- | --- |
| Node | `aieos-prod-aistor-01` |
| Compute | `s-2vcpu-4gb` |
| Storage | six **new** 190 GiB dedicated Volumes |
| Filesystem | XFS |
| Mount identity | filesystem UUID |
| Mounts | `/srv/aistor/data01` … `/srv/aistor/data06` |
| Geometry | N=6 / K=3 / M=3 / EC:3 |

No downsize is authorized merely to meet the commercial ceiling.

---

## L. Managed PostgreSQL

Bootstrap planning candidate:

| Item | Value |
| --- | --- |
| Engine | DigitalOcean Managed PostgreSQL **18** |
| Region | BLR1 |
| Plan | `db-s-1vcpu-1gb` |
| Nodes | **1** |
| Classification | single-node Bootstrap / **no HA standby claim** |

Required: private normal runtime connectivity; managed backup/PITR class; separate runtime / migrator / schema-owner / backup authorities.

This freezes the current Bootstrap candidate for pre-apply planning. Actual availability and price **MUST** be revalidated immediately before creation.

---

## M. App Platform

Do **not** freeze two-component sizing as final production implementation yet.

Current conservative commercial candidate (planning evidence only):

- **2 × `basic-s`** ≈ USD **40**/month total  
- Intended minimum roles: API component; worker component  

Governed backend evidence shows no completed production main entrypoints for all required execution classes. Therefore:

**FINAL APP PLATFORM COMPONENT TOPOLOGY AND SIZING = IMPLEMENTATION-GATED**

Required production functions include HTTP API, Temporal worker, NATS/outbox dispatcher, and scheduled/reconciliation work. Final component count may freeze only after production entrypoints and memory/runtime evidence exist.

Do not downsize merely to fit budget.

---

## N. Temporal Cloud

Preserve [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md): Temporal Cloud is required.

| Item | Status |
| --- | --- |
| Account/namespace existence | not evidenced |
| Commercial planning evidence | Essentials class ≈ ≥ USD **100**/month minimum commitment |
| Classification | **NON-DIGITALOCEAN COMMERCIAL COMMITMENT — SEPARATE FOUNDER VISIBILITY** |

This ADR does **not** authorize Temporal Cloud purchase or namespace creation.

---

## O. Commercial release condition

No billable first-production DigitalOcean compute/stateful resource apply may advance while the projected steady-state DigitalOcean estate exceeds **USD 250/month pre-tax**.

Commercial release requires one of:

**A.** evidence-backed reduction of retained/new estate to ≤ USD 250/month  

**OR**

**B.** a new Founder-approved higher DigitalOcean ceiling  

Legacy production workloads may **not** be destroyed merely to satisfy this condition.

---

## P. Non-billable / negligible-cost preparation (not authorized by this ADR)

The following **MAY** later be separately authorized before full commercial release because they do not themselves constitute the blocked billable compute estate:

- production state bucket creation under an already-paid Spaces subscription  
- state-bucket Versioning  
- restricted state credential creation  
- production remote-state initialization verification  
- production VPC creation if current provider pricing remains zero  
- code/documentation hardening  

This ADR does **not** itself execute or authorize those mutations. Each still requires an explicit execution authorization.

---

## Q. Implementation gaps (open)

- production API main/entrypoint confirmation  
- production Temporal worker main  
- production NATS/outbox dispatcher main  
- production reconciliation/scheduled execution main  
- App Platform spec/composition  
- Managed PostgreSQL runtime composition  
- NATS runtime/bootstrap automation  
- Temporal Cloud runtime config/composition  
- AIStor mount/bootstrap automation  
- stable DNS automation  
- TLS issuance/install runbook  
- remote-state bootstrap operational execution  

Architecture gap does **not** grant implementation freedom.

---

## R. GitHub supply-chain hardening (before CI production authority)

Infrastructure workflow action references must be immutable commit SHA pins.

Verified candidates from preflight:

| Action | Tag | Commit SHA |
| --- | --- | --- |
| `actions/checkout` | v4.2.2 | `11bd71901bbe5b1630ceea73d27597364c9af683` |
| `opentofu/setup-opentofu` | v2.0.2 | `a1320f892987e89d278cc92dc5adc984fb93aca4` |

Checkout must use `persist-credentials: false`.

### Corrigendum — setup-opentofu tag mapping

ADR-AIEOS-044 v1.0.0 incorrectly labeled immutable SHA `a1320f892987e89d278cc92dc5adc984fb93aca4` as setup-opentofu **v1.0.8**.

Upstream verification established:

- `v1.0.8` = `9d84900f3238fab8cd84ce47d658d25dd008be2f`
- `v2.0.2` = `a1320f892987e89d278cc92dc5adc984fb93aca4`

The immutable SHA selected by the original architecture deposit was correct; only its human-readable tag/version label was wrong.

Chief Architect binding correction:

`setup-opentofu` **v2.0.2** @ `a1320f892987e89d278cc92dc5adc984fb93aca4`

No other ADR-AIEOS-044 decision is changed.

This is a repository-hardening requirement, **not** cloud authorization.

---

## Explicit non-authorizations

ADR-AIEOS-044 does **NOT** authorize:

- DOKS retirement  
- legacy workload migration  
- orphan Volume deletion  
- DigitalOcean ceiling increase  
- Temporal Cloud purchase  
- Temporal namespace creation  
- AIStor Droplet creation  
- AIStor Volume creation  
- NATS node creation  
- Managed PostgreSQL creation  
- App Platform creation  
- state bucket creation  
- state key creation  
- remote state init  
- VPC creation  
- DNS mutation  
- TLS issuance  
- production credentials  
- `tofu plan` against live production  
- `tofu apply`  
- PED-I03  
- production application deployment  
- production mutation  

---

## Consequences

- Pre-apply execution EDRs that were blocking coherent staging are now frozen as architecture.  
- Complete first-production DigitalOcean apply remains commercially blocked until release condition O is satisfied.  
- Negligible-cost preparation items remain possible only under **separate** explicit execution authorizations.  
- Backend/infrastructure implementation must close recorded gaps before App Platform topology can freeze.  
