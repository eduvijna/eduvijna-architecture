# Evidence Summary — DISC-REP-0006

**Repository:** `eduvijna-ai/eduvijna-web`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Runtime

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | private |
| hosting_platform | github |
| primary_language | TypeScript |
| organisational_owner | eduvijna-ai |
| technical_owner | UNKNOWN |
| codeowners_present | false |
| ci_cd_present | true |
| readme_present | true |
| workflows | .github/workflows/deploy.yml |
| stack | Next.js ^15.3.6 (package.json) |
| package_manager | npm (package-lock.json) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web-root.json` | Root listing evidences package.json, package-lock.json, next.config.ts, Dockerfile, docker-compose.yml, README.md, .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web-workflows.json` | Workflow path .github/workflows/deploy.yml; ci_cd_present=true. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web__package.json.txt` | Dependency next ^15.3.6; package name nextn. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web__Dockerfile.txt` | Dockerfile present at repository root (content captured). |
| docker-compose | `docker-compose.yml (root listing)` | docker-compose.yml present at repository root. |
| other | `https://github.com/eduvijna-ai/eduvijna-web` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Runtime — Next.js web application with deploy workflow.
- **OBS-02:** No axios dependency evidenced in package.json interest scan; outbound API relationships UNKNOWN.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
