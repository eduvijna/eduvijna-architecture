# Evidence Summary — DISC-TECH-0020

**Technology:** `redis (Python client)`
**Source repository:** `eduvijna-api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** library

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-redis-python-client-eduvijna-api |
| name | redis (Python client) |
| category | library |
| version_observed | >=5.0.0 |
| usage_context | Python Redis client libraries listed in requirements.txt (redis, aioredis). |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__requirements.txt.txt` | requirements.txt lists redis>=5.0.0 and aioredis>=2.0.1. |

## Unknowns

- `technology.support_status`

## Observations

- **OBS-01:** Client library evidence only; Redis server deployment not evidenced in this capture.
