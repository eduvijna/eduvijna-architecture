# Evidence Summary — DISC-REP-0012

**Repository:** `eduvijna-ai/Edu-Chrome-Extension`  
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
| stack | React + Vite Chrome extension (package.json + @types/chrome) |
| package_manager | npm and yarn (package-lock.json + yarn.lock) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension-root.json` | Root listing evidences package.json, package-lock.json, yarn.lock, vite.config.ts, README.md; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension-languages.json` | Languages: JavaScript slightly ahead of TypeScript. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension__package.json.txt` | react, vite, axios, @types/chrome; chrome-webstore-upload scripts; name eduvijna-chrome-extension. |
| other | `https://github.com/eduvijna-ai/Edu-Chrome-Extension` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Experimental — Chrome extension with no CI workflows evidenced.
- **OBS-02:** axios dependency evidenced; backend target UNKNOWN.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
