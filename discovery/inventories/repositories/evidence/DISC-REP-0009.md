# Evidence Summary — DISC-REP-0009

**Repository:** `eduvijna-ai/Learning-Journey-App`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Experimental

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | private |
| hosting_platform | github |
| primary_language | JavaScript |
| organisational_owner | eduvijna-ai |
| technical_owner | UNKNOWN |
| codeowners_present | false |
| ci_cd_present | false |
| readme_present | true |
| workflows | 404 |
| stack | React Native ^0.72.17 (package.json) |
| package_manager | npm (package-lock.json) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App-root.json` | Root listing evidences package.json, package-lock.json, android/, ios/, README.md, BACKEND_* markdown files; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App-languages.json` | Languages: JavaScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App__package.json.txt` | react-native ^0.72.17, axios; name learning-journey-mobile. |
| other | `https://github.com/eduvijna-ai/Learning-Journey-App` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Experimental — Learning Journey mobile app with no CI evidenced.
- **OBS-02:** BACKEND_* markdown filenames suggest backend integration; contents not captured; target UNKNOWN.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
