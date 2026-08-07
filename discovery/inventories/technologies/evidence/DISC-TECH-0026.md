# Evidence Summary — DISC-TECH-0026

**Technology:** `npm`
**Source repository:** `Quiz-React`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** tool

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-npm-quiz-react |
| name | npm |
| category | tool |
| version_observed | UNKNOWN |
| usage_context | Package manager evidenced by package-lock.json (dual lockfile with yarn.lock). |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React-root.json` | Root listing includes package-lock.json and package.json. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`

## Observations

- **OBS-01:** Both package-lock.json and yarn.lock present; canonical installer not selected in this discovery.
