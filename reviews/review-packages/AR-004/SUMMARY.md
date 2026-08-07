---
id: AR-004
title: Review Package — EBP-003 Discovery Framework Metadata Enhancement
owner: Enterprise Architecture Office
status: draft
version: 0.1.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-004 — Summary

## Subject

EBP-003 — Discovery Framework Metadata Enhancement under `discovery/` in `eduvijna-architecture`.

## Objective reviewed

Standardise common inventory metadata and structured evidence across the Discovery Specification Framework without redesigning domains, creating inventories, or analysing repositories.

## Scope of this package

| In scope | Out of scope |
|----------|--------------|
| Common metadata schema | Framework redesign |
| Structured evidence model | Discovery inventories / reports |
| Template + domain schema updates | Application repository analysis or modification |
| Validation rule updates (ID convention, metadata, evidence) | Folder structure changes |
| Repository example update | New discovery domains |

## Changes delivered

| Change | Result |
|--------|--------|
| `discovery/schemas/common-metadata.schema.json` | Added — reusable `metadata` and `evidence` definitions |
| All 10 domain templates | Use shared `metadata` + structured `evidence` only |
| All 10 domain JSON schemas | `$ref` common-metadata; no duplicated metadata defs |
| `discovery/VALIDATION_RULES.md` | ID convention, evidence types, metadata/timestamp rules |
| `discovery/examples/repository-example.yaml` | Demonstrates metadata, evidence, confidence, source_repository, discovered_at |

## Design notes

- `metadata` fields: `discovery_id`, `discovery_type`, `source_repository`, `discovered_at`, `discovered_by`, `confidence` (`HIGH` \| `MEDIUM` \| `LOW`), `version`.
- `discovered_at` is ISO-8601 (e.g. `2026-08-07T14:00:00Z`).
- `evidence` is an array of `{ type, location, description }` with governed evidence types.
- Discovery IDs follow `DISC-{REP|APP|DB|API|INF|AI|SEC|CAP|TECH|DEBT}-0001`.
- Domain sections, unknown-value policy, observations, assumptions, and reproducibility are retained (not redesigned).

## Risks / notes for reviewers

1. Prior inventory shape (`discovery` / `evidence_sources`) is superseded; any draft inventories written against AR-003 shapes would need migration (none exist in this repo).
2. Evidence type `other` remains necessary for governance-repo artefacts (LICENSE, CODEOWNERS, manifests) until additional types are authorised.
3. Relative `$ref` to `common-metadata.schema.json` assumes validators resolve refs from `discovery/schemas/`.

## Recommended review outcome paths

- **Approve** — metadata/evidence standardisation complete against EBP-003.
- **Approve with conditions** — clarify open questions (single vs multi evidence cardinality already array; ID sequencing ownership).
- **Request changes** — adjust evidence type enum or ID pattern before Wave 1 capture.

## Related paths

- Common schema: `discovery/schemas/common-metadata.schema.json`
- Validation rules: `discovery/VALIDATION_RULES.md`
- Prior framework review: `reviews/review-packages/AR-003/`
- This package: `reviews/review-packages/AR-004/`
