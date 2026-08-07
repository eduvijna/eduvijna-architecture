# Evidence Summary — DISC-TECH-0042

**Technology:** `Terraform`
**Source repository:** `Eduvijna-Cloud-Infra-Deploy`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** tool

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-terraform-eduvijna-cloud-infra-deploy |
| name | Terraform |
| category | tool |
| version_observed | UNKNOWN |
| usage_context | Infrastructure as Code tool evidenced by *.tf files and terraform.yml workflow. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| terraform | `providers.tf / variables.tf / outputs.tf / versions.tf (root listing)` | Terraform configuration files present at repository root. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-root.json` | Root listing includes providers.tf, variables.tf, outputs.tf, versions.tf, modules/. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-workflows.json` | Workflow .github/workflows/terraform.yml present. |

## Unknowns

- `technology.version_observed`
- `technology.support_status`
