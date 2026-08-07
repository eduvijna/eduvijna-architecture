# Evidence Summary — DISC-TECH-0036

**Technology:** `Node.js`
**Source repository:** `eduvijna-web`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** runtime

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-node-js-eduvijna-web |
| name | Node.js |
| category | runtime |
| version_observed | 20 |
| usage_context | Container runtime base image node:20-alpine used for build and run stages. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web__Dockerfile.txt` | FROM node:20-alpine in deps, builder, and runner stages. |

## Unknowns

- `technology.support_status`
