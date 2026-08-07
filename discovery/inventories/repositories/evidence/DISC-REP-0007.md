# Evidence Summary — DISC-REP-0007

**Repository:** `eduvijna-ai/Eduvijna-Cloud-Infra-Deploy`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Infrastructure

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | private |
| hosting_platform | github |
| primary_language | HCL |
| organisational_owner | eduvijna-ai |
| technical_owner | UNKNOWN |
| codeowners_present | false |
| ci_cd_present | true |
| readme_present | false |
| workflows | .github/workflows/helm-deploy.yml, .github/workflows/terraform.yml |
| stack | Terraform (tf files) + Helm + Kubernetes YAML |
| package_manager | none evidenced |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-root.json` | Root listing evidences providers.tf, variables.tf, outputs.tf, versions.tf, modules/, helm/, kubernetes yaml files, .github; no README.md. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-languages.json` | Languages: HCL dominant, Go Template secondary. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-workflows.json` | Workflows helm-deploy.yml and terraform.yml; ci_cd_present=true. |
| terraform | `providers.tf / variables.tf / outputs.tf / versions.tf (root listing)` | Terraform configuration files present at repository root. |
| kubernetes | `helm/ and yaml at root (root listing)` | helm/ directory and Kubernetes YAML filenames evidenced. |
| other | `https://github.com/eduvijna-ai/Eduvijna-Cloud-Infra-Deploy` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Infrastructure — Terraform/Helm/Kubernetes deploy repository.
- **OBS-02:** No README at repository root in collected listing.
- **OBS-03:** Relationships from infra modules to application repositories not evidenced (module contents not captured).

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.description`
- `repository_optional.monorepo`
