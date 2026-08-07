# Evidence Summary — DISC-REP-0005

**Repository:** `eduvijna-ai/Quiz-React`  
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
| stack | React + Vite (package.json and vite.config.ts at root) |
| package_manager | npm and yarn (package-lock.json + yarn.lock) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React-root.json` | Root listing evidences package.json, package-lock.json, yarn.lock, vite.config.ts, docker-compose.yml, Dockerfile.dev, Dockerfile.prod, .github, README.md; no root Dockerfile. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React-workflows.json` | Workflow path .github/workflows/deploy.yml; ci_cd_present=true. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React__package.json.txt` | vite, @vitejs/plugin-react, react, axios; name quiz-react. |
| dockerfile | `Dockerfile.dev / Dockerfile.prod (root listing)` | Container Dockerfiles present (dev/prod variants); root Dockerfile name not present. |
| docker-compose | `docker-compose.yml (root listing)` | docker-compose.yml plus docker-compose.dev.yml and docker-compose.prod.yml present. |
| other | `https://github.com/eduvijna-ai/Quiz-React` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Runtime — primary web quiz frontend with deploy workflow.
- **OBS-02:** Both yarn.lock and package-lock.json present; canonical installer not evidenced.
- **OBS-03:** axios dependency evidenced; specific backend repository link not captured.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
