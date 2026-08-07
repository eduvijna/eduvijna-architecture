# Evidence Summary — DISC-REP-0004

**Repository:** `eduvijna-ai/eduvijna-api`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Runtime

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | private |
| hosting_platform | github |
| primary_language | Python |
| organisational_owner | eduvijna-ai |
| technical_owner | UNKNOWN |
| codeowners_present | false |
| ci_cd_present | true |
| readme_present | true |
| workflows | .github/workflows/deploy.yml |
| stack | Python FastAPI + uvicorn (requirements.txt, Dockerfile CMD) |
| package_manager | pip (requirements.txt) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api-root.json` | Root listing evidences Dockerfile, docker-compose.yml/.dev/.prod, requirements.txt, README.md, .github, FRONTEND_BACKEND_API_MAPPING.md; no CODEOWNERS. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api-languages.json` | Languages: Python dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api-workflows.json` | Workflow path .github/workflows/deploy.yml present; ci_cd_present=true. |
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__requirements.txt.txt` | Lists fastapi, uvicorn, and related Python dependencies. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__Dockerfile.txt` | Python 3.11-slim image; CMD uvicorn app.main:app on port 8000. |
| docker-compose | `docker-compose.yml (root listing)` | docker-compose.yml, docker-compose.dev.yml, docker-compose.prod.yml present at repository root. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__CODEOWNERS.txt` | CODEOWNERS fetch returned HTTP 404; codeowners_present=false. |
| other | `https://github.com/eduvijna-ai/eduvijna-api` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Runtime — primary API backend with deploy workflow and containerisation.
- **OBS-02:** FRONTEND_BACKEND_API_MAPPING.md exists at root (filename evidenced); content not captured; frontend targets UNKNOWN.
- **OBS-03:** Prior note claimed CODEOWNERS true; collected evidence shows absent CODEOWNERS and contents API 404.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
