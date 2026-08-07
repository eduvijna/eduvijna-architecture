# Evidence Summary — DISC-TECH-0021

**Technology:** `psycopg2-binary`
**Source repository:** `eduvijna-api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** library

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-psycopg2-binary-eduvijna-api |
| name | psycopg2-binary |
| category | library |
| version_observed | UNKNOWN |
| usage_context | PostgreSQL database adapter listed in requirements.txt. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__requirements.txt.txt` | requirements.txt lists psycopg2-binary without a pinned version. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`

## Observations

- **OBS-01:** Adapter evidence only; PostgreSQL server instance not evidenced in this capture.
