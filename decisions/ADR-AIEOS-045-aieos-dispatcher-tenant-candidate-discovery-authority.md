---
id: ADR-AIEOS-045
title: AIEOS Dispatcher Tenant-Candidate Discovery Authority
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: proposed
version: 1.0.0
created: 2026-08-22
last_updated: 2026-08-22
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-045 — AIEOS Dispatcher Tenant-Candidate Discovery Authority

**Status:** Proposed / Awaiting Chief Architect Freeze  
**Date:** 2026-08-22  
**Related:** [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md)

**Catalogue note:** Proposed is architecture deposition status only. It is **not** Frozen / Approved, **not** implementation authorization, **not** database migration authorization, **not** production deployment, and **not** production identity provisioning.

Evidence SHAs at deposit (read-only gate):

- Architecture `origin/main`: `e613f980a1f71fc373eca49b97ac1de4639babf5`
- Backend `origin/main`: `72fde2f3fe77e894406d2d624308d14c67be2b37`
- Infrastructure `origin/main`: `7fb9ca076e39ebef838344dc434f0c53b6b1d07c`

Implementation contracts referenced as evidence (not higher authority than frozen ADRs): GCI-I02 DATABASE-PRIVILEGE-CONTRACT, GCI-I07 workflow intents, GCI-I08 transactional outbox.

---

## Context

AIEOS production runtime separates independent workload kinds including `EVENT_DISPATCHER` and `WORKFLOW_DISPATCHER`. Existing dispatcher primitives are tenant-scoped:

- `ContentOutboxDispatcher.dispatch_once(tenant_id)`
- `ContentReviewStartDispatcher.dispatch_once(tenant_id)`
- `ContentReviewCommandDispatcher.dispatch_once(tenant_id)`

GCI-I08 explicitly states that dispatcher APIs are tenant-scoped and does **not** authorize BYPASSRLS for global tenant scanning. No production dispatcher daemon currently exists.

Before dispatcher daemon implementation, architecture must freeze **how** dispatchers discover which tenant(s) have eligible pending work while preserving:

- strict tenant isolation for claim / publish / deliver processing
- FORCE ROW LEVEL SECURITY
- least privilege
- no BYPASSRLS on dispatcher login identities
- no schema ownership for dispatcher login identities
- no implicit cross-tenant business-data read
- separate event and workflow dispatcher authorities
- transactional outbox and workflow intent semantics

Tenant directory state and pending-work queue state are different concerns. A tenant with no pending work must not be scanned merely because it exists.

---

## Decision

### Candidate source

**EVENT_DISPATCHER** candidates originate from eligible pending rows in:

```text
integration.outbox_messages
```

**WORKFLOW_DISPATCHER** candidates originate from eligible pending rows in:

```text
workflow.workflow_start_intents
workflow.workflow_command_intents
```

Candidate discovery **must not** use:

- all tenants
- all ACTIVE tenants
- `security.tenants` as the work queue
- client-supplied tenant lists as production authority

Static configured tenant lists or per-tenant dispatcher processes may exist only as **bootstrap or testing fallbacks**, not as the production architecture baseline.

### Distinct cross-tenant infrastructure authority

Cross-tenant candidate discovery is a **distinct infrastructure database authority**.

It is **not**:

- business capability authorization
- tenant membership authority
- API runtime authority
- dispatcher tenant-scoped processing authority
- migration authority
- schema ownership
- BYPASSRLS authority

Ordinary dispatcher login roles remain:

```text
LOGIN
NOSUPERUSER
NOBYPASSRLS
not schema owner
```

### Dedicated candidate-reader principle

Candidate discovery uses dedicated least-privilege **candidate-reader** identities, separate from schema owners, migrators, API runtime, dispatcher logins, and backup/restore authority.

Conceptual identities (production provisioning remains infrastructure/deployment work):

| Identity | Scope |
|----------|-------|
| `aieos_event_candidate_reader` | Event outbox candidate discovery only |
| `aieos_workflow_candidate_reader` | Workflow intent candidate discovery only |

Each candidate-reader identity is:

```text
NOLOGIN
NOSUPERUSER
NOBYPASSRLS
not schema owner
```

**No dispatcher login receives membership in a candidate-reader role.**

Schema-owner-owned candidate functions are **not** the production baseline. The schema owner is **not** the runtime candidate-reader authority.

### SECURITY DEFINER boundary

SECURITY DEFINER is an allowed implementation mechanism **only** under these constraints:

- function owner is the dedicated candidate-reader authority, **not** the schema owner
- owner is NOLOGIN
- owner is NOSUPERUSER
- owner is NOBYPASSRLS
- no dispatcher login can `SET ROLE` to candidate-reader
- dispatcher receives `EXECUTE` only
- `PUBLIC` `EXECUTE` revoked
- secure pinned `search_path`
- schema-qualified object references
- no dynamic SQL unless separately justified
- no payload returned
- function has no mutation authority
- function cannot alter tenant context
- function cannot disable RLS
- function cannot `SET row_security off`
- function cannot assume another privileged role

FORCE RLS remains enabled on underlying queue tables.

### RLS authority — caller vs definer

Any candidate-reader cross-tenant visibility must be explicit through **role-specific RLS policy** plus **minimum SQL privileges**. No hidden bypass.

| Plane | Authority |
|-------|-----------|
| **Caller authority** | Dispatcher LOGIN may `EXECUTE` candidate function only |
| **Definer authority** | Dedicated candidate-reader may inspect only the minimum queue columns necessary to derive candidate scheduling metadata |

Candidate-reader visibility is itself the controlled cross-tenant exception authorized by this ADR when frozen.

### Minimum data exposure

Candidate discovery may return only:

- `tenant_id`
- bounded scheduling metadata demonstrably required for fairness, such as earliest eligible `available_at`
- optional aggregate eligible count (implementation-evaluated)

Candidate discovery must **never** return:

- CloudEvent envelope
- event business payload
- workflow input
- workflow command payload
- Content payload
- Principal data
- Membership data
- capability grants
- credentials
- secrets

**Cross-tenant business payload visibility is forbidden.**

### Event lifecycle semantics

An outbox row committed in the authoritative transaction remains an **integration delivery obligation** even if the tenant later becomes `SUSPENDED` or `DISABLED`.

Therefore candidate discovery, claim, broker publication, bounded retry, quarantine, and controlled replay are **not** suppressed solely by later tenant state.

Reason:

- the event represents already-committed historical integration truth ([ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md))
- receiving an event is not authorization to act
- suppressing publication would make later tenant state rewrite committed integration history

Tenant deletion semantics are **not** introduced by this ADR.

### Workflow intent lifecycle semantics

A durable workflow-start or workflow-command intent already committed remains eligible for **infrastructure delivery** even if the tenant later becomes `SUSPENDED` or `DISABLED`.

Dispatcher delivery itself is **not** authorization for later sensitive effects. Current authority **must** be revalidated at the governed sensitive-effect / activity boundary under [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) and [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md).

Therefore tenant suspension or disablement alone is **not**:

- dispatcher quarantine
- permanent delivery failure
- automatic deletion
- authorization to continue sensitive effects

Temporal history and delivered intents remain non-authoritative for current business permission.

### Event / workflow separation

Event and workflow candidate discovery remain **separate authorities**.

Do **not** create one universal `aieos_dispatcher` authority.

| Workload | Candidate scope |
|----------|-----------------|
| EVENT | `integration.outbox_messages` only |
| WORKFLOW | workflow intent queues only |

The mechanisms may share an engineering pattern, but roles, functions, grants, and queue visibility remain distinct.

### Transaction boundary

Conceptual flow:

```text
candidate discovery
        ↓
select tenant_id
        ↓
begin tenant-processing transaction
        ↓
SET LOCAL aieos.tenant_id = tenant_id
        ↓
existing tenant-scoped claim / read / update
        ↓
commit / rollback
```

Candidate discovery itself does **not** establish persistent tenant context. Session-global tenant context is forbidden. Connection-pool reuse must not leak tenant context.

### Existing dispatch_once contract

Preserve:

- `ContentOutboxDispatcher.dispatch_once(tenant_id)`
- `ContentReviewStartDispatcher.dispatch_once(tenant_id)`
- `ContentReviewCommandDispatcher.dispatch_once(tenant_id)`

Candidate discovery supplies candidate tenant IDs to future daemon scheduling. It does **not** replace tenant-scoped dispatcher APIs.

### Fairness

Minimum architectural requirements only:

- bounded work per tenant per scheduling pass
- no noisy-tenant starvation
- retry availability respected
- no full-table hot polling loop
- index-supportable candidate lookup
- deterministic / bounded candidate batch size

Specific in-memory scheduling algorithms (for example round-robin) are engineering detail, not architecture.

### Candidate table rejected

A separate candidate SoR / table is **rejected** as baseline because it:

- duplicates existing queue state
- creates dual-write / crash-consistency risk
- risks a second execution-state machine

A later ADR would be required to introduce such a SoR.

### Security / audit

Candidate polling and dispatcher delivery are **infrastructure execution**. They are **not** `security.audit_records` mutations merely because a tenant ID is selected ([ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) is not broadened).

Required evidence remains:

- structured operational logs / traces
- dispatcher workload identity
- candidate invocation
- selected tenant
- claim outcome
- publish / delivery result
- retry / quarantine metadata
- broker ACK evidence where applicable

### Workload Principals

An AIEOS WORKLOAD Principal is **not** required merely to authenticate candidate database access. Database login identity and AIEOS Principal identity remain separate ([ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md)).

Future workload-principal use for provenance or business authority remains separately governed. Candidate enumeration does **not** invoke the Authorization Kernel.

### PostgreSQL semantics and required later proof

Architecture assumptions consistent with PostgreSQL 18:

- FORCE RLS subjects table owners to RLS
- superuser / BYPASSRLS remain explicit bypass mechanisms and are forbidden here
- role-targeted policies may grant bounded visibility
- SECURITY DEFINER executes with function-owner authority

Schema ownership itself is **not** the candidate-read mechanism.

Before any migration is accepted, implementation **must** prove the selected mechanism using disposable PostgreSQL 18, including:

- candidate-reader NOLOGIN / NOBYPASSRLS
- FORCE RLS remains active
- dispatcher cannot direct cross-tenant queue `SELECT`
- dispatcher cannot `SET ROLE` candidate-reader
- dispatcher can only `EXECUTE` function
- function returns allowed metadata only
- function cannot return payload columns
- tenant-scoped processing still fails closed without `SET LOCAL`
- no leakage after pooled transaction reuse

No PostgreSQL experiment is executed by this ADR deposition.

---

## Explicit rejections

This ADR rejects:

- dispatcher BYPASSRLS
- superuser dispatcher
- dispatcher schema ownership
- schema-owner-as-runtime-candidate-reader baseline
- global direct `SELECT DISTINCT tenant_id` by dispatcher login
- `security.tenants` directory scan as work discovery
- cross-tenant payload access
- API credentials reused by dispatcher
- migrator credentials reused by dispatcher
- persistent / session-global tenant context
- candidate-table second SoR
- tenant-state suppression of committed event facts
- treating delivered workflow intent as current authorization

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A45-INV-01 | EVENT candidates originate from eligible pending `integration.outbox_messages` only. |
| A45-INV-02 | WORKFLOW candidates originate from eligible pending workflow intent queues only. |
| A45-INV-03 | Candidate discovery is distinct infrastructure authority, not business authorization. |
| A45-INV-04 | Dispatcher login roles remain NOSUPERUSER / NOBYPASSRLS / not schema owner. |
| A45-INV-05 | Dedicated NOLOGIN candidate-reader identities own SECURITY DEFINER candidate functions. |
| A45-INV-06 | Schema owner is not the runtime candidate-reader authority. |
| A45-INV-07 | Dispatcher login receives EXECUTE only; no candidate-reader role membership. |
| A45-INV-08 | Candidate output exposes tenant_id and bounded scheduling metadata only. |
| A45-INV-09 | Cross-tenant business payload visibility is forbidden. |
| A45-INV-10 | Committed outbox facts remain deliverable regardless of later tenant suspension/disablement. |
| A45-INV-11 | Committed workflow intents remain infrastructure-deliverable; sensitive effects revalidate current authority. |
| A45-INV-12 | Event and workflow candidate authorities remain separate. |
| A45-INV-13 | Tenant-scoped `dispatch_once(tenant_id)` contract is preserved. |
| A45-INV-14 | Tenant processing requires transaction-local `SET LOCAL aieos.tenant_id`. |
| A45-INV-15 | Separate candidate SoR / table is rejected as baseline. |

---

## Explicit non-goals / deferred decisions

This ADR does **not** authorize or freeze:

- concrete SQL function signatures
- exact production role names beyond conceptual examples
- exact RLS SQL
- exact GRANT statements
- Alembic revision
- dispatcher daemon implementation
- polling intervals
- batch sizes
- App Platform sizing
- dispatcher runtime configuration
- scheduled / reconciliation ownership
- Asset backup scheduling
- production identity provisioning
- cloud deployment

Scheduled / reconciliation architecture remains separately architecture-gated.

---

## Consequences

- Dispatcher daemon implementation remains blocked until this ADR is Frozen / Approved and subsequent implementation / migration gates authorize work.
- Future database migration design must implement dedicated candidate-reader identities and prove PostgreSQL behavior before acceptance.
- GCI-I02 / privilege contract updates remain future infrastructure documentation work after freeze.
- Bootstrap static tenant lists may be used temporarily but do not satisfy production baseline alone.

---

## Related ADRs

| ID | Relationship |
|----|--------------|
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Tenant state; DB login ≠ AIEOS Principal |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | FORCE RLS; transaction-local tenant context |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | Outbox / committed integration truth |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Intent delivery vs activity revalidation |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Audit ≠ infrastructure polling |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Current authorization at sensitive effects |
| [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) | Governance ≠ authorization |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | Separate dispatch worker components |
