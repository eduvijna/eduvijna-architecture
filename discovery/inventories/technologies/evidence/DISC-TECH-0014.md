# Evidence Summary — DISC-TECH-0014

**Technology:** `uvicorn`
**Source repository:** `eduvijna-api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** runtime

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-uvicorn-eduvijna-api |
| name | uvicorn |
| category | runtime |
| version_observed | UNKNOWN |
| usage_context | ASGI server used as container CMD to run the FastAPI application. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__requirements.txt.txt` | requirements.txt lists uvicorn without a pinned version. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__Dockerfile.txt` | CMD [uvicorn, app.main:app, ...]. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
