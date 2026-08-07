# Evidence Summary — DISC-REP-0008

**Repository:** `eduvijna-ai/Learning-Journey-Api`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Experimental

## Facts (evidenced)

| Field | Value |
|-------|-------|
| visibility | private |
| hosting_platform | github |
| primary_language | Python |
| organisational_owner | eduvijna-ai |
| technical_owner | UNKNOWN |
| codeowners_present | false |
| ci_cd_present | false |
| readme_present | true |
| workflows | 404 |
| stack | Python FastAPI (requirements.txt) |
| package_manager | pip (requirements.txt) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api-root.json` | Root listing evidences requirements.txt, README.md, app/, routers/; no Dockerfile; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api-languages.json` | Languages: Python dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api__requirements.txt.txt` | fastapi==0.104.1, uvicorn[standard]==0.24.0 and related deps. |
| other | `https://github.com/eduvijna-ai/Learning-Journey-Api` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Experimental — separate Learning Journey product with no CI workflows evidenced.
- **OBS-02:** Co-named with Learning-Journey-App; coupling not evidenced beyond naming.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
