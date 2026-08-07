---
id: DISC-TECH-001-OQ
title: DISC-TECH-001 Open Questions
owner: Enterprise Architecture Office
status: draft
version: 0.1.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# DISC-TECH-001 — Open Questions

| # | Topic | Status | Notes |
|---|-------|--------|-------|
| 1 | Dual lockfiles (Quiz-React, Edu-Chrome-Extension) | **Recorded** | Both npm (`package-lock.json`) and yarn (`yarn.lock`) inventoried; canonical installer not selected. |
| 2 | Category vocabulary alignment | **Open** | Intake prompt listed extended categories (Package Manager, CI/CD, etc.). Approved schema enum is `language\|framework\|runtime\|library\|tool\|platform\|other\|UNKNOWN`. Inventories follow the schema. |
| 3 | Dependency depth | **Open** | Wave 1 technology discovery captures stack-defining technologies. Full transitive dependency inventories not authorised in this package. |
| 4 | Database/server runtime confirmation | **Deferred → DISC-DB-001 / DISC-INF-001** | Client libraries (psycopg2, redis) evidenced; server instances not evidenced here. |
| 5 | LLM/AI provider inventory depth | **Deferred → DISC-AI-001** | `openai` / TTS / torch recorded as technology facts; AI capability inventory is separate. |
| 6 | Terraform/Helm version pins | **Open** | `versions.tf` / chart version files not content-captured in Wave 1 `_raw`; versions remain UNKNOWN. |
| 7 | support_status evidence | **Open** | No vendor lifecycle artefacts captured; all `support_status` values are UNKNOWN. |
