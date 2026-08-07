# Evidence Summary — DISC-TECH-0041

**Technology:** `HCL`
**Source repository:** `Eduvijna-Cloud-Infra-Deploy`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** language

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-hcl-eduvijna-cloud-infra-deploy |
| name | HCL |
| category | language |
| version_observed | UNKNOWN |
| usage_context | Primary language for Terraform configuration files in the infra deploy repository. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-languages.json` | GitHub languages API lists HCL as dominant language. |
| terraform | `providers.tf / variables.tf / outputs.tf / versions.tf (root listing)` | Terraform HCL files present at repository root. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
