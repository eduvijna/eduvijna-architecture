# Evidence Summary — DISC-TECH-0044

**Technology:** `Kubernetes`
**Source repository:** `Eduvijna-Cloud-Infra-Deploy`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** platform

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-kubernetes-eduvijna-cloud-infra-deploy |
| name | Kubernetes |
| category | platform |
| version_observed | UNKNOWN |
| usage_context | Orchestration platform evidenced by Kubernetes YAML manifests and Helm charts. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| kubernetes | `filebrowser-cli-pod.yaml / patch-anythingllm.yaml / seed-env-pod.yaml (root listing)` | Kubernetes YAML filenames evidenced at repository root. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-root.json` | Root listing includes Kubernetes YAML files and helm/. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
