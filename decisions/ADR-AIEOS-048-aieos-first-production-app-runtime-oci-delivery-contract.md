---
id: ADR-AIEOS-048
title: AIEOS First-Production App Runtime & OCI Delivery Contract
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-23
last_updated: 2026-08-23
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-048 — AIEOS First-Production App Runtime & OCI Delivery Contract

**Status:** Frozen / Approved  
**Date:** 2026-08-23  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-040R1](ADR-AIEOS-040R1-aieos-asset-blobstore-bootstrap-scale-production-topology.md) · [ADR-AIEOS-044R2](ADR-AIEOS-044R2-aieos-production-state-region-availability-resolution.md) · [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) · [ADR-AIEOS-046](ADR-AIEOS-046-aieos-production-event-plane-identity-least-privilege-contract.md) · [ADR-AIEOS-047](ADR-AIEOS-047-aieos-production-workflow-plane-identity-least-privilege-contract.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. It does **not** authorize:

- DigitalOcean resource creation/mutation
- App Platform application creation/update
- VPC creation/update
- registry repository creation
- registry subscription change
- OCI build publication
- OCI promotion
- Temporal API-key creation/revocation
- runtime secret injection
- OpenTofu plan/apply
- production DB access/migration
- production workflow execution
- production deployment
- commercial release

**ID family note:** `ADR-AIEOS-048` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS [ADR-048](ADR-048-review-queue-owns-approval.md) (Review Queue owns approval). Do not rename or conflate these decisions.

Chief-facing alignment contract approved on **2026-08-23**.

Evidence SHAs at deposit (read-only gate):

- Architecture `origin/main`: `5dec3214ddf170ac7e07096b8eca1d2aad2b9109`
- Infrastructure `origin/main`: `2a6782889fb901c37fc57ec8760761d4abc8a6c7`
- Backend `origin/main`: `8f4dd172e6a0ba8b4ad944b0ae22060442356342`

Binding prior authority (bodies not rewritten by this ADR): ADR-AIEOS-022, ADR-AIEOS-026, ADR-AIEOS-029, ADR-AIEOS-037, ADR-AIEOS-040R1 (BLR1 production topology relevance), ADR-AIEOS-044R2, ADR-AIEOS-045, ADR-AIEOS-046, ADR-AIEOS-047.

---

## Context

[ADR-AIEOS-047](ADR-AIEOS-047-aieos-production-workflow-plane-identity-least-privilege-contract.md) freezes Temporal Cloud identity separation for WORKFLOW_DISPATCHER and TEMPORAL_WORKER, including distinct Temporal API-key environment families and least-privilege RBAC.

First-production **runtime hosting**, **network locality**, **OCI delivery**, and **App Platform application topology** for those long-running non-HTTP workloads remain open and must not be improvised from legacy registry tags, default VPC reuse, or Preview SKUs without an explicit architecture freeze.

Live discovery (architecture evidence only; not mutation) established:

- App Platform region `blr` maps to datacenter `blr1`
- No App Platform applications currently host the governed executables
- Candidate dedicated VPC CIDR `10.130.0.0/20` has zero overlap with existing default-blr1 / DOKS networks
- Live provider size catalogue has no non-preview, non-deprecated ≥1 GiB ≤ USD 15 worker SKU; Preview `apps-s-1vcpu-1gb-fixed` is the only bounded first-production path accepted by Founder / Chief Architecture for this slice

Production App Platform creation, VPC creation, OCI publication, and deployment remain unauthorized by this ADR.

---

## Decision

### Network topology

Freeze:

```text
production App Platform region = blr
DigitalOcean datacenter         = blr1
production VPC                  = NEW dedicated VPC
name                            = aieos-prod-blr1
CIDR                            = 10.130.0.0/20
VPC required                    = true
dedicated egress                = false
```

Collision validation evidence (read-only; 2026-08-23):

| Network | CIDR |
|---------|------|
| Candidate | `10.130.0.0/20` |
| Existing `default-blr1` | `10.122.0.0/20` |
| Existing DOKS cluster subnet | `10.123.0.0/16` |
| Existing DOKS service subnet | `10.122.32.0/19` |

```text
overlap count = 0
```

`10.130.0.0/20` ≠ `10.122.0.0/20`, and neither contains the other.

**Forbidden:**

- reuse `default-blr1` as the AIEOS production VPC
- silently choose another CIDR
- move workload region without a later architecture revision

### Application topology

Freeze **two separate** App Platform applications.

**Application 1**

```text
name           = eduvijna-aieos-prod-workflow-dispatcher
component type = worker
executable     = python -m aieos.platform.runtime.entrypoints.workflow_dispatcher_main
instance count = 1
```

**Application 2**

```text
name           = eduvijna-aieos-prod-temporal-worker
component type = worker
executable     = python -m aieos.platform.runtime.entrypoints.temporal_worker_main
instance count = 1
```

Freeze:

- independent application lifecycle
- independent secret boundary
- independent restart/redeployment
- no public HTTP routing requirement for either workload
- no coupling of dispatcher and worker deployment lifecycle

Do **not** add API runtime or EVENT dispatcher to this ADR’s first deployment slice.

### Preview compute risk acceptance

Freeze first-production size:

```text
apps-s-1vcpu-1gb-fixed
```

Current validated provider metadata:

| Field | Value |
|-------|-------|
| CPU | 1 SHARED |
| Memory | 1 GiB |
| Price evidence | USD 10/month per instance |
| `feature_preview` | true |
| `single_instance_only` | true |
| bandwidth | 100 GiB |

**Founder / Chief Architecture accepted the provider Preview status for this FIRST-PRODUCTION workload slice on 2026-08-23** because live provider discovery showed no non-preview, non-deprecated ≥1 GiB ≤ USD 15 worker SKU.

This is a **bounded exception**.

It does **not** authorize:

- arbitrary Preview products
- future Preview SKU adoption
- horizontal-scaling assumptions
- automatic migration to another SKU

If the SKU becomes unavailable, materially changes, or is deprecated before deployment:

```text
FAIL CLOSED — ARCHITECTURE RECONCILIATION REQUIRED
```

### OCI / registry contract

Freeze adoption of the **existing** registry:

```text
eduvijna-registry
```

Do **not** freeze or claim its subscription tier unless independently proven.

Freeze a **new** logical AIEOS repository:

```text
aieos-backend
```

Production runtime artifact:

```text
ONE common Backend OCI image
```

Both WORKFLOW_DISPATCHER and TEMPORAL_WORKER use the **same immutable image digest** but distinct run commands and distinct workload configuration.

Production artifact authority requires **all** of:

- Backend exact governed Git SHA
- successful governed CI
- production OCI build provenance
- immutable manifest digest
- explicit production OCI publication/promotion gate

**Forbidden production authority:**

- `latest`
- mutable tags
- repository name alone
- legacy `eduvijna-api`
- legacy `eduvijna-web`
- current NON_PRODUCTION OCI runtime-probe image
- unverified local image
- tag-only identity

`latest` may exist elsewhere in the registry but **MUST NOT** become AIEOS production deployment authority.

```text
deploy_on_push = false
```

### Secret delivery contract

Preserve [ADR-AIEOS-047](ADR-AIEOS-047-aieos-production-workflow-plane-identity-least-privilege-contract.md) identity separation.

| Workload | Temporal API-key destination |
|----------|------------------------------|
| WORKFLOW_DISPATCHER | `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY` |
| TEMPORAL_WORKER | `AIEOS_TEMPORAL_API_KEY` |

Future App Platform destination for each:

```text
scope             = RUN_TIME
type              = SECRET
component-specific = true
```

**Forbidden:**

- app-level Temporal API key
- dispatcher key visible to worker
- worker key visible to dispatcher
- shared Temporal key
- GENERAL plaintext type
- BUILD_TIME-only secret
- secret value in Git
- secret value in ADR
- secret value in OpenTofu source
- secret value in `.tfvars`
- secret value in OpenTofu state
- secret value in CI logs
- secret value in runtime logs

This ADR does **not** decide a secret-bearing OpenTofu mechanism. Exact live provisioning mechanism remains a later implementation/provisioning design, but **MUST** preserve the no-secret-in-OpenTofu-state invariant.

### Runtime credential baseline (WPI-K01 operating specialization)

Record the already approved WPI-K01 operating specialization as operational timing that **supplements** ADR-AIEOS-047 and does **not** weaken its identity/RBAC model:

- separate dispatcher and worker API keys
- ≤90-day key lifetime
- rotation begins at T-30
- normal active keys = 1 per identity
- maximum 2 during bounded rotation overlap
- independent rotation/revocation

### Commercial posture

Validated scenario (architecture/commercial evidence only; not purchase authorization):

| Item | USD / month |
|------|-------------|
| Existing source-modeled DigitalOcean subtotal | 217.90 |
| Two first-production workers (current provider evidence) | 20.00 |
| Modeled subtotal | 237.90 |
| Operating target | 240.00 |
| Margin to target | 2.10 |
| Hard DigitalOcean service ceiling | 250.00 |
| Margin to ceiling | 12.10 |

Existing registry marginal cost: **UNPROVEN / DO NOT DOUBLE COUNT**.

Sensitivity if +USD 5 registry cost were incremental: **USD 242.90/month**.

- Taxes/GST remain tracked separately under existing authority
- Provider price movement before live deployment requires cost revalidation
- Hard ceiling must remain fail-closed

### Infrastructure activation boundary

The existing broad `enable_cloud_resources` coupling **MUST NOT** be used to obtain only the production VPC/App runtime if doing so implicitly enables unrelated AIStor resources.

Future Infrastructure source **MUST** provide fail-closed independent activation boundaries for at least:

- production VPC
- AIStor resources
- App Platform WORKFLOW_DISPATCHER
- App Platform TEMPORAL_WORKER

Exact Terraform/OpenTofu variable names are implementation design and are not frozen by this ADR.

**Invariant:** authorizing one workload/resource slice **MUST NOT** implicitly authorize another.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A48-INV-01 | First-production long-running workflow workloads use DigitalOcean App Platform worker components in `blr`. |
| A48-INV-02 | Production uses dedicated `aieos-prod-blr1` / `10.130.0.0/20`; `default-blr1` reuse is forbidden. |
| A48-INV-03 | WORKFLOW_DISPATCHER and TEMPORAL_WORKER are separate App Platform applications. |
| A48-INV-04 | Each first-production workload runs exactly one `apps-s-1vcpu-1gb-fixed` instance unless later governed revision changes it. |
| A48-INV-05 | Preview SKU acceptance is bounded to this first-production slice. |
| A48-INV-06 | Both workloads use one common immutable Backend OCI image digest. |
| A48-INV-07 | Mutable tags including `latest` are not production artifact authority. |
| A48-INV-08 | Production image comes from `eduvijna-registry/aieos-backend` under a governed OCI publication/promotion gate. |
| A48-INV-09 | Deploy-on-push is false. |
| A48-INV-10 | Temporal runtime API-key destinations are workload-specific RUN_TIME SECRET variables. |
| A48-INV-11 | Dispatcher and worker Temporal credentials are never shared. |
| A48-INV-12 | Temporal API-key values never enter OpenTofu state. |
| A48-INV-13 | App/VPC/AIStor activation boundaries are independently authorized. |
| A48-INV-14 | Production deployment requires immutable OCI provenance + digest + explicit deployment authorization. |
| A48-INV-15 | Provider/cost drift that breaks the frozen Preview SKU or USD 250 service ceiling fails closed pending architecture review. |

---

## Consequences

### Positive

- App runtime topology, network locality, OCI identity, and secret destinations are frozen before Infrastructure or deployment improvisation
- Dispatcher/worker secret and lifecycle boundaries remain aligned with ADR-AIEOS-047
- Preview compute risk is explicit and bounded rather than implicit

### Negative / residual risk

- Preview SKU may change or disappear before deployment (fail-closed)
- Dedicated VPC and App Platform apps do not yet exist
- Governed production OCI image for `aieos-backend` does not yet exist
- Commercial envelope remains tight; registry marginal cost is unproven

### Explicit non-authorizations

This ADR does **not** authorize DigitalOcean mutation, App Platform creation/update/deployment, VPC creation, DOCR repository/image publication, registry subscription change, Temporal API-key creation/revocation, runtime secret injection, OpenTofu plan/apply, production DB access/migration, production workflow execution, production deployment, or commercial release.

---

## Status

**Frozen / Approved** — architecture source authority only.
