---
id: ADR-AIEOS-036R1
title: Asset Security-Audit Resource Revision Semantics
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-036R1 — Asset Security-Audit Resource Revision Semantics

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)

**Catalogue note:** Separate refining record for [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md). **Not** a replacement, retirement, or generic new revision framework. [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) remains active. Asset remains NON_PRODUCTION. Production deployment is not authorized.

---

## Context

Asset `aggregate_revision` and `AssetRevisionNumber` are different concepts. Encoding aggregate revision in `ResourceRef.resource_revision`, or using `AssetRevisionId` as `ResourceRef.resource_id`, would corrupt audit identity semantics for Asset while Content audit already uses a different convention.

## Decision

### Primary Asset audit ResourceRef

- `resource_type` = exact typed Asset type already stored on the aggregate (`asset.image` / `asset.document` / `asset.audio` / `asset.video`)
- `resource_id` = AssetId
- `resource_revision` = **None**

Do **not** encode aggregate revision in `ResourceRef.resource_revision`.  
Do **not** use `AssetRevisionId` as `ResourceRef.resource_id`.

### Before / after

Audit `resource_revision_before` / `resource_revision_after` represent Asset **`aggregate_revision`**.  
They do **not** represent `AssetRevisionNumber`.

### Related pinned ResourceRef

Revision-specific Asset mutations carry **one** related pinned Asset ResourceRef:

- same Asset resource type
- same AssetId
- `resource_revision` = exact `AssetRevisionNumber`

Required for:

- `asset.revision.register`
- `asset.revision.activate`
- `asset.safety.pass`
- `asset.safety.fail`

Lifecycle and quarantine audits do not require that related pinned ref.

### Aggregate-revision before/after pairs

| Action | before | after |
|--------|--------|-------|
| Asset create | `None` | `0` |
| Asset revision register | N | N |
| Other eight Asset increment actions | N | N + 1 |

The eight increment actions are: activate; lifecycle withdraw / restore / delete; quarantine set / clear; safety pass / fail.

### Content

Existing Content audit semantics remain unchanged: Content primary `ResourceRef.resource_revision` equals `resource_revision_after`.

## Binding invariants

- 036 capability, command mapping, authorization-before-UoW, and same-transaction audit rules remain in force.
- This record refines encoding only; it does not add Asset HTTP, events, purge, provider, or production composition.

## Explicit non-goals / deferred decisions

Same open production boundaries as [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md).

## Consequences

- Consumers of audit evidence must read Asset identity from the unpinned primary ResourceRef and aggregate movement from before/after fields.
- Register audits remain N→N on aggregate revision, matching [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md).

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) | Baseline this record refines; remains active |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Security-audit authority |
| [ADR-AIEOS-035](ADR-AIEOS-035-aieos-asset-mutation-revision-activation-semantics.md) | Create / register / increment aggregate rules |
