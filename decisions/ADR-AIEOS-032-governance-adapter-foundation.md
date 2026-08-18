---
id: ADR-AIEOS-032
title: Governance Adapter Foundation
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-032 — Governance Adapter Foundation

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md)

**Catalogue note:** Frozen / Approved is architecture status. Canonical title reconciliation: **Governance Adapter Foundation**. This ADR does not select a production storage provider and does not grant production composition.

---

## Context

Authorization answers whether a principal may perform a capability. Governance answers whether a proposed or current resource use is compliant with governed rules for that resource and purpose. Domains must depend on governance ports, not on platform storage or policy internals.

## Decision

- Governance ≠ authorization.
- Domains depend on governance ports / contracts, not platform implementations.
- Platform adapters implement **current** governance checks.
- Current governance outranks historical workflow / event snapshots.
- Unavailable or uncertain governance fails closed through governed unavailable semantics.
- No domain → platform inversion.
- `AssetUseAuthority` is the Asset current-use governance boundary.
- Content governance adapters may consume that boundary.
- A `ResourceRef` does not itself prove current usability.
- Adapter foundation does not select a production BlobStore / cloud storage provider.
- Adapter foundation does not grant production composition.

Approval ≠ governance. Schema validation ≠ governance. Asset existence ≠ permanent Asset governance approval.

## Binding invariants

| Condition | Architecture outcome |
|-----------|----------------------|
| Authoritative governance says NO | Business rejection of the governed use |
| Required governance cannot safely evaluate | Fail-closed unavailable semantics |
| Unexpected programming defect | Not relabeled as governance unavailable |

## Explicit non-goals / deferred decisions

- Concrete Asset current-use **decision vocabulary and precedence** are [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md).
- Asset SoR and BlobStore architecture are [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md).
- Implementation class names and versioned policy labels used in a later slice are evidence, not this ADR’s frozen types.
- Production BlobStore provider, Asset HTTP, Asset events, purge, and production deployment remain **not authorized / not frozen** as applicable.

## Consequences

- Content must not import Asset persistence or BlobStore internals to “check” an asset.
- Historical CloudEvents or Temporal history cannot be reused as current-use approval.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Capability authorization |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Asset identities / BlobStore boundary |
| [ADR-AIEOS-034](ADR-AIEOS-034-aieos-asset-current-use-authority-decision-semantics.md) | Current-use rejection vocabulary |
