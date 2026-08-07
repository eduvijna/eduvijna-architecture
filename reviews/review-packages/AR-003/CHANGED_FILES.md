---
id: AR-003-CHANGED
title: AR-003 Changed Files
owner: Enterprise Architecture Office
status: draft
version: 0.2.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-003 — Changed Files

Snapshot of files introduced or modified for the Discovery Specification Framework and this review package. Paths are repository-relative.

## Modified

| Path | Change |
|------|--------|
| `discovery/OVERVIEW.md` | Expanded to describe the Discovery Specification Framework, domain table, and usage steps |

## Added — framework

| Path | Change |
|------|--------|
| `discovery/VALIDATION_RULES.md` | Mandatory discovery validation rules |
| `discovery/templates/repository-template.yaml` | Repository discovery template |
| `discovery/templates/application-template.yaml` | Application discovery template |
| `discovery/templates/technology-template.yaml` | Technology discovery template |
| `discovery/templates/database-template.yaml` | Database discovery template |
| `discovery/templates/api-template.yaml` | API discovery template |
| `discovery/templates/ai-template.yaml` | AI discovery template |
| `discovery/templates/infrastructure-template.yaml` | Infrastructure discovery template |
| `discovery/templates/capability-template.yaml` | Capability discovery template |
| `discovery/templates/security-template.yaml` | Security discovery template |
| `discovery/templates/technical-debt-template.yaml` | Technical debt discovery template |
| `discovery/schemas/repository.schema.json` | JSON Schema for repository inventories |
| `discovery/schemas/application.schema.json` | JSON Schema for application inventories |
| `discovery/schemas/technology.schema.json` | JSON Schema for technology inventories |
| `discovery/schemas/database.schema.json` | JSON Schema for database inventories |
| `discovery/schemas/api.schema.json` | JSON Schema for API inventories |
| `discovery/schemas/ai.schema.json` | JSON Schema for AI inventories |
| `discovery/schemas/infrastructure.schema.json` | JSON Schema for infrastructure inventories |
| `discovery/schemas/capability.schema.json` | JSON Schema for capability inventories |
| `discovery/schemas/security.schema.json` | JSON Schema for security inventories |
| `discovery/schemas/technical-debt.schema.json` | JSON Schema for technical debt inventories |
| `discovery/examples/repository-example.yaml` | Worked Repository Discovery example |

## Added — this review package

| Path | Change |
|------|--------|
| `reviews/review-packages/AR-003/SUMMARY.md` | Review summary |
| `reviews/review-packages/AR-003/TREE.txt` | Tree of framework + package |
| `reviews/review-packages/AR-003/CHANGED_FILES.md` | This file |
| `reviews/review-packages/AR-003/CHECKLIST.md` | Review checklist |
| `reviews/review-packages/AR-003/OPEN_QUESTIONS.md` | Open questions |

## Explicitly not changed

- Application / product repositories (none modified)
- No discovery reports generated
- `architecture/`, `standards/`, `decisions/`, `blueprints/` untouched for this change set
