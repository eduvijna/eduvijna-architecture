---
id: DISC-REP-001-OQ
title: DISC-REP-001 Open Questions
owner: Enterprise Architecture Office
status: resolved
version: 0.2.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# DISC-REP-001 — Open Questions (Resolved)

Decisions recorded for Wave 1 follow-up and Discovery Framework 1.1.0.

| # | Topic | Decision | Disposition |
|---|-------|----------|-------------|
| 1 | Default branch | Approve capturing `default_branch` via host API / `gh` in future (and current) discovery runs. | **Resolved — capture** |
| 2 | CODEOWNERS | Keep evidence-first approach. If the file is not found, record `codeowners_present: false`. Do not infer. | **Resolved — evidence-first** |
| 3 | Frontend↔API mapping | Not part of Repository Discovery. | **Deferred → DISC-API-001** |
| 4 | Learning Journey pairing | Not part of Repository Discovery. | **Deferred → DISC-CAP-001** |
| 5 | Infra deploy targets | Not part of Repository Discovery. | **Deferred → DISC-INF-001** |
| 6 | Visibility / full repo JSON retention | Optional hygiene for reproducibility; not blocking. Prefer retaining host metadata artefacts under `evidence/_raw/` when practical. | **Resolved — prefer retain** |
| 7 | Dual lockfiles | Not part of Repository Discovery. | **Deferred → DISC-TECH-001** |
| 8 | Classification governance | Approve the vocabulary and **freeze** it: Governance, Engineering, Platform, Runtime, Infrastructure, Research, Experimental, Archive (+ UNKNOWN). | **Resolved — frozen** |

## Related framework updates (1.1.0)

- Distinguish `metadata.discovery_id` (inventory) from `repository.repository_id` (asset).
- Require `lifecycle`, `criticality`, `classification`, and `metadata.inventory_version`.
- Document future relationship model: `{ target, confidence }` (not inventing edges in Wave 1).
