---
id: ADR-AIEOS-031
title: Production Authorization Kernel
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-031 — Production Authorization Kernel

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md) · [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) · [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md)

**Catalogue note:** Frozen / Approved is architecture status. It is not production deployment or mutation authorization. Asset-specific capabilities are frozen by [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md), not by rewriting this ADR.

---

## Context

After authentication yields `principal_id` only, AIEOS needs an embedded current-authorization kernel so JWT claims, historical events, and workflow history cannot act as business permission.

## Decision

- Exact capability-based current authorization.
- Binary decision model: **ALLOW** / **DENY**.
- Default **DENY**.
- Unknown capability is **DENY**.
- Wildcard capability is not an authorization shortcut.
- Token roles / scopes / permissions are not AIEOS current authority.
- Tenant, principal, membership, and grant **current** validity are evaluated.
- Current authority must be revalidated; authorization decisions are not cached as durable permission.
- Current tenant suspension, principal suspension, and grant revocation take effect.
- Authorization dependency failure is not transformed into ALLOW.
- Unavailable current authority fails closed with sanitized unavailable semantics (distinct from DENY).
- Authorization remains separate from authentication ([ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md)).
- Authorization remains separate from governance ([ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md)).
- No hidden `is_admin => allow` bypass.
- Capability catalog may grow only through governed domain additions.

## Binding invariants

```text
Bearer JWT
  → TrustedRequestIdentity(principal_id only)
  → requested tenant
  → CurrentTenantAccessAuthority
  → TrustedSecurityContext
  → current capability authorization
  → application/domain command
```

## Explicit non-goals / deferred decisions

- This ADR does not list Asset capabilities; those are [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md).
- External policy engines are not the AIEOS authorization authority.
- Control-plane grant APIs and production deployment remain independently unauthorized.
- Identity/tenant/membership inputs are owned by [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md). Historical ADR-AIEOS-023 remains frozen but unavailable; this ADR does not reconstruct that historical body.

## Consequences

- Domain command paths must invoke the kernel with an exact capability string.
- Governance NO and authorization DENY remain different concerns.

## Related ADRs

| ID | Relationship |
|----|----------------|
| ADR-AIEOS-023 | Historical Frozen / Approved Identity/Tenant/Security; original body unavailable |
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Canonical identity/tenant/membership inputs |
| [ADR-AIEOS-030](ADR-AIEOS-030-production-jwt-bearer.md) | Authentication only |
| [ADR-AIEOS-032](ADR-AIEOS-032-governance-adapter-foundation.md) | Governance ports |
| [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) | Asset capability catalog addition |
