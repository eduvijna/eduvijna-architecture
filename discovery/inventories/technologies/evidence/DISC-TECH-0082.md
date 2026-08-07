# Evidence Summary — DISC-TECH-0082

**Technology:** `Node.js`
**Source repository:** `eduvijna`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** runtime

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-node-js-eduvijna |
| name | Node.js |
| category | runtime |
| version_observed | 20 |
| usage_context | Container runtime base image node:20-alpine. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna__Dockerfile.txt` | FROM node:20-alpine in builder and runner stages. |

## Unknowns

- `technology.support_status`
