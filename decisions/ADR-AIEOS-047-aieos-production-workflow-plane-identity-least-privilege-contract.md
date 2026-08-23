---
id: ADR-AIEOS-047
title: AIEOS Production Workflow Plane Identity & Least-Privilege Contract
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-23
last_updated: 2026-08-23
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-047 — AIEOS Production Workflow Plane Identity & Least-Privilege Contract

**Status:** Frozen / Approved  
**Date:** 2026-08-23  
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md)

**Catalogue note:** Frozen / Approved is architecture authority only. It does **not** authorize Temporal Cloud account/namespace/service-account/API-key creation or mutation, production Temporal access, production database access, database migration, WORKFLOW dispatcher Backend implementation, Temporal worker deployment, WORKFLOW dispatcher deployment, DigitalOcean mutation, OpenTofu apply, App Platform mutation, commercial purchase, production execution, or production deployment.

**ID family note:** `ADR-AIEOS-047` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) (Outcome-first Prepare Tomorrow). Do not rename or conflate these decisions.

Chief-facing alignment contract approved on **2026-08-23**.

Evidence SHAs at deposit (read-only gate):

- Architecture `origin/main`: `fab7d20da9097b47177afbad66c987b5b5f6c533`
- Infrastructure `origin/main`: `41c8aac26bf459fab7744efd90bbc595066669b1`
- Backend `origin/main`: `8e837d2ef723db468e18b0405cb8bbc039efa8c2` (read-only evidence/reference only)

Binding prior authority (bodies not rewritten by this ADR): ADR-AIEOS-023R1, ADR-AIEOS-024, ADR-AIEOS-026, ADR-AIEOS-028, ADR-AIEOS-029, ADR-AIEOS-031, ADR-AIEOS-037, ADR-AIEOS-045.

Backend evidence contracts inspected (read-only; Architecture gap ≠ implementation freedom):

- `src/aieos/platform/workflows/constants.py`
- `src/aieos/platform/workflows/temporal/gateway.py`
- `src/aieos/platform/workflows/temporal/dispatchers.py`
- `src/aieos/platform/workflows/persistence/repositories.py`
- `src/aieos/platform/runtime/config_temporal.py`
- `src/aieos/platform/runtime/entrypoints/temporal_worker_main.py`

---

## Context

[ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) selected Temporal as the durable workflow runtime, froze capability-oriented task queues, forbade per-tenant namespace/task-queue baselines, and stated that production Temporal hosting, namespace operations, and deployment remain independently unauthorized.

[ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) records Temporal Cloud as the production Temporal service at the infrastructure baseline level.

Production runtime and identity design now require one aligned production workflow-plane contract: hosting specialization, Namespace Endpoint connection mode, TLS, distinct Temporal Cloud identities for WORKFLOW_DISPATCHER and TEMPORAL_WORKER, stable built-in provider RBAC floor, and an application-level operation fence for the implemented `TemporalClientReviewGateway`.

**Precedence:** ADR-047 has current precedence for first-production Temporal hosting specialization, production Namespace topology, Namespace Endpoint connection mode, production Temporal TLS requirements, WORKFLOW_DISPATCHER / TEMPORAL_WORKER Temporal Cloud identity separation, minimum stable provider RBAC, WORKFLOW_DISPATCHER operation fence, dispatcher Temporal environment-variable names, and API-key rotation semantics. ADR-AIEOS-026 remains valid and is **not** rewritten. ADR-AIEOS-026 selected Temporal but deferred production hosting, namespace operations, and deployment; this ADR freezes those production specializations as **source authority only**.

Production provisioning remains unauthorized.

---

## Decision

### Production Temporal hosting

Freeze:

```text
Temporal Cloud
=
first-production Temporal hosting baseline
```

This is a production specialization of ADR-AIEOS-026. ADR-AIEOS-026 remains valid and must not be rewritten.

### Namespace topology

Freeze environment-isolated Namespace topology.

```text
Production and staging do NOT share one Namespace.
```

Production baseline:

```text
one AIEOS production Namespace
for the governed workflow capabilities in that environment
```

Do **not** create a Namespace per tenant.  
Do **not** create a task queue per tenant.

Preserve capability-oriented task queues from ADR-AIEOS-026.

### Current governed Content Review capability

Freeze exactly:

```text
Workflow type:
ContentReviewWorkflowV1

Task queue:
aieos.content.review

Signal:
review_decision_recorded
```

Do not invent additional production workflow types in this ADR. Future workflow types / task queues require governed source change / release.

### Connection endpoint mode

Freeze endpoint MODE:

```text
Temporal Cloud Namespace Endpoint
```

The exact provider-generated hostname is **not** frozen in this ADR. Runtime receives it as environment configuration.

Do **not** hard-code into Architecture source:

- region endpoint
- cloud region
- Temporal account ID
- Temporal namespace ID
- generated hostname
- IP address

Namespace Endpoint must be used as the stable production connection identity unless a later architecture decision explicitly replaces it. Exact port/hostname comes from Temporal provisioning output. No production DNS/IP probe is authorized by this ADR.

### TLS

Production Temporal connection requires:

```text
TLS enabled
server certificate verification enabled
platform trust store / governed CA trust
no plaintext fallback
```

Forbidden:

```text
tls=False
insecure channel
verify=False
certificate-verification bypass
silent plaintext fallback
```

Failure to establish a verified production Temporal connection: **FAIL CLOSED**.

### Temporal Cloud authentication

Freeze production workload authentication as:

```text
Temporal Cloud Service Account
+
Service-account API Key
```

API key is runtime **data-plane** authentication only. Do not inject a Cloud Ops administrative credential into application workloads.

No API key value may appear in:

```text
Git
ADR text
CHANGELOG
CI logs
PR comments
OpenTofu source
.tfvars
committed plan
runtime logs
exception text
```

Exact service-account IDs and key values are provisioning outputs, not source constants.

### Two distinct Temporal workload identities

Freeze distinct production identities for:

```text
A. WORKFLOW_DISPATCHER
B. TEMPORAL_WORKER
```

They must **not** share:

```text
service account
API key
secret environment variable
```

even if the provider permission class is currently the same.

Required rationale:

- independent rotation
- independent revocation
- smaller credential blast radius
- provider-side attribution where supported
- workload separation

Do not reuse:

- API runtime identity
- EVENT dispatcher identity
- database identity
- migrator identity
- human operator identity

Temporal Cloud identity and AIEOS business Principal identity are separate concepts.

### Provider RBAC baseline

Freeze the minimum **stable built-in** Temporal Cloud RBAC baseline for each runtime identity:

```text
Account access:
READ

Target production Namespace access:
WRITE
```

Forbidden runtime provider authority:

```text
Account Owner
Account Admin
Account Developer
Namespace Admin
```

Do not require Temporal Cloud Custom Roles for first production.

**Why:** As of 2026-08-23, Temporal Cloud Custom Roles are Pre-Release. They may be evaluated as a future least-privilege hardening step after GA and AIEOS validation.

Do **not** claim provider Namespace Write is operation-perfect least privilege. Provider built-in RBAC is currently **coarser** than the AIEOS workload operation boundary.

Therefore the first-production boundary is defense in depth:

```text
stable provider Namespace Write
+
separate service accounts / API keys
+
Backend source operation fence
+
CI / static abuse tests
```

### WORKFLOW_DISPATCHER allowed operation set

Freeze the current AIEOS WORKFLOW_DISPATCHER application authority to the operations required by the implemented `TemporalClientReviewGateway`.

Allowed current data-plane behavior:

1. Start the governed `ContentReviewWorkflowV1` Workflow Execution.
2. Start it only using the governed task queue `aieos.content.review`.
3. Describe an identified governed Workflow Execution.
4. Read/fetch Workflow Execution history required for idempotent start reconciliation and delivery reconciliation.
5. Signal the governed `review_decision_recorded` command signal.
6. Wait/read Workflow result/history required to determine command delivery reconciliation.

Normal SDK protocol/metadata calls strictly necessary to support the above are allowed.

The dispatcher is **not** a general Temporal operator.

### WORKFLOW_DISPATCHER forbidden operation set

Explicitly freeze as **NOT AUTHORIZED** for WORKFLOW_DISPATCHER:

```text
worker Workflow Task polling
worker Activity Task polling
worker task completion/failure APIs
namespace create/update/delete
Temporal Cloud account administration
service-account administration
API-key administration
Cloud Ops API usage
arbitrary Workflow types
arbitrary task queues
arbitrary Signals
Workflow terminate
Workflow cancel
Workflow reset
batch operations
Schedule administration
Search Attribute administration
Nexus administration
connectivity-rule administration
HA/failover administration
retention administration
namespace configuration mutation
```

unless a later governed architecture/source decision explicitly expands the contract.

Do not infer permission merely because provider Namespace Write may technically allow a broader operation.

### TEMPORAL_WORKER boundary

TEMPORAL_WORKER remains a distinct workload. Its purpose is to poll/execute/respond for registered workflow/activity code on governed capability task queues.

Current governed queue:

```text
aieos.content.review
```

Current worker secret/config family remains the existing:

```text
AIEOS_TEMPORAL_TARGET_HOST
AIEOS_TEMPORAL_NAMESPACE
AIEOS_TEMPORAL_API_KEY
AIEOS_TEMPORAL_CONNECT_TIMEOUT_SECONDS
AIEOS_TEMPORAL_SHUTDOWN_GRACE_SECONDS
```

Do **not** rename the already-shipped worker secret family in this architecture freeze.

Worker API key must not be reused by WORKFLOW_DISPATCHER. Worker is not granted Cloud Ops administration merely because it executes Workflow/Activity Tasks.

### WORKFLOW_DISPATCHER runtime config contract

Freeze these exact future dispatcher Temporal environment-variable names:

```text
AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_TARGET_HOST
AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_NAMESPACE
AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY
AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_CONNECT_TIMEOUT_SECONDS
```

Common release identity continues to use:

```text
AIEOS_DEPLOYMENT_ENVIRONMENT
AIEOS_RELEASE_VERSION
AIEOS_GIT_SHA
AIEOS_BUILD_ID
AIEOS_ARTIFACT_DIGEST
```

API key is secret and must be redacted from `repr` / `str` / logs / errors. Target host and Namespace are non-secret configuration.

Do not freeze actual production values. Do not freeze polling cadence or dispatcher batch size here.

### Initial connect

Future WORKFLOW_DISPATCHER startup must:

```text
load fail-closed config
        ↓
construct distinct dispatcher Temporal client identity
        ↓
connect to Namespace Endpoint with API key + TLS
        ↓
bound COMPLETE initial connection by configured connect timeout
        ↓
compose gateway/dispatchers only after connection success
```

Initial connection timeout/failure:

```text
startup FAIL CLOSED
non-zero process exit
```

No offline Core/fallback workflow transport. Do not invent a second workflow engine.

### API key rotation

Freeze rotation semantics only, not operational timing.

Required safe model:

```text
create/authorize replacement key
        ↓
inject replacement through encrypted runtime secret channel
        ↓
restart/redeploy affected workload
        ↓
verify healthy authenticated operation
        ↓
revoke old key
```

Dispatcher and worker rotate independently. No long-lived credential value is frozen into source. Exact operational runbook and rotation interval remain deployment/operations work.

### Workflow intent / tenant state semantics

Preserve [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) exactly.

Committed START and COMMAND intents remain infrastructure-deliverable even if a tenant later becomes SUSPENDED or DISABLED. Temporal delivery itself is not business authorization. Sensitive effects must revalidate CURRENT authority at the governed Activity / application-command boundary under ADR-AIEOS-026 / ADR-AIEOS-031.

Do **not**:

- suppress committed workflow intent merely because tenant later suspends
- treat delivered intent as permission
- treat Temporal history as permission
- treat Temporal API key as business authorization
- delete intent because current authority changed

### Database / Temporal identity separation

WORKFLOW dispatcher database login remains distinct from its Temporal Cloud service account. Candidate-reader remains NOLOGIN and has no credential.

Do not conflate:

```text
PostgreSQL authentication
Temporal Cloud authentication
AIEOS Principal identity
business capability authorization
```

Candidate discovery does not invoke Temporal. Temporal service account does not grant database authority.

### Audit / observability

Freeze minimum operational evidence:

```text
workload kind
release / git SHA
non-secret Temporal client identity
non-secret Namespace identifier
Workflow ID
Workflow type
task queue
workflow intent ID
command ID where appropriate
delivery / reconciliation result
retry / quarantine error category
shutdown / startup category
```

Do **not** log:

```text
workflow start input payload
workflow command payload
API key
DB URL / password
authorization token
secret values
```

Temporal provider attribution/audit evidence is supplemental operational evidence. It is **not**:

- AIEOS `security.audit_records` authority
- approval truth
- business authorization truth

Do not require Temporal Principal Attribution for first production because its current provider status is Pre-Release.

### Networking

Do **not** require AWS PrivateLink / GCP Private Service Connect for the first-production source contract because AIEOS production workloads are DigitalOcean-hosted.

Freeze:

```text
verified TLS to the Temporal Namespace Endpoint
```

Stable IP allowlisting may be used by later provisioning if required. Do not freeze an IP range in Architecture. Do not create a public proxy or AIEOS-owned Temporal gateway. No network/cloud mutation is authorized by this ADR.

### Custom Roles / future hardening

Temporal Cloud Custom Roles are **not** a first-production dependency. When the feature becomes GA and is validated, AIEOS may introduce narrower provider-enforced dispatcher/worker permissions through a future governed source freeze. Do not block this Architecture freeze on a Pre-Release feature.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A47-INV-01 | Temporal Cloud is first-production workflow hosting baseline. |
| A47-INV-02 | Production connects using a Temporal Namespace Endpoint; exact generated endpoint is provisioning output. |
| A47-INV-03 | Namespaces are environment-isolated, not tenant-per-namespace. |
| A47-INV-04 | Capability-oriented task queues remain the baseline. |
| A47-INV-05 | WORKFLOW_DISPATCHER and TEMPORAL_WORKER use distinct Temporal service accounts and distinct API keys. |
| A47-INV-06 | Minimum stable provider RBAC is Account Read + target Namespace Write; no runtime Namespace Admin / Account Admin. |
| A47-INV-07 | Custom Roles are not required for first production. |
| A47-INV-08 | WORKFLOW_DISPATCHER is fenced to governed start / describe / history / signal / result operations for the current workflow contract. |
| A47-INV-09 | WORKFLOW_DISPATCHER does not poll Worker tasks or perform Temporal control-plane administration. |
| A47-INV-10 | Current Content Review workflow type is `ContentReviewWorkflowV1`. |
| A47-INV-11 | Current Content Review task queue is `aieos.content.review`. |
| A47-INV-12 | Current dispatcher signal is `review_decision_recorded`. |
| A47-INV-13 | Dispatcher Temporal secret env is `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY`. |
| A47-INV-14 | Worker retains separate `AIEOS_TEMPORAL_API_KEY` authority. |
| A47-INV-15 | Production Temporal transport requires TLS and certificate verification. |
| A47-INV-16 | Committed workflow intents remain infrastructure-deliverable; sensitive effects revalidate current authority. |
| A47-INV-17 | Temporal auth identity is not AIEOS business authorization or database identity. |
| A47-INV-18 | No production credential value or generated endpoint is frozen into source. |

---

## Explicit non-authorizations

ADR-AIEOS-047 does **not** authorize:

```text
Temporal Cloud account creation
Temporal Namespace creation
Temporal Namespace modification
Temporal service-account creation
Temporal API-key creation
Temporal API-key revocation
Temporal Cloud Ops API mutation
production Temporal access
production database access
database migration
WORKFLOW dispatcher Backend implementation
Temporal worker deployment
WORKFLOW dispatcher deployment
DigitalOcean mutation
OpenTofu apply
App Platform mutation
commercial purchase
production execution
production deployment
```

It freezes **SOURCE AUTHORITY** only.

WORKFLOW dispatcher Backend runtime remains **NOT YET AUTHORIZED**.  
Temporal Cloud production provisioning remains **NOT AUTHORIZED**.  
Production execution remains **NOT AUTHORIZED**.

---

## Consequences

- Infrastructure source may later encode and verify this contract without creating production Temporal resources under this ADR.
- Future Backend WORKFLOW dispatcher source must use the frozen dispatcher Temporal env names and distinct API-key secret, fail closed on connect, and remain inside the operation fence.
- Existing Temporal worker env family (`AIEOS_TEMPORAL_*`) is preserved and must not be reused as the dispatcher secret.
- Historical ADR-026 deferral of production hosting remains historical context; current production hosting specialization follows this ADR without rewriting ADR-026.
- Catalogue and current-state surfaces must present the workflow plane as **ARCHITECTURE FROZEN / APPROVED**, not implemented / provisioned / deployed / production-ready / activated.

---

## Related ADRs

| ID | Relationship |
|----|--------------|
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Identity / tenant / security baseline |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | Data / SoR baseline |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Temporal selection; capability task queues; deferred production hosting remains valid and not rewritten |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Audit ≠ authorization |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Deploy ≠ mutation enabled |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Sensitive-effect revalidation |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | Temporal Cloud as production Temporal service |
| [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) | Committed intent deliverability; candidate discovery; no Temporal auth conflation |
| Teacher OS [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) | Distinct decision family — Outcome-first Prepare Tomorrow; not this ADR |
