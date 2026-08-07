# Discovery — Overview

## Purpose

Provide a controlled home for Enterprise Discovery programme outputs that establish current-state understanding of the EduVijna ecosystem.

This directory also hosts the **Discovery Specification Framework**: reusable templates, JSON schemas, and validation rules used to capture discovery inventories consistently. The framework defines *how* discovery facts are recorded; it does not itself contain analysed discovery reports for EduVijna applications.

## Scope

- Authorised discovery plans, inventories, assessments, and findings
- Traceable inputs that inform architecture, standards, roadmap, and decisions
- Discovery work tracked against authorised programmes and sprints
- Discovery Specification Framework (templates, schemas, validation rules, worked examples)

## Ownership

EduVijna Enterprise Architecture Office (EAO), in partnership with domain owners participating in discovery.

## Contents

| Path | Role |
|------|------|
| `OVERVIEW.md` | This overview |
| `VALIDATION_RULES.md` | Mandatory rules for discovery capture and validation |
| `templates/` | YAML inventory templates (one per discovery domain) |
| `schemas/` | JSON Schema definitions that validate completed inventories |
| `examples/` | Worked examples of correctly completed inventories |

### Discovery domains

| Domain | Template | Schema |
|--------|----------|--------|
| Repository | `templates/repository-template.yaml` | `schemas/repository.schema.json` |
| Application | `templates/application-template.yaml` | `schemas/application.schema.json` |
| Technology | `templates/technology-template.yaml` | `schemas/technology.schema.json` |
| Database | `templates/database-template.yaml` | `schemas/database.schema.json` |
| API | `templates/api-template.yaml` | `schemas/api.schema.json` |
| AI | `templates/ai-template.yaml` | `schemas/ai.schema.json` |
| Infrastructure | `templates/infrastructure-template.yaml` | `schemas/infrastructure.schema.json` |
| Capability | `templates/capability-template.yaml` | `schemas/capability.schema.json` |
| Security | `templates/security-template.yaml` | `schemas/security.schema.json` |
| Technical Debt | `templates/technical-debt-template.yaml` | `schemas/technical-debt.schema.json` |

## How to use

1. Confirm discovery work is authorised (programme / blueprint / intake).
2. Copy the relevant template from `templates/` into an authorised discovery working location.
3. Complete **required** fields with evidenced facts only; use `UNKNOWN` where evidence is missing (see Unknown Value Policy in each template).
4. Record **optional** fields only when evidence exists.
5. Populate **Evidence Source**, keep **Observations** separate from facts, and list **Assumptions** explicitly.
6. Validate the completed YAML against the matching schema in `schemas/`.
7. Follow `VALIDATION_RULES.md` before submitting for Architecture Review.

## Exclusions

- Unauthorised or informal discovery notes without EAO intake
- Target-state architecture designs (see `architecture/`)
- Standards, ADRs, and product backlogs
- Speculative inventories not authorised by a discovery programme
- Analysis of application repositories performed without authorised discovery intake
- Discovery *reports* produced outside an authorised programme sprint
