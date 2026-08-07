---
id: AR-003
title: Review Package — Discovery Specification Framework
owner: Enterprise Architecture Office
status: draft
version: 0.2.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-003 — Summary

## Subject

Discovery Specification Framework under `discovery/` in `eduvijna-architecture`.

## Objective reviewed

Introduce reusable discovery templates, JSON schemas, validation rules, and a single Repository Discovery worked example — without analysing EduVijna application repositories or producing discovery reports.

## Scope of this package

| In scope | Out of scope |
|----------|--------------|
| Framework artefacts under `discovery/` | Application repository analysis |
| Templates, schemas, validation rules, example | Discovery reports / findings |
| Structure and conformance of the framework | Changes to product/application repos |
| This review package (`reviews/review-packages/AR-003/`) | Standards, ADRs, target-state architecture |

## Delivered artefacts

| Artefact | Status |
|----------|--------|
| `discovery/OVERVIEW.md` (framework orientation) | Present (modified) |
| `discovery/VALIDATION_RULES.md` | Present (new) |
| 10 YAML templates under `discovery/templates/` | Present (new) |
| 10 JSON schemas under `discovery/schemas/` | Present (new) |
| `discovery/examples/repository-example.yaml` | Present (new) |

## Design intent

- Inventories separate **facts**, **observations**, and **assumptions**.
- Every fact requires an **Evidence Source**.
- **Unknown is acceptable**; guessing is forbidden.
- Completed inventories must be **reproducible** and schema-validatable.
- Templates cover: repository, application, technology, database, API, AI, infrastructure, capability, security, technical debt.

## Risks / notes for reviewers

1. Empty template placeholders are not themselves schema-valid (schemas target *completed* inventories).
2. The repository example intentionally marks remote hosting fields `UNKNOWN` and uses only local-file evidence.
3. Process questions remain in `OPEN_QUESTIONS.md` (inventory storage root, programme ID format, validation tooling).

## Recommended review outcome paths

- **Approve** — framework complete against stated objective.
- **Approve with conditions** — address open process questions before Wave 1 inventories are captured.
- **Request changes** — adjust field model/enums if reviewers reject current shapes.

## Related paths

- Framework overview: `discovery/OVERVIEW.md`
- Validation rules: `discovery/VALIDATION_RULES.md`
- This package: `reviews/review-packages/AR-003/`
