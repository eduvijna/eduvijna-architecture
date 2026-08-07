# Evidence Summary — DISC-REP-0010

**Repository:** `eduvijna-ai/eduvijna-app`  
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
| readme_present | true |
| workflows | 404 |
| stack | React Native 0.72.6 (package.json; not Expo) |
| package_manager | npm (package-lock.json) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app-root.json` | Root listing evidences package.json, package-lock.json, android/, app.json, README.md; no .github; no eas.json. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app__package.json.txt` | react-native 0.72.6, axios; name EduVijnaApp; no expo dependency. |
| other | `https://github.com/eduvijna-ai/eduvijna-app` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Experimental — older/parallel React Native app under eduvijna-ai.
- **OBS-02:** axios dependency evidenced; backend target UNKNOWN.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
