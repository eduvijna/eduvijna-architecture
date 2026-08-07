---
id: DISC-REP-001
title: Review Package — Discovery Wave 1 Repository Inventory
owner: Enterprise Architecture Office
status: draft
version: 0.1.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# DISC-REP-001 — Summary

## Subject

Discovery Wave 1 repository inventory for GitHub user `eduvijna` and organisation `eduvijna-ai`.

## Objective

Produce schema-valid repository discovery inventories for all repositories in scope (13), with evidence summaries, catalogue artefacts, and a dependency map limited to evidenced relationships.

## Scope

| In scope | Out of scope |
|----------|--------------|
| GitHub user `eduvijna` (3 repos) | Application repository code changes |
| GitHub org `eduvijna-ai` (10 repos) | Invented dependency links |
| Inventories under `discovery/inventories/repositories/` | Non-repository discovery domains |
| This review package | Org named `EduVijna` (not found) |

## Outcome

- **13** repositories inventoried (`DISC-REP-0001` … `DISC-REP-0013`)
- Evidence summaries, per-repo YAML entries, catalogue YAML/JSON/MD, dependency map
- Schema validation executed against `discovery/schemas/repository.schema.json`
- Cross-repo dependency edges: **none evidenced**

## Classification snapshot (observations)

| Classification | Repos |
|----------------|-------|
| Governance | eduvijna-architecture |
| Runtime | Eduvijna, Eduvijna-App, eduvijna-api, Quiz-React, eduvijna-web |
| Infrastructure | Eduvijna-Cloud-Infra-Deploy |
| Research | voice-clone |
| Experimental | Learning-Journey-Api, Learning-Journey-App, eduvijna-app, Edu-Chrome-Extension, eduvijna |

## Evidence caveats

- Several README/CODEOWNERS fetches in `_raw` are truncated or HTTP 404; facts prefer root listings and successfully parsed manifests.
- `default_branch` is `main` for all entries (evidenced by `gh repo list` defaultBranchRef; see `evidence/_raw/gh-repo-list-default-branches.txt`).
- Prior notes claiming CODEOWNERS for Eduvijna / eduvijna-api conflict with collected 404/root listings; inventories follow evidence (`codeowners_present: false`).
