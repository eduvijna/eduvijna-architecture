# Evidence Summary — DISC-TECH-0037

**Technology:** `npm`
**Source repository:** `eduvijna-web`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** tool

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-npm-eduvijna-web |
| name | npm |
| category | tool |
| version_observed | UNKNOWN |
| usage_context | Package manager evidenced by package-lock.json and Dockerfile npm ci. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web-root.json` | Root listing includes package-lock.json. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web__Dockerfile.txt` | Dockerfile runs npm ci and npm run build. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
