---
id: AR-004-CHECKLIST
title: AR-004 Review Checklist
owner: Enterprise Architecture Office
status: draft
version: 0.1.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-004 — Checklist

Architecture Review checklist for EBP-003 Discovery Framework Metadata Enhancement.

## A. Objective conformance

- [ ] Enhancement only (no framework redesign)
- [ ] No inventories created beyond the illustrative repository example update
- [ ] No EduVijna application repositories analysed or modified
- [ ] No discovery reports generated
- [ ] Folder structure unchanged

## B. Common metadata schema

- [ ] `discovery/schemas/common-metadata.schema.json` exists
- [ ] Defines `discovery_id`, `discovery_type`, `source_repository`, `discovered_at`, `discovered_by`, `confidence`, `version`
- [ ] `confidence` enum is `HIGH` \| `MEDIUM` \| `LOW`
- [ ] `discovered_at` enforces ISO-8601 timestamp form

## C. Structured evidence

- [ ] Free-form `evidence_sources` removed from templates/schemas
- [ ] `evidence` uses `{ type, location, description }`
- [ ] Supported evidence types match EBP-003 list (including `other`)

## D. ID convention

- [ ] `VALIDATION_RULES.md` documents `DISC-REP/APP/DB/API/INF/AI/SEC/CAP/TECH/DEBT-0001`
- [ ] Common schema pattern enforces the ID shape

## E. Templates

- [ ] All ten templates use shared `metadata` (no independent metadata blocks)
- [ ] All ten templates use structured `evidence`
- [ ] Domain sections otherwise preserved

## F. Domain schemas

- [ ] Every domain schema `$ref`s `common-metadata.schema.json` for metadata and evidence
- [ ] No duplicated metadata property definitions in domain schemas
- [ ] Domain `discovery_type` constrained to the matching const where applicable

## G. Example

- [ ] `repository-example.yaml` shows `metadata`, `evidence`, `confidence`, `source_repository`, `discovered_at`
- [ ] Example ID follows `DISC-REP-0001` convention
- [ ] Example remains illustrative (not a product discovery report)

## H. Package completeness at generation time

| Item | Result |
|------|--------|
| common-metadata.schema.json | Pass |
| Ten templates updated | Pass |
| Ten domain schemas $ref common metadata | Pass |
| VALIDATION_RULES ID + evidence updates | Pass |
| Repository example updated | Pass |
| No app-repo analysis / reports | Pass |

## Reviewer decision

- [ ] Approved
- [ ] Approved with conditions (list in review notes)
- [ ] Changes requested
- [ ] Deferred

Reviewer: _________________  Date: _____________
