---
id: ADR-AIEOS-030
title: Production JWT Bearer
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-030 — Production JWT Bearer

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md)

**Catalogue note:** Frozen / Approved is architecture status. JWT Bearer authentication is not tenant authority, capability authorization, or production deployment authorization. Canonical title reconciliation: **Production JWT Bearer**.

---

## Context

AIEOS needs a production-facing request authentication mechanism that yields a trusted principal identity without smuggling roles, tenants, or capabilities from the token into business authority.

## Decision

- JWT Bearer is **request authentication**.
- Successful authentication yields trusted principal identity only.
- `TrustedRequestIdentity` contains `principal_id` only.
- Authentication ≠ tenant authority.
- Authentication ≠ capability / business authorization.
- Issuer is verified.
- Audience is verified.
- Signature / key material is verified against governed key configuration (for example a configured JWKS source).
- Accepted signing algorithms are a **governed configuration**, fail-closed; this ADR does not freeze a single algorithm as an eternal universal requirement, and it does not freeze a JWT library as architectural authorization technology.
- Authentication configuration is fail-closed.
- Key / JWKS / authenticator dependency unavailability fails closed.
- Token `roles` / `groups` / `scope` / `tenant` / `permission` claims are **not** AIEOS business authority.
- A bearer OpenAPI security declaration may describe the authentication contract; it does not grant authorization.
- Current business authority belongs to later Authorization Kernel / current-authority evaluation ([ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md)).

## Binding invariants

```text
Bearer JWT (verified)
        ↓
TrustedRequestIdentity(principal_id ONLY)
        ↓
requested tenant (not token-asserted tenant authority)
        ↓
current tenant access + Authorization Kernel (later)
```

Client headers asserting principal, role, admin, or capability are not authentication.

## Explicit non-goals / deferred decisions

- Principal provisioning, issuer-to-principal mapping tables, and identity SoR deposition for ADR-AIEOS-023 are out of this ADR.
- Production deployment remains independently unauthorized ([ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md)).

## Consequences

- Authorization adapters must not read JWT role/scope claims as grants.
- Authenticator library choice remains an implementation concern behind this authentication architecture.

## Related ADRs

| ID | Relationship |
|----|----------------|
| ADR-AIEOS-023 | Frozen Identity/Tenant/Security decision; canonical body **not** deposited in EA-SYNC-01A |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Fail-closed production configuration |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Current capability authorization |
