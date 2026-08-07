# Evidence Summary — DISC-TECH-0081

**Technology:** `npm`
**Source repository:** `eduvijna`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** tool

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-npm-eduvijna |
| name | npm |
| category | tool |
| version_observed | UNKNOWN |
| usage_context | Package manager evidenced by package-lock.json and Dockerfile npm install path. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-root.json` | Root listing includes package-lock.json. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna__Dockerfile.txt` | Dockerfile prefers npm ci when package-lock.json is present. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
