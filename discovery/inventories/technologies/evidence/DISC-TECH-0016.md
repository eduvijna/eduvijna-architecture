# Evidence Summary — DISC-TECH-0016

**Technology:** `Docker`
**Source repository:** `eduvijna-api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** platform

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-docker-eduvijna-api |
| name | Docker |
| category | platform |
| version_observed | UNKNOWN |
| usage_context | Containerisation evidenced by Dockerfile and docker-compose files at repository root. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__Dockerfile.txt` | Dockerfile present and captured. |
| docker-compose | `docker-compose.yml (root listing)` | Root listing evidences docker-compose.yml/.dev/.prod. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api-root.json` | Root listing includes Dockerfile and docker-compose*.yml. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
