---
id: AR-004-CHANGED
title: AR-004 Changed Files
owner: Enterprise Architecture Office
status: draft
version: 0.1.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-004 — Changed Files

Paths are repository-relative.

## Added

| Path | Change |
|------|--------|
| `discovery/schemas/common-metadata.schema.json` | Reusable metadata + structured evidence definitions |
| `reviews/review-packages/AR-004/SUMMARY.md` | Review summary |
| `reviews/review-packages/AR-004/TREE.txt` | Tree of enhancement scope |
| `reviews/review-packages/AR-004/CHANGED_FILES.md` | This file |
| `reviews/review-packages/AR-004/CHECKLIST.md` | Review checklist |
| `reviews/review-packages/AR-004/OPEN_QUESTIONS.md` | Open questions |

## Modified — rules and example

| Path | Change |
|------|--------|
| `discovery/VALIDATION_RULES.md` | Common metadata, structured evidence types, ID convention, ISO-8601 `discovered_at` |
| `discovery/examples/repository-example.yaml` | Demonstrates `metadata`, `evidence`, `confidence`, `source_repository`, `discovered_at` |

## Modified — templates (common metadata + structured evidence)

| Path |
|------|
| `discovery/templates/repository-template.yaml` |
| `discovery/templates/application-template.yaml` |
| `discovery/templates/technology-template.yaml` |
| `discovery/templates/database-template.yaml` |
| `discovery/templates/api-template.yaml` |
| `discovery/templates/ai-template.yaml` |
| `discovery/templates/infrastructure-template.yaml` |
| `discovery/templates/capability-template.yaml` |
| `discovery/templates/security-template.yaml` |
| `discovery/templates/technical-debt-template.yaml` |

## Modified — domain schemas (`$ref` common-metadata; no duplicated metadata)

| Path |
|------|
| `discovery/schemas/repository.schema.json` |
| `discovery/schemas/application.schema.json` |
| `discovery/schemas/technology.schema.json` |
| `discovery/schemas/database.schema.json` |
| `discovery/schemas/api.schema.json` |
| `discovery/schemas/ai.schema.json` |
| `discovery/schemas/infrastructure.schema.json` |
| `discovery/schemas/capability.schema.json` |
| `discovery/schemas/security.schema.json` |
| `discovery/schemas/technical-debt.schema.json` |

## Explicitly not changed

- Folder structure under `discovery/`
- Application / product repositories
- No discovery performed; no discovery reports generated
- Domain field models retained (identity / optional / unknown policy / observations / assumptions / reproducibility)
