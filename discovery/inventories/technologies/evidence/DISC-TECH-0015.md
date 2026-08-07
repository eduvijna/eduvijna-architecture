# Evidence Summary — DISC-TECH-0015

**Technology:** `pip`
**Source repository:** `eduvijna-api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** tool

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-pip-eduvijna-api |
| name | pip |
| category | tool |
| version_observed | UNKNOWN |
| usage_context | Python package installer used in Dockerfile to install requirements.txt. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__Dockerfile.txt` | RUN pip install -r requirements.txt. |
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__requirements.txt.txt` | requirements.txt dependency manifest present. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
