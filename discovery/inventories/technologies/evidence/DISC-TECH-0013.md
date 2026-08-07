# Evidence Summary — DISC-TECH-0013

**Technology:** `FastAPI`
**Source repository:** `eduvijna-api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** framework

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-fastapi-eduvijna-api |
| name | FastAPI |
| category | framework |
| version_observed | UNKNOWN |
| usage_context | Web API framework listed in requirements.txt; uvicorn serves app.main:app. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__requirements.txt.txt` | requirements.txt lists fastapi without a pinned version. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__Dockerfile.txt` | CMD runs uvicorn app.main:app. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
