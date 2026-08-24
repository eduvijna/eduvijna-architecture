---
id: ADR-AIEOS-049
title: AIEOS App Platform State-Free Deployment Plane
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-24
last_updated: 2026-08-24
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-049 — AIEOS App Platform State-Free Deployment Plane

**Status:** Frozen / Approved  
**Date:** 2026-08-24  
**Related:** [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) · [ADR-AIEOS-048R1](ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md) · [ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR freezes the **WPI-AP-DP01** design for the governed state-free App Platform deployment plane required by [ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md). It does **not** authorize implementation, disposable live validation, credential issuance, or production execution. Do **not** rewrite ADR-AIEOS-048 / 048R1 / 048R2 historical bodies.

**ID family note:** `ADR-AIEOS-049` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS ADR numbering.

---

## Governed baselines (deposition record)

```text
Architecture   origin/main = 8d07e1ba64f0fd0f4579db322391521b8d3f2137
Infrastructure origin/main = 157f25a6a580d01be92ce798594302cdfe84cc9f
Backend        origin/main = 8f4dd172e6a0ba8b4ad944b0ae22060442356342
```

Infrastructure **WPI-AP-I02** (remove OpenTofu App ownership) = **FORMALLY CLOSED** at Infrastructure main `157f25a6a580d01be92ce798594302cdfe84cc9f`.

---

## Context

[ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md) rejected production OpenTofu `digitalocean_app` ownership after WPI-AP-SV01/R1 proved DigitalOcean provider 2.99.1 materializes out-of-band encrypted `EV[...]` secret material into plan/state.

WPI-AP-I02 removed superseded OpenTofu App resource paths from Infrastructure. Production App lifecycle therefore requires a **governed state-free deployment plane**.

This ADR freezes that plane’s required behavior (WPI-AP-DP01 design). Exact secret-delivery product and concrete lease technology remain intentionally unselected for later implementation design, but the binding semantics below are architecture authority.

---

## Decision

### 1. Deployment-plane owner

Production DigitalOcean App Platform application lifecycle is owned by a **purpose-built governed state-free release controller** using **DigitalOcean REST APIs directly**.

OpenTofu `digitalocean_app` ownership remains **REJECTED** under ADR-AIEOS-048R2.

The controller is **not** a persistent desired-state engine.

No persistent secret-bearing AppSpec, Terraform/OpenTofu state, plan, cache, or equivalent may be used as App lifecycle authority.

### 2. One App / one process / one release

Each controller process handles **exactly one** authorized production App and **exactly one** release/reconciliation operation.

WORKFLOW_DISPATCHER and TEMPORAL_WORKER remain independent applications, release lifecycles, deployment credentials, runtime credentials, secret sets, rollback boundaries, and failure domains.

The controller process **must never** carry both workload secret sets.

### 3. Frozen target topology

| Concern | WORKFLOW_DISPATCHER | TEMPORAL_WORKER |
|---------|---------------------|-----------------|
| App name | `aieos-prod-workflow-dispatcher` | `aieos-prod-temporal-worker` |
| Component type | worker | worker |
| Component name | `aieos-prod-workflow-dispatcher` | `aieos-prod-temporal-worker` |
| Run command | `python -m aieos.platform.runtime.entrypoints.workflow_dispatcher_main` | `python -m aieos.platform.runtime.entrypoints.temporal_worker_main` |

Both applications:

```text
App Platform region              = blr
DigitalOcean datacenter          = blr1
dedicated VPC                    = aieos-prod-blr1
CIDR                             = 10.130.0.0/20
default-blr1 reuse               = FORBIDDEN
instance size                    = apps-s-1vcpu-1gb-fixed
instance count                   = 1
registry                         = eduvijna-registry
repository                       = aieos-backend
image authority                  = immutable OCI manifest digest only
mutable latest/tag authority     = FORBIDDEN
deploy_on_push                   = false
app-level runtime environment    = FORBIDDEN
runtime environment placement    = worker component only
```

### 4. Immutable OCI authority

A production release accepts only an already-governed immutable manifest digest matching:

```text
sha256:<64 lowercase hexadecimal characters>
```

The deployment controller does **not** build, publish, promote, mutate, or infer the OCI artifact.

`AIEOS_ARTIFACT_DIGEST` **must** equal the exact governed OCI manifest digest deployed into the App.

`AIEOS_GIT_SHA`, release version, build ID, and artifact digest must come from the governed release record.

### 5. Project / VPC resolution

Before mutation, resolve and verify the exact authorized AIEOS Production project.

Resolve the dedicated production VPC and require exact conformance:

```text
name = aieos-prod-blr1
region/datacenter = blr1
CIDR = 10.130.0.0/20
```

No default-blr1 adoption. No fallback VPC. No VPC creation by the deployment controller. Missing, duplicate, ambiguous, or mismatching project/VPC authority **fails closed**.

### 6. Runtime environment ownership

**Common GENERAL / RUN_TIME**

- `AIEOS_DEPLOYMENT_ENVIRONMENT`
- `AIEOS_RELEASE_VERSION`
- `AIEOS_GIT_SHA`
- `AIEOS_BUILD_ID`
- `AIEOS_ARTIFACT_DIGEST`

**WORKFLOW_DISPATCHER SECRET / RUN_TIME**

- `AIEOS_WORKFLOW_DISPATCHER_DATABASE_URL`
- `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY`

**WORKFLOW_DISPATCHER GENERAL / RUN_TIME**

- `AIEOS_WORKFLOW_DISPATCHER_ROLE`
- `AIEOS_WORKFLOW_DISPATCHER_DATABASE_CONNECT_TIMEOUT_SECONDS`
- `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_TARGET_HOST`
- `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_NAMESPACE`
- `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_CONNECT_TIMEOUT_SECONDS`
- `AIEOS_WORKFLOW_DISPATCHER_POLL_INTERVAL_SECONDS`
- `AIEOS_WORKFLOW_DISPATCHER_CANDIDATE_BATCH_SIZE`
- `AIEOS_WORKFLOW_DISPATCHER_MAX_INTENTS_PER_TENANT_PER_PASS`
- `AIEOS_WORKFLOW_DISPATCHER_CLAIM_LEASE_SECONDS`
- `AIEOS_WORKFLOW_DISPATCHER_MAX_ATTEMPTS`
- `AIEOS_WORKFLOW_DISPATCHER_RETRY_DELAY_SECONDS`
- `AIEOS_WORKFLOW_DISPATCHER_RESULT_TIMEOUT_SECONDS`
- `AIEOS_WORKFLOW_DISPATCHER_START_RECONCILIATION_TIMEOUT_SECONDS`
- `AIEOS_WORKFLOW_DISPATCHER_SHUTDOWN_GRACE_SECONDS`

**TEMPORAL_WORKER SECRET / RUN_TIME**

- `AIEOS_TEMPORAL_API_KEY`

**TEMPORAL_WORKER GENERAL / RUN_TIME**

- `AIEOS_TEMPORAL_TARGET_HOST`
- `AIEOS_TEMPORAL_NAMESPACE`
- `AIEOS_TEMPORAL_CONNECT_TIMEOUT_SECONDS`
- `AIEOS_TEMPORAL_SHUTDOWN_GRACE_SECONDS`

Do **not** freeze unresolved production numeric daemon/timeout/batch values in ADR-AIEOS-049. Backend validation bounds are not production operating-value authority.

### 7. Secret handling

Plaintext secrets may enter only the short-lived release process through a separately authorized secret-delivery mechanism.

DigitalOcean encrypted `EV[...]` values are classified as **SECRET MATERIAL**.

For update reconciliation, unchanged DigitalOcean SECRET values may be retained only **transiently in process memory** using the current encrypted provider representation.

Neither plaintext nor `EV[...]` nor other opaque secret-bearing values may be persisted in:

Git; HCL; Terraform/OpenTofu state; plans; JSON/YAML AppSpec files; temporary files; CLI arguments; shell history; logs; traces; exception dumps; release artifacts; caches; evidence; screenshots.

The secret-delivery product/mechanism is intentionally **NOT** selected by ADR-AIEOS-049.

### 8. Create flow

For an explicitly authorized bootstrap create:

1. acquire per-App durable lease  
2. perform all read-only authority/preflight checks  
3. require target App cardinality = zero  
4. construct complete AppSpec only in process memory  
5. include explicit authorized project identity and dedicated VPC identity  
6. include exact one-worker topology  
7. include governed immutable OCI digest  
8. include exact worker-level runtime environment  
9. submit create once  
10. reconcile provider outcome  
11. verify resulting App exactly  
12. persist only redacted evidence  
13. terminate process  

Initial production App creation itself remains **NOT AUTHORIZED** by this ADR.

### 9. Update flow

For an existing authorized App:

1. acquire per-App durable lease  
2. GET live App/spec into process memory  
3. validate full App against exact AIEOS allowlist  
4. preserve unchanged SECRET provider representations only transiently  
5. apply only the explicitly authorized release/rotation delta  
6. perform a second GET immediately before mutation  
7. compare `updated_at` plus normalized non-secret managed fingerprint, topology, secret key set, secret type/scope structure  
8. if anything changed since first read: **fail closed**  
9. PUT the full desired AppSpec exactly once  
10. do not blind-retry ambiguous writes  
11. reconcile provider result  
12. verify deployment and exact post-state  
13. emit redacted evidence  
14. terminate process  

### 10. Exact allowlist reconciliation

Recognized (non-blocking) classifications:

- `DRIFT_NONE`
- `DRIFT_SECRET_CIPHERTEXT_ONLY`
- `DRIFT_ALLOWED_PROVIDER_DEFAULT`

Blocking classifications:

- `DRIFT_NONSECRET_MANAGED`
- `DRIFT_SECRET_KEY_SET`
- `DRIFT_TOPOLOGY`
- `DRIFT_UNMANAGED_COMPONENT`
- `DRIFT_REMOTE_CHANGED_DURING_RELEASE`

Unexpected service/job/static-site/function/database/domain/autoscaling/additional worker/app-level env/image source/secret key **fails closed**.

There is **no force bypass**.

### 11. Concurrency protection

Because no provider-side conditional AppSpec CAS contract is architecture authority, production reconciliation requires **BOTH**:

1. a globally durable, exclusive, expiring, owner-identifiable **per-App lease**  
2. **double-read stale-write detection** immediately before mutation  

A local filesystem lock alone is **insufficient**.

The concrete lease technology is intentionally left to implementation design, but these semantics are binding.

If the implementation platform cannot supply these semantics, production release is **non-conforming**.

### 12. Deployment credential separation

WORKFLOW_DISPATCHER and TEMPORAL_WORKER deployment-plane credentials are **separate**.

Bootstrap creation credentials and steady-state release credentials should be separate in capability/lifetime where provider controls permit.

Steady-state credentials must not gain unnecessary `app:create`, `app:delete`, console, registry mutation, VPC mutation, broad project mutation, or broad API-write authority.

DigitalOcean deployment credentials are not workload runtime credentials and are never business authorization.

Actual credential issuance remains **NOT AUTHORIZED**.

### 13. Rotation

Temporal workload credential rotation remains:

```text
replacement credential
→ transient delivery to only affected workload release
→ deploy
→ verify healthy cutover
→ revoke old credential
```

Dispatcher and worker rotate independently.

Database credential rotation is dispatcher-only and remains dependent on separate production DB/candidate-authority readiness.

No workload self-rotation.

### 14. Redacted evidence

Persistent release evidence may contain only non-secret audit facts such as:

App ID/name; deployment ID; project/VPC identifiers; Architecture/Infrastructure/Backend governed SHAs; OCI digest; release version/build ID; timestamps/duration; redacted normalized AppSpec fingerprint; secret **KEY NAMES** only; credential generation labels without values; provider request IDs; drift classification; verification outcome; rollback outcome.

Never persist plaintext, `EV[...]` values, opaque secret values, raw AppSpecs, or raw HTTP request/response bodies.

Canonical fingerprints must replace secret values with a fixed structural sentinel before hashing.

### 15. Ambiguous result / failure recovery

No blind mutation retries.

After timeout, connection loss, uncertain 5xx-after-send, or other ambiguous result:

1. perform read-only reconciliation  
2. inspect current App and deployment history  
3. determine whether the intended operation committed  
4. mutate again only when deterministically proven safe  

If outcome cannot be reconciled, stop and escalate.

### 16. Native rollback

Do not persist previous full AppSpecs for rollback.

Use DigitalOcean native deployment rollback authority:

```text
validate rollback
→ rollback to recorded last-known-good deployment
→ verify while rollback is pinned
→ commit rollback/unpin only after successful verification
```

If rollback state is ambiguous, do not auto-commit and do not issue additional automated mutations.

First-ever App creation has no prior deployment rollback target. A failed initial App remains for explicit recovery; ordinary release authority does not imply deletion.

### 17. Mandatory disposable empirical validation

Before any production implementation is released for use, a separately authorized disposable non-production validation must prove at minimum:

- direct REST create  
- full-spec update  
- in-memory `EV[...]` preservation  
- dummy-secret rotation  
- zero persistent secret material  
- dispatcher/worker secret isolation  
- managed drift rejection  
- unexpected secret-key rejection  
- durable per-App concurrency exclusion  
- stale-write/double-read fence  
- ambiguous-mutation reconciliation  
- native rollback validation / rollback / commit  
- secret structural survival after rollback  
- evidence redaction  
- complete disposable cleanup  

Use dummy secrets only. Use no production state or real production secrets.

Suggested classifications:

| Code | Classification |
|------|----------------|
| A | `PASS_STATE_FREE_DEPLOYMENT_PLANE` |
| B | `FAIL_SECRET_PERSISTENCE` |
| C | `FAIL_DRIFT_FENCE` |
| D | `FAIL_CONCURRENCY_FENCE` |
| E | `FAIL_ROLLBACK_RECOVERY` |
| F | `INCONCLUSIVE` |
| G | `CLEANUP_FAILURE` |

`CLEANUP_FAILURE` overrides all other results.

ADR-AIEOS-049 does **NOT** authorize this live validation.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A49-INV-01 | Production App Platform lifecycle is owned by a governed state-free REST release controller; OpenTofu `digitalocean_app` ownership remains rejected. |
| A49-INV-02 | Each controller process handles exactly one App and one release; dispatcher and worker secret sets never coexist in one process. |
| A49-INV-03 | Production image authority is immutable `sha256:<64 lowercase hex>` digest only; mutable tags are forbidden. |
| A49-INV-04 | Plaintext and DigitalOcean `EV[...]` secret material must never persist outside short-lived process memory. |
| A49-INV-05 | Production mutation requires durable per-App lease plus double-read stale-write fence; local filesystem lock alone is insufficient. |
| A49-INV-06 | Exact allowlist reconciliation has no force bypass; unexpected topology/secret-key/unmanaged components fail closed. |
| A49-INV-07 | Rollback uses native DigitalOcean deployment rollback; prior full AppSpecs are not persisted for rollback. |
| A49-INV-08 | Disposable empirical validation must PASS before production implementation release; ADR-049 alone does not authorize that validation or production execution. |

---

## Architecture relationship

| ADR | Role |
|-----|------|
| ADR-AIEOS-048 | historical/base topology and OCI-delivery authority where not superseded |
| ADR-AIEOS-048R1 | **CURRENT** naming authority |
| ADR-AIEOS-048R2 | **CURRENT** App ownership-boundary authority (rejects OpenTofu `digitalocean_app`) |
| ADR-AIEOS-049 | **CURRENT** detailed state-free deployment-plane behavior demanded by ADR-AIEOS-048R2 |

---

## Consequences

### Positive

- Concrete, reviewable deployment-plane contract without restoring OpenTofu secret-state risk  
- Independent dispatcher/worker failure domains and secret boundaries preserved  
- Explicit concurrency, drift, rollback, and evidence redaction requirements  

### Negative / residual risk

- Implementation, lease technology, and secret-delivery product still required  
- Disposable empirical validation still required before production use  
- Provider API semantics may require careful ambiguous-result reconciliation  

### Explicit non-authorizations

ADR-AIEOS-049 architecture freeze does **NOT** authorize:

- deployment-controller implementation  
- deployment-controller CI/CD release  
- durable lease implementation  
- production deployment credentials  
- Temporal runtime API-key issuance/revocation  
- production DB credential creation/mutation  
- secret-delivery integration  
- production state access  
- production OpenTofu plan/apply/refresh  
- VPC creation/update  
- App Platform App creation/update/delete  
- DOCR repository/subscription mutation  
- OCI build/publication/promotion  
- runtime secret injection  
- App restart/redeployment  
- WORKFLOW_DISPATCHER execution  
- TEMPORAL_WORKER execution  
- production workflow execution  
- any production deployment or cloud mutation  
- disposable live validation (requires separate Founder authorization)  

---

## Status

**Frozen / Approved** — architecture source authority only.

Design = **frozen/approved**.  
Implementation = **REQUIRED / NOT AUTHORIZED**.  
Disposable empirical validation = **REQUIRED / NOT AUTHORIZED**.  
Production App Platform deployment = **NOT AUTHORIZED**.
