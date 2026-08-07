# Evidence Summary — DISC-REP-0003

**Repository:** `eduvijna/Eduvijna-App`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Runtime

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | private |
| hosting_platform | github |
| primary_language | TypeScript |
| organisational_owner | eduvijna |
| technical_owner | UNKNOWN |
| codeowners_present | false |
| ci_cd_present | false |
| readme_present | true |
| workflows | 404 |
| stack | Expo ~52 + React Native 0.76.9 + TypeScript (package.json) |
| package_manager | npm (package-lock.json) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App-root.json` | Root listing evidences package.json, package-lock.json, app.json, eas.json, ARCHITECTURE.md, README.md; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App-workflows.json` | gh API HTTP 404 for workflows directory; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App__package.json.txt` | Dependencies include expo ~52.0.0, react-native 0.76.9, axios; name eduvijna. |
| other | `https://github.com/eduvijna/Eduvijna-App` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Runtime — Expo mobile app parallel to eduvijna/Eduvijna.
- **OBS-02:** Shares npm package name eduvijna with eduvijna/Eduvijna; relationship beyond naming not evidenced.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
