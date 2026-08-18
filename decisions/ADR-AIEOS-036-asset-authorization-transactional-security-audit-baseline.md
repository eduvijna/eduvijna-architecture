---
id: ADR-AIEOS-036
title: Asset Authorization & Transactional Security Audit Baseline
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-036 — Asset Authorization & Transactional Security Audit Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) · [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md)

**Catalogue note:** Frozen / Approved is architecture status. Asset remains **NON_PRODUCTION**. This ADR does not authorize production deployment, mutation activation, provider selection, or HTTP. [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) refines ResourceRef semantics; it does not replace this ADR.

---

## Context

Asset mutations authorized by [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) still require exact capability authorization through the Authorization Kernel and same-transaction security-audit evidence for successful state-changing mutations.

## Decision

### Exact Asset capability vocabulary

- `asset.create`
- `asset.revision.register`
- `asset.revision.activate`
- `asset.lifecycle.manage`
- `asset.quarantine.manage`
- `asset.safety.decide`

No wildcard `asset.*`. No `*`. No role vocabulary. No JWT role/scope business authority. No owner bypass.

### Command → capability

| Command | Capability |
|---------|------------|
| `create_asset` | `asset.create` |
| `register_revision` | `asset.revision.register` |
| `activate_revision` | `asset.revision.activate` |
| `withdraw_asset` / `restore_asset` / `delete_asset` | `asset.lifecycle.manage` |
| `quarantine_asset` / `clear_quarantine` | `asset.quarantine.manage` |
| `mark_safety_passed` / `mark_safety_failed` | `asset.safety.decide` |

### Exact Asset SecurityAuditAction vocabulary

- `asset.create`
- `asset.revision.register`
- `asset.revision.activate`
- `asset.lifecycle.withdraw`
- `asset.lifecycle.restore`
- `asset.lifecycle.delete`
- `asset.quarantine.set`
- `asset.quarantine.clear`
- `asset.safety.pass`
- `asset.safety.fail`

### Authorization invariant

```text
trusted principal
  + tenant
  + exact capability
  → current Authorization Kernel
  → allow
  → mutation
```

Authorization occurs **before** the first Asset Unit of Work.

For activation, authorization occurs **before** candidate read / `BlobStore.inspect`.

DENY or authorization-unavailable:

- zero Asset UoW
- zero mutation
- zero `BlobStore.inspect` initiated by that mutation
- no successful mutation audit row

### Successful state-changing mutation

- Exactly one required `security.audit_records` row
- **Same** PostgreSQL transaction as the authoritative Asset mutation
- Audit insert failure rolls back the mutation

No second successful audit for create replay or revision-registration replay.

No successful mutation audit for:

- authorization failure
- authorization unavailable
- stale aggregate
- invalid transition
- identity conflict
- BlobStore missing / unavailable / integrity failure

Audit records PostgreSQL mutation truth. It does not assert physical bytes remain forever.

ResourceRef audit encoding is refined by [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md).

## Binding invariants

- Principal + tenant + exact capability; resource context is contextual only.
- Ownership is not an allow rule.
- [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) transition and concurrency rules remain in force.

## Explicit non-goals / forbidden by this ADR

- Asset events / outbox — **not frozen**
- Asset HTTP / binary delivery — **not frozen**
- Production BlobStore / cloud provider — **not selected / not authorized**
- Purge, `bytes_purged` mutation, deletion-evidence orchestration — **not frozen**
- Asset production runtime composition — **open**
- PED-I03 Asset mutation activation — **open**
- Asset schema-owner readiness — **open**
- Production migration / mutation / deployment — **not authorized**

## Consequences

- Capability catalog growth for Asset is this ADR, not a silent rewrite of [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md).
- Implementation remaining uncomposed does not weaken these invariants.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Audit baseline |
| [ADR-AIEOS-031](ADR-AIEOS-031-production-authorization-kernel.md) | Kernel decision model |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | Mutation commands being authorized |
| [ADR-AIEOS-036R1](ADR-AIEOS-036R1-asset-security-audit-resource-revision-semantics.md) | Refines audit ResourceRef; does not replace this ADR |
