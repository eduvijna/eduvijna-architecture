# Evidence Summary — DISC-REP-0002

**Repository:** `eduvijna/Eduvijna`  
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
| stack | Expo ~54 + React Native 0.81.5 + TypeScript (package.json) |
| package_manager | npm (package-lock.json) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-root.json` | Root listing evidences package.json, package-lock.json, app.json, eas.json, ARCHITECTURE.md, README.md; no .github; no CODEOWNERS. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-workflows.json` | gh API HTTP 404 for .github/workflows; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna__package.json.txt` | Dependencies include expo ~54.0.35, react-native 0.81.5, react, axios; name eduvijna. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna__CODEOWNERS.txt` | CODEOWNERS fetch returned HTTP 404 JSON; codeowners_present=false. |
| other | `https://github.com/eduvijna/Eduvijna` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Runtime — Expo mobile app with product-oriented package manifests.
- **OBS-02:** ARCHITECTURE.md and eas.json present at root (listing only); contents not fully captured in _raw.
- **OBS-03:** axios dependency present; target backend repository URL not evidenced in collected artefacts.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
