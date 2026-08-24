---
id: ADR-AIEOS-048R1
title: AIEOS App Platform Provider-Compliant Naming Revision
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-24
last_updated: 2026-08-24
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-048R1 — AIEOS App Platform Provider-Compliant Naming Revision

**Status:** Frozen / Approved  
**Date:** 2026-08-24  
**Related:** [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR is a **NARROW FORWARD REVISION** of [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md). It corrects only the App Platform application naming contract after implementation review found the ADR-AIEOS-048 names incompatible with the provider's application/component naming constraint. **All non-naming ADR-AIEOS-048 decisions remain binding.** Unchanged authority flows through the base ADR. Do **not** rewrite the ADR-AIEOS-048 historical body.

**ID family note:** `ADR-AIEOS-048R1` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS [ADR-048](ADR-048-review-queue-owns-approval.md).

---

## Context

[ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) froze first-production App Platform worker topology, dedicated BLR1 VPC, Preview compute exception, OCI digest authority, and Temporal secret destinations.

Implementation review of Infrastructure WPI-AP-I01 found the previously frozen application names exceeded the DigitalOcean App Platform naming constraint. This revision records the provider-compliant names as current architecture authority without changing any other ADR-AIEOS-048 contract.

Blocked Infrastructure implementation (Architecture record only; this ADR does **not** modify Infrastructure):

```text
WPI-AP-I01
Infrastructure PR #12
reviewed blocked head = 0d9f4560b4b2b0b79cdf5d0512f67d92841541e7
classification         = BLOCKED — ADR-AIEOS-048 PROVIDER NAME-CONSTRAINT CONTRACT FAILURE
```

Infrastructure PR #12 **MUST** remain unmerged until this ADR is merged and PR #12 is subsequently reconciled to the new naming authority and re-reviewed.

---

## Decision

### Naming supersession (this revision only)

| Application | ADR-AIEOS-048 name (SUPERSEDED / historical) | ADR-AIEOS-048R1 name (CURRENT / FROZEN) | Length |
|-------------|---------------------------------------------|-----------------------------------------|--------|
| WORKFLOW_DISPATCHER | `eduvijna-aieos-prod-workflow-dispatcher` | `aieos-prod-workflow-dispatcher` | 30 |
| TEMPORAL_WORKER | `eduvijna-aieos-prod-temporal-worker` | `aieos-prod-temporal-worker` | 26 |

Both CURRENT names are provider-compliant under the validated naming contract.

The superseded `eduvijna-aieos-prod-*` values remain historical record only. They **MUST NOT** be used for new App Platform creation.

No other ADR-AIEOS-048 value is superseded.

### Component naming

The two applications remain exactly:

- `aieos-prod-workflow-dispatcher`
- `aieos-prod-temporal-worker`

For first-production source implementation, using the same provider-compliant string as the single worker component name is **PERMITTED**. This does **not** merge the two apps or their lifecycle boundaries.

If implementation later chooses a shorter component-local name, it must remain unambiguous and must not alter application identity or architecture topology. This ADR does **not** create a new requirement merely to force separate worker component names.

### Unchanged ADR-AIEOS-048 authority

The following remain binding exactly as frozen by ADR-AIEOS-048:

```text
production App Platform region = blr
DigitalOcean datacenter         = blr1
dedicated production VPC        = aieos-prod-blr1
CIDR                            = 10.130.0.0/20
default-blr1 reuse              = FORBIDDEN
application topology            = TWO separate App Platform applications
component type                  = worker
WORKFLOW_DISPATCHER run command = python -m aieos.platform.runtime.entrypoints.workflow_dispatcher_main
TEMPORAL_WORKER run command     = python -m aieos.platform.runtime.entrypoints.temporal_worker_main
instance size                   = apps-s-1vcpu-1gb-fixed
instance count                  = 1 each
Preview acceptance              = unchanged / bounded first-production exception
registry                        = eduvijna-registry
repository                      = aieos-backend
image                           = ONE common immutable Backend OCI digest
mutable tags / latest authority = FORBIDDEN
deploy_on_push                  = false
```

Also unchanged:

- Temporal workload credential separation
- OpenTofu secret-state prohibition
- independent Infrastructure activation boundaries
- USD 240 operating target
- USD 250 hard service ceiling
- production deployment authorization = **NOT AUTHORIZED**

ADR-AIEOS-048 invariants A48-INV-01 through A48-INV-15 remain in force.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A48R1-INV-01 | The first-production WORKFLOW_DISPATCHER App Platform application name is `aieos-prod-workflow-dispatcher`. |
| A48R1-INV-02 | The first-production TEMPORAL_WORKER App Platform application name is `aieos-prod-temporal-worker`. |
| A48R1-INV-03 | The superseded `eduvijna-aieos-prod-*` application names MUST NOT be used for new App Platform creation. |
| A48R1-INV-04 | This revision changes naming only; two-app isolation, workload identity, network, compute, OCI, secret, and authorization contracts remain unchanged. |

---

## Consequences

### Positive

- Current application names are provider-compliant and can be implemented without violating DigitalOcean App Platform naming constraints
- Historical ADR-AIEOS-048 names remain auditable as superseded values
- All non-naming first-production runtime contracts remain frozen

### Negative / residual risk

- Infrastructure WPI-AP-I01 / PR #12 remains blocked until this ADR is merged and that PR is reconciled to the CURRENT names
- Dedicated VPC, App Platform applications, governed OCI digest, and runtime secrets still do not exist

### Explicit non-authorizations

This ADR is architecture authority only. It does **not** authorize:

- Infrastructure PR #12 merge
- DigitalOcean resource mutation
- VPC creation
- App Platform application creation/update
- DOCR repository/image publication
- OCI promotion
- Temporal API-key issuance/revocation
- runtime secret injection
- OpenTofu production plan
- OpenTofu production apply
- production state mutation
- production workflow execution
- production deployment
- commercial release

---

## Status

**Frozen / Approved** — architecture source authority only. Current App Platform **naming** authority. ADR-AIEOS-048 remains the historical/base first-production App runtime contract for all non-naming decisions.
