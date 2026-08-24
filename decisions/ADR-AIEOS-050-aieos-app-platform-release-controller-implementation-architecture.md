---
id: ADR-AIEOS-050
title: AIEOS App Platform Release Controller Implementation Architecture
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-24
last_updated: 2026-08-24
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-050 — AIEOS App Platform Release Controller Implementation Architecture

**Status:** Frozen / Approved  
**Date:** 2026-08-24  
**Related:** [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) · [ADR-AIEOS-048R1](ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md) · [ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md) · [ADR-AIEOS-049](ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR freezes the **WPI-AP-DP02** release-controller **implementation architecture** required to realize [ADR-AIEOS-049](ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md). It does **not** authorize controller source implementation, workflow implementation, credential issuance, disposable live validation, or production execution. Do **not** rewrite ADR-AIEOS-048 / 048R1 / 048R2 / 049 historical bodies.

**ID family note:** `ADR-AIEOS-050` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS ADR numbering.

---

## Governed baselines (deposition record)

```text
Architecture   origin/main = 6ca27bf73fc321ac53d7bd6b37760f91df6b3bc2
Infrastructure origin/main = 157f25a6a580d01be92ce798594302cdfe84cc9f
Backend        origin/main = 8f4dd172e6a0ba8b4ad944b0ae22060442356342
```

---

## Context

[ADR-AIEOS-049](ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md) froze the state-free App Platform deployment-plane **behavior** (WPI-AP-DP01) after [ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md) rejected production OpenTofu `digitalocean_app` ownership.

ADR-AIEOS-049 intentionally left implementation placement, runtime toolchain, GitHub workflow/Environment identities, concurrency lease technology, secret-delivery product, HTTP client posture, credential-scope contract, evidence sink, and offline CI proof areas unselected.

This ADR freezes those **implementation-architecture** selections (WPI-AP-DP02 design) so later implementation gates remain separated and reviewable. Exact controller source does **not** exist yet and is **not** authorized by this deposition.

---

## Decision

### 1. Controller placement

Future controller implementation belongs in **`eduvijna-aieos-infrastructure`** as an isolated release tool.

It must **not** be packaged into the Backend package or production Backend OCI image.

It must remain separate from OpenTofu resource ownership.

Expected future logical source boundary:

```text
tools/app_platform_release/
```

Exact implementation source does **NOT** exist yet and is **NOT** authorized by this ADR deposition.

### 2. Runtime

| Concern | Frozen value |
|---------|--------------|
| Language | Python 3.14 |
| Initial implementation/CI pin | Python 3.14.7 |
| Dependency management | `uv` with committed lockfile |
| Production release runner | GitHub-hosted `ubuntu-24.04` **only** |
| Self-hosted production release runners | **FORBIDDEN** |

The release process is ephemeral and handles **one App / one release** only.

### 3. Two fixed production workflows

Future production workflow identities are:

```text
app-platform-release-workflow-dispatcher.yml
app-platform-release-temporal-worker.yml
```

Both must be **manual `workflow_dispatch` only**.

**Forbidden:**

- push deployment  
- `pull_request` deployment  
- `pull_request_target` deployment  
- schedule deployment  
- deploy-on-push  
- matrix target  
- arbitrary App target  
- arbitrary App ID  
- arbitrary DigitalOcean URL  
- arbitrary HTTP method  

### 4. GitHub Environment separation

Future workload GitHub Environment identities:

```text
aieos-prod-workflow-dispatcher
aieos-prod-temporal-worker
```

Secrets from one workload Environment must **never** be available to the other release process.

Environment secret storage is the selected **first-production process-local secret-delivery boundary**.

Repository-wide or organization-wide production workload secrets are **forbidden** for these App releases.

Non-secret desired runtime configuration must remain **governed source** rather than mutable Environment variables.

Actual GitHub Environment / secret creation is **NOT** authorized by ADR-AIEOS-050.

### 5. Per-App durable lease

| Workload | Concurrency group |
|----------|-------------------|
| Dispatcher | `aieos-prod-app-release-workflow-dispatcher` |
| Worker | `aieos-prod-app-release-temporal-worker` |

Required GitHub Actions concurrency semantics:

```text
cancel-in-progress: false
queue: single
```

Required release job timeout: **60 minutes**.

Lease evidence identifies:

- `github.run_id`  
- `github.run_attempt`  
- exact concurrency group  
- exact App identity  

[ADR-AIEOS-049](ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md) double-read stale-write detection remains **independently mandatory**.

Concurrency is **not** a substitute for provider reconciliation.

### 6. Direct DigitalOcean REST client

Use direct DigitalOcean v2 REST via an internally controlled Python **`httpx`** client.

**Do not use:**

- OpenTofu App ownership  
- `doctl` as production mutation engine  
- generic shell `curl` deployment  
- generic arbitrary DigitalOcean API proxy  

Required HTTP posture:

```text
base = https://api.digitalocean.com
TLS verification = required
trust_env = false
redirects = disabled
automatic transport mutation retries = zero
```

Read-only provider operations may use explicitly bounded retries.

### 7. Closed mutation allowlist

Controller behavior may expose only architecture-authorized typed operations such as:

| Operation | Bound |
|-----------|-------|
| `CREATE` | `POST /v2/apps` |
| `UPDATE` / `ROTATE_SECRET` | controlled full-spec `PUT` against exact authorized App ID |
| `ROLLBACK` | DigitalOcean native rollback validation / rollback / verification / commit sequence per ADR-AIEOS-049 |

**No** generic:

- `DELETE`  
- restart  
- console  
- arbitrary method  
- arbitrary endpoint  
- arbitrary App mutation API  

No blind retry after a mutation request may have been transmitted.

### 8. DigitalOcean credential model

Four logical PAT classes are frozen:

1. dispatcher bootstrap PAT  
2. dispatcher steady-state release PAT  
3. worker bootstrap PAT  
4. worker steady-state release PAT  

These are **distinct secret values** and release boundaries.

DigitalOcean PATs are provider personal access tokens; ADR-AIEOS-050 does **NOT** claim separate provider App-specific principals.

Steady-state permissions must be custom-scoped to only the exact required read and App-update capabilities.

At design time the required scope posture includes:

```text
app:update
app:read
regions:read
sizes:read
actions:read
project:read
vpc:read
registry:read
```

Bootstrap additionally requires only the create/assignment scopes actually proven necessary by current provider contract, including:

```text
app:create
project:assign_resource
```

**Forbidden** for normal release authority:

```text
app:delete
app:access_console
broad api:write
registry mutation
VPC mutation
broad project mutation
```

Provider scope requirements must be revalidated immediately before credential creation.

AIEOS operating lifetime: **≤ 90 days**; rotation target begins **T-30 days**.

Actual PAT creation is **NOT** authorized.

### 9. Runtime secret delivery

Future dispatcher Environment secret family includes:

```text
AIEOS_DO_APP_RELEASE_TOKEN
AIEOS_WORKFLOW_DISPATCHER_DATABASE_URL
AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY
```

Temporary bootstrap authority may additionally use a distinct bootstrap token.

Future worker Environment secret family includes:

```text
AIEOS_DO_APP_RELEASE_TOKEN
AIEOS_TEMPORAL_API_KEY
```

Temporary bootstrap authority may additionally use a distinct bootstrap token.

Do **not** freeze secret values. Do **not** create these secrets under ADR-AIEOS-050.

Secrets must be mapped only to the controller execution step, not globally to the whole workflow/job where avoidable.

Controller moves them immediately into redacting in-memory holders and removes their process-environment entries.

No child process may be spawned after secret acquisition.

### 10. Non-secret source authority

Infrastructure governed source remains authority for non-secret desired configuration.

Future source contract split:

```text
contracts/app-platform/production-workflow-runtime.yaml
  = workload/App runtime topology and non-secret governed runtime contract

contracts/app-platform/production-release-plane.yaml
  = release-controller behavior / workflow identity / environment identity /
    concurrency / endpoint allowlist / credential-scope contract / evidence schema /
    physical IDs when frozen / reconciliation controls
```

GitHub Environment variables must **not** become an unofficial production config database.

Unresolved dispatcher cadence/batch/timeout operating values remain unresolved and must **not** be invented by ADR-AIEOS-050.

### 11. Physical ID pinning

Before production bootstrap create, exact production project UUID and dedicated VPC UUID must be separately verified and frozen into governed source.

Bootstrap target App cardinality must equal **zero**.

After first successful production App creation, returned App UUID must be captured only as **non-secret evidence**.

No second steady-state release may occur until a source-only reconciliation gate pins the exact App UUID into governed source.

Steady state requires exact match of:

- App UUID  
- App semantic name  
- project UUID  
- VPC UUID  

### 12. OCI digest authority

The controller receives only an already-governed:

```text
sha256:<64 lowercase hex>
```

Backend OCI manifest digest.

It does **not** build, publish, promote, retag, or infer the artifact.

Before mutation it must prove the authorized digest exists in:

```text
registry   = eduvijna-registry
repository = aieos-backend
```

using read-only provider authority.

OCI source-SHA/provenance correlation remains a separate governed release gate.

### 13. Secret / EV handling

[ADR-AIEOS-049](ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md) remains binding:

plaintext secrets and DigitalOcean `EV[...]` representations are **SECRET MATERIAL**.

`EV[...]` may be retained only transiently in the one-release process when required for full-spec update.

Never persist secret material in:

Git; AppSpec files; temporary files; OpenTofu state; plans; logs; traces; exception dumps; Actions artifacts; step summaries; CLI arguments; shell history; cache; screenshots.

### 14. Logging / crash protections

Future production release process requires:

```text
GitHub-hosted ubuntu-24.04
ulimit -c 0
set -euo pipefail
set -x = forbidden
ACTIONS_STEP_DEBUG=true  => FAIL CLOSED
ACTIONS_RUNNER_DEBUG=true => FAIL CLOSED
```

Additional protections:

- HTTPX/HTTPCORE debug logging disabled  
- raw HTTP request/response bodies forbidden from logs  
- raw AppSpec output forbidden  
- `EV[...]` output forbidden  
- temporary AppSpec files forbidden  
- external telemetry/crash reporters forbidden from the secret-bearing process  
- secret-bearing types must redact `str`/`repr`  
- one release then process exit  

Do **not** rely solely on provider/platform automatic masking.

### 15. Ambiguous mutation result

Mutation request: **exactly once**.

If response becomes ambiguous after send (timeout, connection loss, uncertain 5xx, runner interruption, other unknown result):

transition only to **read-only reconciliation**.

Inspect current App identity/state and deployment history.

Mutate again only when deterministically proven safe.

No retry decorator or HTTP transport may blindly replay `POST`/`PUT`.

### 16. Release state machine

Required logical states:

```text
SOURCE_GATE
LEASE_ACQUIRED
PROVIDER_PREFLIGHT
LIVE_SPEC_READ_1
ALLOWLIST_VALIDATE
DESIRED_SPEC_IN_MEMORY
LIVE_SPEC_READ_2
STALE_WRITE_FENCE
MUTATION_SENT_ONCE
RESULT_RECONCILIATION
DEPLOYMENT_VERIFY
RECEIPT
PROCESS_EXIT
```

Any uncertainty after `MUTATION_SENT_ONCE` enters ambiguous-result reconciliation, **not** mutation.

### 17. Release receipt

First-production persistent evidence sink:

```text
sanitized GitHub Actions artifact: release-receipt.json
retention: 90 days
```

plus non-secret workflow/job metadata.

Receipt **may** contain:

App name; App UUID; deployment ID; project/VPC identifiers; Architecture/Infrastructure/Backend governed SHAs; OCI digest; release/build identity; `run_id`; `run_attempt`; concurrency group; redacted canonical managed-spec fingerprint; secret **KEY NAMES** only; non-secret generation labels; provider request IDs; drift classification; verification result; rollback result; receipt SHA-256.

Receipt **MUST NOT** contain:

raw AppSpec; plaintext secret; `EV[...]`; opaque provider secret; authorization header; raw HTTP request/response; secret hash/fingerprint; secret values.

90 days is first-production operational evidence retention, **not** permanent AIEOS Audit SoR retention.

### 18. Canonical managed-spec fingerprint

Fingerprint only the managed App projection.

Replace every secret **VALUE** with a single constant structural sentinel before serialization.

Canonical form requires deterministic UTF-8 JSON, sorted keys, and compact serialization.

Hash with SHA-256.

Encrypted provider ciphertext equality is **NOT** semantic configuration equality.

### 19. Source CI

Future Infrastructure controller implementation must introduce credential-free offline CI:

```text
app-release-controller-validate
```

Required proof areas:

- strict contract parsing  
- endpoint/method allowlist  
- exact App targeting  
- create cardinality  
- secret-key-set drift  
- managed drift  
- allowed provider defaults  
- double-read stale-race detection  
- read-only retry policy  
- zero mutation retry  
- ambiguous result reconciliation  
- rollback state machine  
- secret/`EV` redaction  
- canonical fingerprint  
- receipt negative tests  
- no filesystem AppSpec persistence  
- dispatcher/worker secret isolation  
- fixed Environment / concurrency bindings  
- no self-hosted release runner  
- no `pull_request_target`  
- no `secrets: inherit`  
- no `doctl` production mutation  
- no OpenTofu App mutation  
- no automatic production trigger  

Ordinary PR CI receives **no** production secrets and performs **no** DigitalOcean live mutation.

Third-party Actions remain pinned by full commit SHA.

After later implementation validation, adding `app-release-controller-validate` as a required branch check is a **separate** repository-administration action.

### 20. Disposable validation

**WPI-AP-DP-TV01** is mandatory before production workflows/credentials/releases.

It must use disposable non-production Apps and dummy secrets only.

Release credential and cleanup credential remain separate; the production-like release credential must **not** require delete authority merely for test cleanup.

ADR-AIEOS-050 does **NOT** authorize TV01 execution.

### 21. Implementation sequence

Future gates remain separated:

| Gate | Scope |
|------|-------|
| **WPI-AP-DP-I01** | controller library/contracts/offline tests/credential-free CI source |
| **WPI-AP-DP-TV01** | separately authorized disposable live provider validation |
| **WPI-AP-DP-I02** | production workflow wrapper/static protections after TV01 PASS |

Then separate gates for:

- production GitHub Environments/secrets  
- DigitalOcean PAT issuance  
- Temporal runtime keys  
- dispatcher DB readiness  
- VPC readiness  
- OCI publication/provenance  
- production App bootstrap  

Do **not** combine these gates.

### 22. Explicit non-authorizations

ADR-AIEOS-050 does **NOT** authorize:

- controller source implementation  
- workflow implementation  
- GitHub Environment creation  
- GitHub secret creation  
- branch protection mutation  
- DigitalOcean PAT creation  
- Temporal API-key creation/revocation  
- production DB credential work  
- secret access  
- WPI-AP-DP-TV01  
- DigitalOcean live validation  
- production state access  
- OpenTofu production refresh/plan/apply  
- VPC creation/mutation  
- App creation/update/rollback/delete  
- DOCR mutation  
- OCI publication/promotion  
- runtime secret injection  
- deployment/restart  
- dispatcher execution  
- worker execution  
- workflow execution  
- any production/cloud mutation  

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A50-INV-01 | Controller implementation belongs in Infrastructure `tools/app_platform_release/`; not Backend package/OCI; not OpenTofu App ownership. |
| A50-INV-02 | Production runner is GitHub-hosted `ubuntu-24.04` only; self-hosted release runners are forbidden. |
| A50-INV-03 | Only two fixed `workflow_dispatch` production workflows; no push/PR/schedule/matrix/arbitrary target deployment. |
| A50-INV-04 | Dispatcher and worker GitHub Environments and secret families are strictly isolated. |
| A50-INV-05 | Per-App durable lease uses fixed concurrency groups with `cancel-in-progress: false`, `queue: single`, 60-minute timeout; ADR-049 double-read fence remains mandatory. |
| A50-INV-06 | Mutation uses direct DigitalOcean v2 REST via controlled `httpx` only; automatic mutation retries = zero. |
| A50-INV-07 | Closed typed mutation allowlist only (`CREATE` / `UPDATE` / `ROTATE_SECRET` / `ROLLBACK`); no generic DELETE/restart/console/arbitrary API. |
| A50-INV-08 | Four distinct PAT classes (dispatcher/worker × bootstrap/steady-state); ≤90-day lifetime; T-30 rotation target. |
| A50-INV-09 | Persistent evidence is sanitized `release-receipt.json` (90-day retention); never secret material / raw AppSpec / `EV[...]`. |
| A50-INV-10 | WPI-AP-DP-TV01 disposable validation is mandatory before production; ADR-050 does not authorize TV01 or production execution. |

---

## Architecture relationship

| ADR | Role |
|-----|------|
| ADR-AIEOS-048 | historical/base topology and OCI-delivery authority where not superseded |
| ADR-AIEOS-048R1 | **CURRENT** naming authority |
| ADR-AIEOS-048R2 | **CURRENT** App ownership-boundary authority (rejects OpenTofu `digitalocean_app`) |
| ADR-AIEOS-049 | **CURRENT** state-free deployment-plane behavior (WPI-AP-DP01) |
| ADR-AIEOS-050 | **CURRENT** release-controller implementation architecture (WPI-AP-DP02) |

---

## Consequences

### Positive

- Concrete, reviewable implementation-architecture contract for the ADR-049 controller without authorizing source or cloud mutation  
- Fixed workflow/Environment/concurrency/runner/HTTP/credential/evidence boundaries for later separated gates  
- Offline CI proof areas and disposable validation gate remain explicit before production wrappers  

### Negative / residual risk

- Controller source, workflows, credentials, TV01, and production bootstrap remain separately authorized  
- Provider scope posture must be revalidated immediately before any credential creation  
- Unresolved dispatcher operating numerics remain intentionally unfrozen  

### Explicit non-authorizations

See Decision §22. Architecture freeze ≠ implementation, live validation, credential issuance, or production execution.

---

## Status

**Frozen / Approved** — architecture source authority only.

WPI-AP-DP02 design = **DESIGN COMPLETE / FROZEN**.  
Implementation = **NOT AUTHORIZED**.  
WPI-AP-DP-TV01 = **REQUIRED / NOT AUTHORIZED**.  
Production App Platform deployment = **NOT AUTHORIZED**.
