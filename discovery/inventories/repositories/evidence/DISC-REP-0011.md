# Evidence Summary — DISC-REP-0011

**Repository:** `eduvijna-ai/voice-clone`  
**Discovered at:** 2026-08-07T14:00:00Z  
**Confidence:** HIGH  
**Classification (observation):** Research

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
| stack | TTS + torch + FastAPI (requirements.txt) |
| package_manager | pip (requirements.txt) |
| default_branch | main |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone-root.json` | Root listing evidences requirements.txt, README.md, LICENSE, CONTRIBUTING.md, deployment/, setup.py; no .github; no root Dockerfile. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone-languages.json` | Languages: Python dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone__requirements.txt.txt` | TTS, torch, fastapi, uvicorn and related research/API dependencies. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone__deployment_Dockerfile.txt` | deployment/Dockerfile for Telugu Voice Cloning API (multi-stage Python 3.10). |
| other | `https://github.com/eduvijna-ai/voice-clone` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

| other | `discovery/inventories/repositories/evidence/_raw/gh-repo-list-default-branches.txt` | gh repo list metadata evidences defaultBranchRef.name=main for this repository. |

## Observations

- **OBS-01:** Classification: Research — TTS/voice-cloning project (requirements headed Core TTS Framework; Dockerfile references Telugu Voice Cloning API).
- **OBS-02:** LICENSE and CONTRIBUTING.md present; SPDX identifier not read from LICENSE contents.

## Unknowns

- `repository.ownership.technical_owner`
- `repository_optional.license`
- `repository_optional.monorepo`
