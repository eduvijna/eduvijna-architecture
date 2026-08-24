---
id: ADR-AIEOS-048R2
title: AIEOS App Platform Runtime Ownership Boundary Revision
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-24
last_updated: 2026-08-24
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-048R2 — AIEOS App Platform Runtime Ownership Boundary Revision

**Status:** Frozen / Approved  
**Date:** 2026-08-24  
**Related:** [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) · [ADR-AIEOS-048R1](ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. [ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md) is a **NARROW FORWARD REVISION** of [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) and [ADR-AIEOS-048R1](ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md). It supersedes **ONLY** the production DigitalOcean App Platform **resource-ownership** mechanism. All topology, naming, network, OCI, workload-isolation, commercial, and secret-separation decisions not explicitly changed by R2 remain binding. Do **not** rewrite the ADR-AIEOS-048 or ADR-AIEOS-048R1 historical bodies.

**ID family note:** `ADR-AIEOS-048R2` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS [ADR-048](ADR-048-review-queue-owns-approval.md).

---

## Context

[ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) froze first-production App Platform worker topology, dedicated BLR1 VPC, Preview compute exception, OCI digest authority, and Temporal secret destinations.

[ADR-AIEOS-048R1](ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md) froze provider-compliant application names without changing any other ADR-AIEOS-048 contract.

Infrastructure source subsequently modeled production `digitalocean_app` resources under a zero-env OpenTofu posture with out-of-band runtime SECRET injection assumed. Empiric provider validation was required before production App Platform plan/apply could be considered.

### Empirical provider evidence (WPI-AP-SV01 / WPI-AP-SV01R1)

```text
OpenTofu                  = 1.12.5
DigitalOcean provider     = 2.99.1
doctl                     = 1.146.0-release
Environment               = isolated disposable DigitalOcean Development project
Disposable App            = one worker; BLR; public BusyBox image; dummy secret only
OpenTofu configuration    = ZERO env blocks
Out-of-band injected env  = SV01_DUMMY_SECRET / type=SECRET / scope=RUN_TIME
Remote representation     = encrypted / opaque nonempty
```

Decisive observation:

A saved OpenTofu **refresh-only** plan, while HCL still contained **ZERO** env blocks, materialized the out-of-band secret into plan JSON as an encrypted `EV[...]` representation.

The refresh-only plan was **NOT** applied. No production state, resources, or secrets were accessed. The disposable App and project were successfully deleted.

```text
Primary classification = B. FAIL_OPEN_TOFU_SECRET_MATERIAL
WPI-AP-SV01 / WPI-AP-SV01R1 = FORMALLY CLOSED — FAIL_OPEN_TOFU_SECRET_MATERIAL
```

Dummy secret plaintext is not recorded in this ADR.

---

## Architecture finding

DigitalOcean provider **2.99.1** does **not** provide an acceptable secret-isolation boundary for AIEOS when OpenTofu owns a `digitalocean_app` resource while runtime SECRET environment values are injected out-of-band.

Provider refresh/plan can materialize encrypted secret material into OpenTofu representations even when the OpenTofu configuration declares no env blocks.

Under AIEOS security architecture:

| Material in infrastructure plan/state | Status |
|---------------------------------------|--------|
| Plaintext secret material | **FORBIDDEN** |
| Encrypted provider secret material such as `EV[...]` | **FORBIDDEN** |
| Opaque nonempty provider secret representations | **FORBIDDEN** |

Encryption-at-rest of an OpenTofu backend does **not** change this rule.

---

## Decision

### Ownership supersession (this revision only)

The following production model is **SUPERSEDED / FORBIDDEN**:

```text
OpenTofu owns digitalocean_app
+
runtime secrets are injected outside OpenTofu
```

This remains forbidden even if:

- OpenTofu HCL contains zero env blocks
- env changes use `lifecycle ignore_changes`
- attributes are marked `sensitive`
- state backend is encrypted
- dispatcher and worker use separate state files
- secret values are encrypted by DigitalOcean
- secret injection occurs only after initial App creation

None of those measures satisfies the AIEOS **no-secret-material-in-infrastructure-state** invariant.

### Current ownership boundary

**OpenTofu MAY** continue to own appropriate non-App infrastructure including (as separately authorized):

- production project association/discovery mechanisms
- dedicated production VPC
- VPC CIDR/region
- AIStor infrastructure
- Temporal Cloud provisioning resources already governed separately
- other infrastructure whose provider state does not violate secret contracts

**OpenTofu MUST NOT** own in production:

- `digitalocean_app` resources for the AIEOS WORKFLOW_DISPATCHER
- `digitalocean_app` resources for the AIEOS TEMPORAL_WORKER
- App Platform runtime environment variables
- App Platform SECRET values
- provider representations of App Platform SECRET values
- any secret-bearing App Platform deployment state

Production App Platform lifecycle moves to a **GOVERNED STATE-FREE DEPLOYMENT PLANE**.

### State-free deployment plane

“State-free deployment plane” means:

The deployment mechanism **MUST NOT** maintain Terraform/OpenTofu-style persistent desired-state/resource-state ownership of the App Platform applications when that state can contain secret material.

It **MUST NOT** persist:

- plaintext runtime secrets
- DigitalOcean `EV[...]` encrypted secret values
- opaque provider secret values
- secret-bearing App specs
- secret-bearing deployment plans
- reusable artifacts containing runtime secret values

It **MAY** persist non-secret evidence such as:

- governed source commit SHA
- Backend OCI manifest digest
- application ID after creation
- deployment ID
- timestamps
- approved topology metadata
- release classification
- validation result
- secret key **NAMES**
- secret rotation generation/version identifiers that reveal no secret value

“State-free” does **not** mean zero audit/evidence records. It means **zero persistent SECRET-BEARING resource state**.

### Deployment-plane responsibilities

The governed deployment plane owns production lifecycle operations for the two App Platform applications:

- create / controlled spec update
- immutable OCI digest selection
- run command / instance-size / instance count
- dedicated VPC attachment
- component-specific non-secret runtime configuration
- component-specific SECRET injection
- deployment/redeployment / controlled restart where required
- verification / rollback/reconciliation procedure

Exact implementation technology is **NOT** selected by this ADR. A later implementation/design gate must determine and validate the concrete mechanism. Do not invent a new CI/CD product or secret manager in this freeze.

### Secret assembly contract

Runtime secret plaintext **MUST** originate only from an explicitly authorized secret delivery source/process.

Secret-bearing App specs **MUST** be transient.

The implementation **MUST** prevent secret values from entering Git, source files, committed templates, logs, command-line arguments, shell history, reusable build artifacts, OpenTofu state, OpenTofu plans, ordinary deployment evidence, and documentation.

Where a secret-bearing request body/spec must exist transiently for API submission:

- keep lifetime minimal
- use process memory/stdin where supported
- do not print it
- do not commit it
- securely remove any unavoidable temporary local representation immediately after use
- validation must prove no residual secret-bearing file remains

DigitalOcean’s encrypted `EV[...]` representation is itself treated as **secret material** and **MUST NOT** be stored in governed persistent deployment evidence.

---

## Preserved ADR-AIEOS-048 / 048R1 topology

Explicitly preserved without alteration:

```text
production App Platform region          = blr
DigitalOcean datacenter                 = blr1
dedicated production VPC                = aieos-prod-blr1
CIDR                                    = 10.130.0.0/20
default-blr1 production reuse           = FORBIDDEN
application topology                    = TWO separate App Platform applications
WORKFLOW_DISPATCHER application         = aieos-prod-workflow-dispatcher
TEMPORAL_WORKER application             = aieos-prod-temporal-worker
component type                          = worker
WORKFLOW_DISPATCHER run command         = python -m aieos.platform.runtime.entrypoints.workflow_dispatcher_main
TEMPORAL_WORKER run command             = python -m aieos.platform.runtime.entrypoints.temporal_worker_main
first-production instance size          = apps-s-1vcpu-1gb-fixed
instance count                          = 1 each
Preview SKU exception                   = unchanged
single_instance_only acceptance         = unchanged
horizontal scaling                      = requires later architecture reconciliation / supported resize
dedicated egress                        = none
registry authority                      = eduvijna-registry
logical Backend repository              = aieos-backend
production image                        = ONE common governed Backend OCI image
production image authority              = immutable manifest digest only
mutable latest/tag                      = NOT production authority
deploy-on-push                          = false
dispatcher/worker                       = separate App lifecycle boundaries
Temporal workload identities            = separate
Temporal runtime API keys               = separate
component-level secret boundaries       = separate
USD 240 operating target                = unchanged
USD 250 hard service ceiling            = unchanged
price/provider drift revalidation       = unchanged
```

ADR-AIEOS-048 invariants and ADR-AIEOS-048R1 naming invariants remain in force except where this ADR supersedes App resource ownership.

### Independent infrastructure activation

Production infrastructure slices do not implicitly authorize one another. Independent authority remains required for:

- production VPC
- AIStor
- WORKFLOW_DISPATCHER production deployment
- TEMPORAL_WORKER production deployment
- Temporal Cloud provisioning plane

WORKFLOW_DISPATCHER and TEMPORAL_WORKER activation/release authorities are now **DEPLOYMENT-PLANE** authorities, **NOT** authorization for OpenTofu `digitalocean_app` resources. Exact implementation variable/flag names may change in subsequent Infrastructure reconciliation.

---

## Current merged Infrastructure source status

```text
eduvijna-aieos-infrastructure
main = 7a3b5070b138a5224de9594c9926d8cb36aa4507
```

That merged source contains `digitalocean_app` production modeling created before the empirical provider failure was known.

Classify that App ownership portion as:

```text
ARCHITECTURALLY SUPERSEDED — MUST REMAIN INACTIVE
```

The source is safe only while production App activation guards remain false and no production App plan/apply occurs.

Do **NOT** classify the entire Infrastructure merge as invalid. Preserve its valid work including:

- independent infrastructure activation boundaries
- VPC architecture
- AIStor isolation
- provider-compatible names
- immutable OCI digest validation
- commercial guardrails
- Temporal resource-address preservation
- zero-env source posture

A later narrow Infrastructure reconciliation is required to remove OpenTofu production App ownership.

### Required Infrastructure follow-up

After ADR-AIEOS-048R2 is merged, a separate explicitly authorized Infrastructure source gate must at minimum:

1. remove production `digitalocean_app` resource ownership from OpenTofu
2. remove/retire the reusable production `app_platform_worker` resource path if it exists solely to create `digitalocean_app` resources
3. ensure production OpenTofu plans contain **ZERO** `digitalocean_app` resources
4. preserve dedicated VPC and AIStor OpenTofu ownership
5. preserve independent dispatcher/worker release authorities conceptually, but move them to deployment-plane governance
6. update machine-readable App Platform contract/docs
7. add CI/static proof preventing `digitalocean_app` production ownership from returning without a future architecture revision
8. preserve all existing production mutation guards as fail-closed until the reconciliation is reviewed and merged

This Architecture gate does **not** implement that Infrastructure work.

### Required deployment-plane design gate

ADR-AIEOS-048R2 does **NOT** authorize an implementation yet.

Require a subsequent design/validation work item (suggested identifier):

```text
WPI-AP-DP01 — App Platform State-Free Deployment Plane Design
```

That gate must determine the exact deployment mechanism, secret source/delivery, App create/update request construction, safe VPC/App/project identifier resolution, immutable OCI digest input authority, non-secret configuration authority, secret injection mechanics, no-secret persistence controls, logs/redaction, create-vs-update reconciliation, failure recovery, partial deployment behavior, independent dispatcher/worker release, secret rotation, evidence/audit output, rollback, provider/API validation strategy, and stale-config/concurrent-update protection.

Do not prematurely select tooling in ADR-AIEOS-048R2.

### Required empirical safety validation

Before production App deployment can ever be authorized, the chosen deployment-plane implementation **MUST** be empirically validated using disposable non-production resources, proving at minimum: no plaintext/`EV[...]`/opaque secret in persistent local artifacts, Git, command history, logs, or ordinary deployment evidence; exact App spec desired fields preserved; safe reconciliation of remote configuration; updates do not silently delete required secret configuration; dispatcher/worker secret boundaries remain separate; cleanup succeeds.

A later explicit Founder authorization is required for any live validation.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A48R2-INV-01 | Production AIEOS DigitalOcean App Platform applications MUST NOT be owned by OpenTofu while provider behavior can materialize runtime secret material into plan/state. |
| A48R2-INV-02 | Encrypted DigitalOcean `EV[...]` secret material is secret material and MUST NOT be persisted in AIEOS infrastructure state, plans, or ordinary deployment evidence. |
| A48R2-INV-03 | Production App lifecycle is owned by a governed deployment plane with no persistent secret-bearing resource state. |
| A48R2-INV-04 | The deployment plane MUST preserve the exact two-App isolation and current provider-compliant application names frozen by ADR-AIEOS-048R1. |
| A48R2-INV-05 | The dedicated VPC, region, CIDR, first-production SKU, OCI digest authority, run commands, workload isolation, cost envelope, and secret separation remain unchanged. |
| A48R2-INV-06 | Production OpenTofu App Platform plan/apply remains forbidden until the superseded `digitalocean_app` ownership is removed from current Infrastructure source and reviewed. |
| A48R2-INV-07 | Production deployment remains forbidden until the state-free deployment plane is designed, implemented, empirically validated, and explicitly released. |
| A48R2-INV-08 | No implementation may treat `sensitive`, `ignore_changes`, encrypted remote state, separate state, or DigitalOcean-side secret encryption as substitutes for the no-secret-material-in-state invariant. |

---

## Historical record

Do **not** rewrite ADR-AIEOS-048 or ADR-AIEOS-048R1 as though they originally contained the R2 ownership decision. Historical decisions must remain auditable.

| ADR | Role |
|-----|------|
| ADR-AIEOS-048 | base first-production App runtime topology/delivery contract |
| ADR-AIEOS-048R1 | provider-compliant application naming revision (**CURRENT naming**) |
| ADR-AIEOS-048R2 | **CURRENT** production App Platform ownership/deployment-plane authority |

Only R2 supersedes the App resource-ownership mechanism.

---

## Consequences

### Positive

- Architecture rejects a provider behavior that would place encrypted secret material into infrastructure plan/state
- Topology/naming/OCI/VPC/workload isolation remain frozen and reusable
- Clear forward path: Infrastructure reconciliation + state-free deployment-plane design/validation

### Negative / residual risk

- Merged Infrastructure App ownership source remains architecturally superseded until removed
- Production App deployment blocked pending deployment-plane design, implementation, and empirical validation
- Additional operational tooling (outside OpenTofu App ownership) is required

### Explicit non-authorizations

ADR-AIEOS-048R2 is architecture authority only. It does **not** authorize:

- Architecture PR merge
- Infrastructure source correction
- Backend source changes
- production OpenTofu refresh / plan / apply
- production state access
- VPC creation
- App Platform production creation/update
- deployment-plane implementation
- DOCR publication
- Backend OCI publication
- Temporal runtime API-key creation
- runtime secret injection
- production deployment/restart
- workflow execution
- production DB access/migration

---

## Status

**Frozen / Approved** — architecture source authority only.

- [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) = base topology authority
- [ADR-AIEOS-048R1](ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md) = **CURRENT** naming authority
- [ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md) = **CURRENT** App Platform ownership/deployment authority

Production App Platform OpenTofu ownership = **REJECTED**.  
Production App Platform plan/apply = **NOT AUTHORIZED**.  
Infrastructure reconciliation = **REQUIRED**.  
State-free deployment-plane design = **REQUIRED**.
