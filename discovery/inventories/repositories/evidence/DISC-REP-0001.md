# Evidence Summary — DISC-REP-0001

**Repository:** `eduvijna/eduvijna-architecture`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Governance

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | public |
| hosting_platform | github |
| primary_language | UNKNOWN |
| organisational_owner | Enterprise Architecture Office |
| technical_owner | Enterprise Architecture Office |
| codeowners_present | true |
| ci_cd_present | false |
| readme_present | true |
| workflows | .github/workflows/.gitkeep |
| stack | Governance documentation repository (no application package manager evidenced) |
| package_manager | none evidenced |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__eduvijna-architecture-root.json` | GitHub root contents listing; evidences README.md, LICENSE, CONTRIBUTING.md, CODEOWNERS, .github; no package.json. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__eduvijna-architecture-languages.json` | GitHub languages API returned empty object; primary_language UNKNOWN. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna__eduvijna-architecture-workflows.json` | Workflows path list contains only .github/workflows/.gitkeep; ci_cd_present=false. |
| other | `CODEOWNERS` | CODEOWNERS present; maps paths to @eduvijna/enterprise-architecture-office. |
| readme | `README.md` | States EAO ownership and governance purpose. |
| other | `LICENSE` | Apache License Version 2.0 text; supports license Apache-2.0. |
| other | `https://github.com/eduvijna/eduvijna-architecture` | GitHub URL from gh discovery; hosting_platform=github; visibility=public. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Governance — EAO governance and discovery framework, not product runtime.
- **OBS-02:** Default branch not present in collected evidence; left UNKNOWN.

## Unknowns

- `repository_optional.monorepo`
