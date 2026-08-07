---
id: DISC-TECH-001
title: Review Package — Discovery Wave 1 Technology Inventory
owner: Enterprise Architecture Office
status: draft
version: 0.1.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# DISC-TECH-001 — Summary

## Subject

Discovery Wave 1 technology inventory derived from repository evidence for GitHub user `eduvijna` and organisation `eduvijna-ai`.

## Objective

Produce schema-valid technology discovery inventories for stack-defining technologies evidenced in Wave 1 repositories, with catalogues, version matrix, and evidence summaries. Facts only — no upgrades, comparisons, vulnerability flags, or replacement suggestions.

## Scope

| In scope | Out of scope |
|----------|--------------|
| Technologies evidenced in Wave 1 `_raw` manifests | Application repository code changes |
| Inventories under `discovery/inventories/technologies/` | Version comparison / upgrade recommendations |
| Dual lockfile recording (npm + yarn) | Vulnerability or support-lifecycle analysis |
| This review package | Non-technology discovery domains |

## Outcome

- **84** technology observations inventoried (`DISC-TECH-0001` … `DISC-TECH-0084`)
- Per-observation YAML entries + evidence summaries
- Catalogue artefacts: YAML / JSON / MD, technology catalog, version matrix
- Schema validation against `discovery/schemas/technology.schema.json`
- `eduvijna-architecture`: no application technology manifests evidenced (coverage noted)

## Category vocabulary

Approved schema enum used: `language` | `framework` | `runtime` | `library` | `tool` | `platform` | `other` | `UNKNOWN`.

## Evidence caveats

- Unpinned requirements (e.g. eduvijna-api `fastapi`) recorded as `version_observed: UNKNOWN`.
- Dual lockfiles recorded as separate npm and yarn observations without selecting a canonical installer.
- Client libraries (redis, psycopg2) do not imply server deployment evidence.
- `support_status` is `UNKNOWN` wherever vendor lifecycle evidence was not captured.
