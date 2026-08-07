# Evidence Summary — DISC-TECH-0043

**Technology:** `Helm`
**Source repository:** `Eduvijna-Cloud-Infra-Deploy`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** tool

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-helm-eduvijna-cloud-infra-deploy |
| name | Helm |
| category | tool |
| version_observed | UNKNOWN |
| usage_context | Kubernetes package manager evidenced by helm/ directory and helm-deploy.yml workflow. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| kubernetes | `helm/ (root listing)` | helm/ directory present at repository root. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-root.json` | Root listing includes helm/. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-workflows.json` | Workflow .github/workflows/helm-deploy.yml present. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
