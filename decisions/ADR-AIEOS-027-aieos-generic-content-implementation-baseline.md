---
id: ADR-AIEOS-027
title: AIEOS Generic Content Implementation Baseline
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-027 — AIEOS Generic Content Implementation Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-048](ADR-048-review-queue-owns-approval.md)

**Catalogue note:** Frozen / Approved is architecture status. It is not implemented, merged, production-authorized, or deployed. This ADR does not rewrite Teacher OS EBP-001.9 / `edu.contents` discovery.

---

## Context

AIEOS needs a clean-room Generic Content domain as the authoritative content System of Record. Legacy LMS/CMS content, Teacher OS mock persistence, search indexes, Temporal history, and NATS events must not become that SoR.

## Decision

### Authority map

| Concern | Authority |
|---------|-----------|
| Generic Content aggregate | `content.contents` |
| Immutable payload / version | `content.content_versions` |
| Approval / review | `content.review_decisions` |
| Publication history | `content.publications` |
| Binary assets | Asset/File domain ([ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md)) |
| Workflow execution | Temporal ([ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md)) |
| Event transport | NATS ([ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md)) |
| Search / vector / KG | Derived, non-authoritative |
| Legacy Content | Migration input only |

Domain-owned PostgreSQL schema: `content`. One authoritative Generic Content SoR.

### Identity and versioning

- Stable UUIDv7 Content ID.
- UUIDv7 ContentVersion ID.
- Explicit `BIGINT` aggregate revision.
- Monotonic business version.
- Business version ≠ aggregate revision.
- Initial linear immutable history.
- No branch / merge / CRDT baseline.

### Payload

- Content envelope plus schema-versioned typed payload.
- Validated structured JSONB.
- Binary assets referenced through `ResourceRef`.
- Code/contract-controlled schema registry.

### Security / tenancy

- Explicit `tenant_id`.
- Explicit `owner_principal_id`.
- Trusted security context establishes tenant.
- Client tenant identifier is not authority.
- PostgreSQL RLS as defense in depth.
- Transaction-local context.

### Stewardship (authoritative)

`DRAFT` · `GENERATED` · `IN_REVIEW` · `APPROVED` · `ARCHIVED`

Product lifecycle **may project**:

Draft · Generating · Generated · In Review · Approved · Published · Archived

- Generating is workflow/operation truth.
- Published is publication truth.

### AI

- No separate AI Content SoR.
- `HUMAN` / `AI` / `IMPORT` / `SYSTEM` all create `ContentVersion`.
- Governed, minimized, typed/allow-listed provenance.
- No secrets in provenance.
- Applicable AI-produced learner/parent-facing output cannot bypass human review.

### Review

- Exact-version `ReviewDecision`.
- `APPROVE` / `REQUEST_CHANGES` / `REJECT`.
- Immutable append-only review history.
- Approval of version N never approves N+1.
- Negative review requires a new version for resubmission.

### Publication

- Approved ≠ Published.
- Separate explicit publish command.
- Exact-version approval required to publish.
- Immutable publication history.
- Current published-version pointer.
- A newer unpublished current version may coexist with an older published version.
- Archive withdraws active publication without erasing history.

## Binding invariants — GCI hardening rules

These twelve promoted identifiers are frozen. They are not a substitute summary.

| ID | Name | Architecture meaning |
|----|------|----------------------|
| GCI-G01 | Immutable History / Purge Authority Separation | Committed immutable Content history is distinct from governed purge authority. |
| GCI-G02 | Same-Aggregate Version Lineage | ContentVersion lineage remains within the same Content aggregate. |
| GCI-G03 | Authoritative Schema Reader Retention | Authoritative historical schema readers/validators required to read retained immutable versions cannot be casually removed while those versions remain authoritative. |
| GCI-G04 | Transaction-Local RLS Context Isolation | Tenant RLS security context is transaction-local and must not leak through pooled connections. |
| GCI-G05 | Negative Review Requires a New Version | `REQUEST_CHANGES` / `REJECT` closes that immutable-version review cycle; resubmission requires a new ContentVersion. |
| GCI-G06 | Review Comment Data Governance | Review comments are governed data and are not an uncontrolled logging/free-text escape hatch. |
| GCI-G07 | Mandatory AI Generation Provenance | AI-origin versions require mandatory generation provenance. |
| GCI-G08 | Typed / Allow-Listed Provenance | Provenance is typed / allow-listed and excludes secret material. |
| GCI-G09 | Asset Reference Dual Validation | Asset references require both reference-contract validation and current governed Asset-use validation where required. |
| GCI-G10 | Archive Withdraws Active Publication | Archive withdraws active publication while preserving publication history. |
| GCI-G11 | Current Asset Governance Survives Publication | Publication does not permanently bless an Asset; current Asset governance continues to apply after publication. |
| GCI-G12 | Migration Source Identity and Digest Conflict Detection | Migration uses stable source identity + source digest/version and detects changed/conflicting source records. |

Approved ≠ Published remains in force (see Publication above). Archive remains distinct from governed purge; physical purge / retention / hold stay deferred.

Additional coordination:

- Current authorization is revalidated before sensitive Content effects.
- Content mutation + outbox intent + required audit intent are coordinated transactionally where required.
- Domain code never publishes directly to NATS.

### Frozen Content event family

The initial frozen Content event types are:

- `io.eduvijna.aieos.content.content.created.v1`
- `io.eduvijna.aieos.content.content.version_created.v1`
- `io.eduvijna.aieos.content.content.submitted_for_review.v1`
- `io.eduvijna.aieos.content.content.review_approved.v1`
- `io.eduvijna.aieos.content.content.review_changes_requested.v1`
- `io.eduvijna.aieos.content.content.review_rejected.v1`
- `io.eduvijna.aieos.content.content.published.v1`
- `io.eduvijna.aieos.content.content.archived.v1`

Those event **semantics** are frozen. Exact event payload schemas remain implementation-contract work. Events remain facts, not Content authority. Domain code does not publish directly to NATS.

This ADR does **not** invent Asset events.

## Explicit non-goals / deferred decisions

- Production Content mutation, migration, and deployment remain independently unauthorized.
- Physical purge / retention / legal hold are not frozen.
- Asset HTTP, Asset events, and production BlobStore provider are not frozen.
- This ADR does not authorize Teacher OS `edu.contents` / PostgREST work.

## Consequences

- Teacher OS Review Queue approval semantics ([ADR-048](ADR-048-review-queue-owns-approval.md)) remain product-facing; AIEOS Generic Content is the platform SoR for AIEOS Content, not a silent rewrite of EBP-001.9.
- Workflow and events cannot mint Content approval or publication truth.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | SoR / identity / UoW |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | API / outbox / NATS |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Workflow execution truth |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Required audit intent |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Asset ResourceRef boundary |
| [ADR-048](ADR-048-review-queue-owns-approval.md) | Teacher OS approval UX; Approved ≠ Published |
