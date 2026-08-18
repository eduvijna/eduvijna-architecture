---
id: ADR-AIEOS-023R1
title: AIEOS Identity, Tenant & Security Canonical Restatement
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-023R1 — AIEOS Identity, Tenant & Security Canonical Restatement

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md)

**Catalogue note:** Frozen / Approved is architecture status. It is not identity-control-plane implementation, authentication implementation, authorization-kernel implementation, production deployment, or mutation authorization.

Historical ADR-AIEOS-023 — Identity/Tenant/Security was previously Frozen / Approved, but its original canonical decision body is no longer recoverable. ADR-AIEOS-023R1 is a transparent new canonical restatement and does not claim to reproduce the lost original text.

---

## Context

AIEOS requires one stable identity, tenant, membership and security-context model that:

- separates external authentication from AIEOS business identity
- separates tenant membership from business capability authority
- supports human and workload actors
- defines current authority rather than historical authorization snapshots
- preserves effective-actor/delegation provenance
- establishes strict frontend/backend trust boundaries
- integrates with PostgreSQL tenant enforcement
- prevents implicit admin/wildcard authority

Historical ADR-AIEOS-023 was Frozen / Approved but its decision body is unavailable.

Later frozen ADRs established specific constraints:

- [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md)
- [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md)
- [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md)
- [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md)
- [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md)
- [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md)
- [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md)
- [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md)
- [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md)

ADR-AIEOS-023R1 restates the missing baseline consistently with those later frozen decisions.

---

## Decision

### Principal identity

AIEOS security-domain Principal is the stable identity used for authorization, provenance and audit.

Exact Principal kinds:

- `HUMAN`
- `WORKLOAD`

No third Principal kind is authorized.

Business-domain entities such as Teacher, Learner, Administrator profile, employee profile, or AI/product profile are not themselves authentication principals merely because they are domain records. Domain objects may reference PrincipalId.

Principal identity:

- UUIDv7
- globally stable
- not tenant-local

Exact Principal states:

- `ACTIVE`
- `SUSPENDED`
- `DISABLED`

Only `ACTIVE` Principals may obtain current business authority.

`SUSPENDED`: temporarily ineligible for current authority.

`DISABLED`: ineligible for current authority until an explicit governed identity control-plane action restores or replaces the identity.

Historical credential or token state cannot override Principal state.

### External authentication identity

External authentication subject ≠ AIEOS Principal.

Conceptual mapping:

```text
external issuer/provider + subject
        ↓
governed identity binding
        ↓
AIEOS PrincipalId
```

Authentication adapters may prove credentials and resolve them to an existing Principal.

External identity-provider claims such as roles, groups, scopes, permissions, tenant claims, or admin claims do **not** themselves become AIEOS business authority.

No identity-provider vendor is selected. Auth0, Keycloak, Entra, Okta, Cognito, Clerk, and equivalent products are **not** selected by this ADR.

Automatic just-in-time Principal creation is **not** part of this baseline. Provisioning and external-subject binding require a governed identity control-plane operation.

JWT Bearer authentication yields trusted principal identity only, as frozen by [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md).

### Tenant identity

Tenant is a stable UUIDv7 security boundary.

Exact Tenant states:

- `ACTIVE`
- `SUSPENDED`
- `DISABLED`

Only an `ACTIVE` Tenant may provide current business authority.

Client-provided tenant identity is requested execution context, **not** proof of tenant access.

A Principal may have memberships in multiple tenants.

Every tenant-scoped execution resolves one exact tenant and verifies current tenant access.

Tenant suspension removes current business authority within that tenant.

Long-running workflow and effect authority must respect current tenant suspension under [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) revalidation rules.

### Membership

Principal-to-Tenant Membership is a first-class security relationship.

Exact Membership states:

- `ACTIVE`
- `SUSPENDED`
- `REVOKED`

Current tenant access requires:

```text
Principal ACTIVE
+ Tenant ACTIVE
+ Membership ACTIVE
```

Membership does **not** itself grant a business capability.

Membership is **not** a role, a wildcard, a domain mutation capability, or an administrator bypass.

`REVOKED` Membership cannot be revived by an old JWT, old workflow history, old event, cached authorization result, old approval, or old session state.

Role vocabulary is **not** platform business authority. Product-facing role labels may exist separately, but cannot independently authorize AIEOS platform operations.

### Session boundary

Authenticated browser/user session ≠ tenant authority ≠ business capability authority.

No universal AIEOS Session SoR is selected by this ADR.

Explicitly deferred:

- browser session implementation
- refresh-token architecture
- cookie/token storage architecture
- identity-provider login UX
- session persistence
- session-control-plane APIs

Current authorization remains re-evaluable independently of session lifetime.

### Trusted request / security context

Conceptual derivation:

```text
credential
        ↓
authentication
        ↓
TrustedRequestIdentity(principal_id)
        ↓
requested tenant
        ↓
current Principal / Tenant / Membership validation
        ↓
TrustedSecurityContext(principal_id, tenant_id)
```

`TrustedRequestIdentity` remains aligned with [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md).

`TrustedSecurityContext` baseline contains:

- `principal_id`
- `tenant_id`

Effective actor / delegation provenance is modeled separately where delegation applies; it must not be accepted from untrusted client input.

Client-supplied principal ID, roles, capabilities, permissions, membership, `is_admin`, effective actor, delegation, or tenant authority are never trusted business authority merely because the client sends them.

### Frontend security context

Frontend may hold advisory presentation context such as:

- authenticated-principal display information
- selected/requested tenant
- navigation capability hints
- human-readable role/product labels

Frontend context is never authoritative.

Hidden UI ≠ security enforcement.  
Visible UI ≠ authorization grant.

Every protected backend effect re-evaluates current server-side authority.

### Authorization boundary

[ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) remains authoritative for Authorization Kernel mechanics.

ADR-AIEOS-023R1 establishes identity inputs and boundaries.

Conceptually:

```text
principal
+ tenant
+ current membership
+ exact governed capability
        ↓
Authorization Kernel
```

Adopt and refer to [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) for:

- ALLOW / DENY
- default DENY
- unknown capability DENY
- exact capabilities
- no wildcard authorization
- JWT roles/scopes not authority
- no hidden `is_admin` bypass
- current-state validation
- unavailable does not become ALLOW

No external policy engine is selected. OPA, Cedar, Cerbos, OpenFGA, Casbin, and equivalent products are **not** selected by this ADR.

### Workload identity

Automated actors that participate as AIEOS business/security actors use `WORKLOAD` Principals.

`WORKLOAD` Principal current authority requires the same fundamentals:

```text
ACTIVE Principal
+ ACTIVE Tenant access
+ ACTIVE Membership where tenant-scoped
+ exact governed capability
+ current Authorization Kernel decision
```

Infrastructure credential identity is not automatically AIEOS Principal identity.

Keep distinct:

- database runtime role
- migration role
- schema owner
- backup/restore role
- NATS credential
- OCI/cloud workload credential

from AIEOS PrincipalId.

A workload may have both infrastructure credentials and an AIEOS `WORKLOAD` Principal, but they are separate identity planes.

### Effective actor

`principal_id` = actual authenticated/calling AIEOS Principal.  
`effective_actor_id` = Principal on whose behalf a governed action is performed.

Default:

- `effective_actor_id` = `principal_id`
- `delegation_id` = None

They may differ only through a valid governed delegation mechanism.

Historical `effective_actor_id` is provenance. It is **not** reusable current authorization authority.

### Delegation

Delegation is architecturally defined but **not** implementation-authorized or production-enabled by this ADR.

A delegation must be:

- tenant-scoped
- from one Principal to another
- exact-capability bounded
- time-bounded
- revocable
- current-state validated
- single-hop / non-transitive
- auditable

Delegation cannot grant authority the delegator does not currently possess.

Delegation cannot create wildcard authority.

Delegation chaining/transitivity is prohibited.

Resource-scoped delegation is deferred.

ADR-AIEOS-023R1 does **not** introduce resource-scoped grants, ACLs, or per-resource permission tables.

Delegation loses authority when applicable on:

- expiry
- revocation
- delegator losing the delegated capability
- delegator becoming ineligible for current authority
- delegate suspension/disablement
- tenant suspension/disablement

Workflow history, event history, approval history or audit history that contains delegation provenance does not make the delegation current.

Live delegation implementation requires a separate bounded implementation authorization and corresponding Authorization Kernel extension.

### Privileged / break-glass access

There is **no** universal administrator bypass.

Break-glass is a separate governed privileged-access mechanism.

It is **not** `is_admin = true`, `role=admin`, JWT admin scope, `*`, `asset.*`, or equivalent wildcard bypass.

Any future break-glass grant must be:

- explicit
- reason-bearing
- time-bounded
- tenant/scoped
- exact-capability bounded
- independently auditable
- non-delegable
- revocable
- current-state validated

Break-glass cannot silently bypass RLS, mutation activation gates, security audit, domain invariants, or concurrency controls.

Historical break-glass authority does not persist as current authority.

Break-glass issuance, approval, and control-plane implementation is deferred and requires separate architecture/implementation authorization.

### Database tenant enforcement

[ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) remains authoritative for persistence mechanics.

Identity/security handoff:

```text
trusted server-side tenant context
        ↓
transaction-local database security context
        ↓
PostgreSQL RLS
```

Application-level current authorization remains the business-security boundary.

RLS is defense in depth.

Raw client tenant input must not become DB tenant context without server-side Principal/Tenant/Membership validation.

Preserve identity separation:

runtime DB identity ≠ migration identity ≠ schema owner ≠ backup/restore authority

No cross-tenant bypass is authorized.

### Current-authority rule

**Historical security context is provenance, not permission.**

None of the following is reusable current authority:

- JWT roles/scopes
- event authorization snapshot
- workflow history
- approval history
- audit record
- cached ALLOW
- previous Membership state
- previous Principal state
- previous Tenant state
- expired/revoked Delegation
- expired/revoked break-glass authority

Sensitive effects must use current authority.

This aligns explicitly with [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md), [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md), [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md), and [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md).

### Security / audit actor context

Where applicable, security-sensitive provenance distinguishes:

- `tenant_id`
- `principal_id`
- `effective_actor_id`
- `delegation_id`
- privileged-access provenance
- correlation context
- causation context

`principal_id` records the actual acting/calling Principal.

`effective_actor_id` records represented authority when delegation applies.

Audit provenance explains who acted, for whom, and under what governed authority context.

Audit evidence is **not** current authorization.

[ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) remains authoritative for security-audit architecture.

[ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) / [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) remain authoritative for Asset audit specifics.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A23R1-INV-01 | External authentication subject ≠ AIEOS Principal. |
| A23R1-INV-02 | Principal IDs are stable global UUIDv7 identities. |
| A23R1-INV-03 | Human and workload actors use explicit Principal kinds. |
| A23R1-INV-04 | Only ACTIVE Principals can hold current authority. |
| A23R1-INV-05 | Tenant selection ≠ tenant authority. |
| A23R1-INV-06 | Current tenant access requires active Principal + active Tenant + active Membership. |
| A23R1-INV-07 | Membership ≠ business capability. |
| A23R1-INV-08 | Client roles/scopes/permissions/admin flags ≠ business authority. |
| A23R1-INV-09 | Session lifetime ≠ authorization lifetime. |
| A23R1-INV-10 | Frontend security context is advisory, never authoritative. |
| A23R1-INV-11 | ADR-AIEOS-031 Authorization Kernel owns current capability authorization. |
| A23R1-INV-12 | No universal administrator/wildcard bypass. |
| A23R1-INV-13 | Effective actor defaults to actual Principal unless governed delegation exists. |
| A23R1-INV-14 | Delegation is explicit, capability-bounded, expiring, revocable and non-transitive. |
| A23R1-INV-15 | Break-glass is explicit, time-bounded, exact-capability scoped and non-persistent. |
| A23R1-INV-16 | Historical JWT/event/workflow/audit/approval state is not reusable current authority. |
| A23R1-INV-17 | Trusted tenant context is established before transaction-local RLS context. |
| A23R1-INV-18 | Security provenance records actual Principal separately from represented / effective actor where applicable. |

---

## Relationship to historical ADR-AIEOS-023

Historical ADR-AIEOS-023:

- Frozen / Approved
- Identity/Tenant/Security
- canonical body unavailable
- remains part of architectural history

ADR-AIEOS-023R1:

- NEW Frozen / Approved decision
- canonical restatement
- does not claim to reproduce lost original prose
- becomes usable canonical identity/tenant/security implementation baseline

Historical ADR-AIEOS-023 is **not** rejected. No fabricated historical ADR-AIEOS-023 file is created.

---

## Relationship to later ADRs

Later ADRs remain authoritative for their narrower scopes.

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | Data / Resource / SoR and RLS mechanics |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | Event/workload security and no reusable event authorization snapshots |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Workflow current-authority revalidation, delegation expiry, tenant suspension and break-glass non-persistence |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Security audit |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Production runtime/database identity separation and readiness |
| [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md) | JWT Bearer authentication and TrustedRequestIdentity |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Current Authorization Kernel mechanics |
| [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) | Governance remains separate from authorization |
| [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) | Asset-specific capabilities and authorization/audit |
| [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) | Asset audit ResourceRef/revision semantics |

---

## Explicit non-goals / deferred decisions

ADR-AIEOS-023R1 does **not** authorize:

- identity administration APIs
- Principal provisioning APIs
- Tenant administration APIs
- Membership administration APIs
- IdP vendor selection
- browser login implementation
- refresh-token implementation
- session-control-plane implementation
- SCIM
- federation
- social login
- JIT Principal creation
- workload credential provider
- delegation implementation
- break-glass implementation
- external policy engine
- new JWT behavior
- frontend authentication changes
- database schema changes
- backend code changes
- production mutation
- production migration
- production deployment

It also does **not** authorize:

- production BlobStore provider
- Asset HTTP
- Asset binary delivery
- Asset events/outbox
- Asset purge/retention/legal hold
- Asset production runtime composition
- PED-I03 Asset mutation activation

---

## Consequences

- Subsequent identity, membership, and security-context implementation must treat this ADR as the canonical baseline and must not invent a substitute historical ADR-AIEOS-023 body.
- [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md) and [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) remain authoritative for JWT Bearer authentication and Authorization Kernel mechanics.
- Deposition of this restatement does not by itself authorize identity-control-plane, delegation, break-glass, or production work.
