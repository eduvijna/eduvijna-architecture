# Evidence Summary — DISC-REP-0013

**Repository:** `eduvijna-ai/eduvijna`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Experimental

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | private |
| hosting_platform | github |
| primary_language | TypeScript |
| organisational_owner | eduvijna-ai |
| technical_owner | UNKNOWN |
| codeowners_present | false |
| ci_cd_present | false |
| readme_present | false |
| workflows | 404 |
| stack | React + Vite (package.json name vite-react-typescript-starter) |
| package_manager | npm (package-lock.json) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-root.json` | Root listing evidences package.json, package-lock.json, vite.config.ts, Dockerfile; no README.md; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna__package.json.txt` | Package name vite-react-typescript-starter; vite and @vitejs/plugin-react dependencies. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna__Dockerfile.txt` | Dockerfile present at repository root (content captured). |
| other | `https://github.com/eduvijna-ai/eduvijna` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Experimental — npm package name vite-react-typescript-starter indicates starter/scaffold.
- **OBS-02:** No README at repository root in collected listing.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.description`
- `repository_optional.monorepo`
