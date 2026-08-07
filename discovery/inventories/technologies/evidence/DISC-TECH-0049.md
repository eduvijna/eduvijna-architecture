# Evidence Summary — DISC-TECH-0049

**Technology:** `pip`
**Source repository:** `Learning-Journey-Api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** MEDIUM
**Category:** tool

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-pip-learning-journey-api |
| name | pip |
| category | tool |
| version_observed | UNKNOWN |
| usage_context | Python package manager implied by requirements.txt manifest (no alternate lockfile evidenced). |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api__requirements.txt.txt` | requirements.txt dependency manifest present. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api-root.json` | Root listing evidences requirements.txt; no poetry.lock/Pipfile evidenced. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
