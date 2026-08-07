# Discovery Wave 1 — Repository Inventory

**Review package:** DISC-REP-001  
**Discovered at:** 2026-08-07T14:00:00Z  
**Scope:** GitHub user `eduvijna` (3) + org `eduvijna-ai` (10) = **13 repositories**  
**Note:** No GitHub organisation named `EduVijna` was found.

## Catalogue

| ID | Repository | Visibility | Primary language | CI/CD | Classification | Confidence |
|----|------------|------------|------------------|-------|----------------|------------|
| DISC-REP-0001 | `eduvijna/eduvijna-architecture` | public | UNKNOWN | false | Governance | HIGH |
| DISC-REP-0002 | `eduvijna/Eduvijna` | private | TypeScript | false | Runtime | HIGH |
| DISC-REP-0003 | `eduvijna/Eduvijna-App` | private | TypeScript | false | Runtime | HIGH |
| DISC-REP-0004 | `eduvijna-ai/eduvijna-api` | private | Python | true | Runtime | HIGH |
| DISC-REP-0005 | `eduvijna-ai/Quiz-React` | private | TypeScript | true | Runtime | HIGH |
| DISC-REP-0006 | `eduvijna-ai/eduvijna-web` | private | TypeScript | true | Runtime | HIGH |
| DISC-REP-0007 | `eduvijna-ai/Eduvijna-Cloud-Infra-Deploy` | private | HCL | true | Infrastructure | HIGH |
| DISC-REP-0008 | `eduvijna-ai/Learning-Journey-Api` | private | Python | false | Experimental | HIGH |
| DISC-REP-0009 | `eduvijna-ai/Learning-Journey-App` | private | JavaScript | false | Experimental | HIGH |
| DISC-REP-0010 | `eduvijna-ai/eduvijna-app` | private | TypeScript | false | Experimental | HIGH |
| DISC-REP-0011 | `eduvijna-ai/voice-clone` | private | Python | false | Research | HIGH |
| DISC-REP-0012 | `eduvijna-ai/Edu-Chrome-Extension` | private | JavaScript | false | Experimental | HIGH |
| DISC-REP-0013 | `eduvijna-ai/eduvijna` | private | TypeScript | false | Experimental | HIGH |

## DISC-REP-0001 — `eduvijna/eduvijna-architecture`

- **Web:** https://github.com/eduvijna/eduvijna-architecture
- **Clone:** https://github.com/eduvijna/eduvijna-architecture.git
- **Ownership:** organisational=`Enterprise Architecture Office`; technical=`Enterprise Architecture Office`; CODEOWNERS=`true`
- **Stack:** Governance documentation repository (no application package manager evidenced)
- **Package manager:** none evidenced
- **Classification (observation):** Governance
- **discovery_view.relationships:** {'evidenced_dependencies': 'none evidenced', 'notes': 'No application dependency links evidenced.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__eduvijna-architecture-root.json` | GitHub root contents listing; evidences README.md, LICENSE, CONTRIBUTING.md, CODEOWNERS, .github; no package.json. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__eduvijna-architecture-languages.json` | GitHub languages API returned empty object; primary_language UNKNOWN. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna__eduvijna-architecture-workflows.json` | Workflows path list contains only .github/workflows/.gitkeep; ci_cd_present=false. |
| other | `CODEOWNERS` | CODEOWNERS present; maps paths to @eduvijna/enterprise-architecture-office. |
| readme | `README.md` | States EAO ownership and governance purpose. |
| other | `LICENSE` | Apache License Version 2.0 text; supports license Apache-2.0. |
| other | `https://github.com/eduvijna/eduvijna-architecture` | GitHub URL from gh discovery; hosting_platform=github; visibility=public. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0001.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0001.yaml`

## DISC-REP-0002 — `eduvijna/Eduvijna`

- **Web:** https://github.com/eduvijna/Eduvijna
- **Clone:** https://github.com/eduvijna/Eduvijna.git
- **Ownership:** organisational=`eduvijna`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** Expo ~54 + React Native 0.81.5 + TypeScript (package.json)
- **Package manager:** npm (package-lock.json)
- **Classification (observation):** Runtime
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'axios client dependency evidenced; no evidenced link to a specific backend repository.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-root.json` | Root listing evidences package.json, package-lock.json, app.json, eas.json, ARCHITECTURE.md, README.md; no .github; no CODEOWNERS. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-workflows.json` | gh API HTTP 404 for .github/workflows; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna__package.json.txt` | Dependencies include expo ~54.0.35, react-native 0.81.5, react, axios; name eduvijna. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna__CODEOWNERS.txt` | CODEOWNERS fetch returned HTTP 404 JSON; codeowners_present=false. |
| other | `https://github.com/eduvijna/Eduvijna` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0002.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0002.yaml`

## DISC-REP-0003 — `eduvijna/Eduvijna-App`

- **Web:** https://github.com/eduvijna/Eduvijna-App
- **Clone:** https://github.com/eduvijna/Eduvijna-App.git
- **Ownership:** organisational=`eduvijna`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** Expo ~52 + React Native 0.76.9 + TypeScript (package.json)
- **Package manager:** npm (package-lock.json)
- **Classification (observation):** Runtime
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'axios present; no evidenced backend repository URL.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App-root.json` | Root listing evidences package.json, package-lock.json, app.json, eas.json, ARCHITECTURE.md, README.md; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App-workflows.json` | gh API HTTP 404 for workflows directory; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-App__package.json.txt` | Dependencies include expo ~52.0.0, react-native 0.76.9, axios; name eduvijna. |
| other | `https://github.com/eduvijna/Eduvijna-App` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0003.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0003.yaml`

## DISC-REP-0004 — `eduvijna-ai/eduvijna-api`

- **Web:** https://github.com/eduvijna-ai/eduvijna-api
- **Clone:** https://github.com/eduvijna-ai/eduvijna-api.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** Python FastAPI + uvicorn (requirements.txt, Dockerfile CMD)
- **Package manager:** pip (requirements.txt)
- **Classification (observation):** Runtime
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'FRONTEND_BACKEND_API_MAPPING.md filename evidenced; mapping targets not captured.'}

### Evidence

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

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0004.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0004.yaml`

## DISC-REP-0005 — `eduvijna-ai/Quiz-React`

- **Web:** https://github.com/eduvijna-ai/Quiz-React
- **Clone:** https://github.com/eduvijna-ai/Quiz-React.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** React + Vite (package.json and vite.config.ts at root)
- **Package manager:** npm and yarn (package-lock.json + yarn.lock)
- **Classification (observation):** Runtime
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'axios client present; backend repository target UNKNOWN.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React-root.json` | Root listing evidences package.json, package-lock.json, yarn.lock, vite.config.ts, docker-compose.yml, Dockerfile.dev, Dockerfile.prod, .github, README.md; no root Dockerfile. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React-workflows.json` | Workflow path .github/workflows/deploy.yml; ci_cd_present=true. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Quiz-React__package.json.txt` | vite, @vitejs/plugin-react, react, axios; name quiz-react. |
| dockerfile | `Dockerfile.dev / Dockerfile.prod (root listing)` | Container Dockerfiles present (dev/prod variants); root Dockerfile name not present. |
| docker-compose | `docker-compose.yml (root listing)` | docker-compose.yml plus docker-compose.dev.yml and docker-compose.prod.yml present. |
| other | `https://github.com/eduvijna-ai/Quiz-React` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0005.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0005.yaml`

## DISC-REP-0006 — `eduvijna-ai/eduvijna-web`

- **Web:** https://github.com/eduvijna-ai/eduvijna-web
- **Clone:** https://github.com/eduvijna-ai/eduvijna-web.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** Next.js ^15.3.6 (package.json)
- **Package manager:** npm (package-lock.json)
- **Classification (observation):** Runtime
- **discovery_view.relationships:** {'evidenced_dependencies': 'none evidenced', 'notes': 'No cross-repository dependency facts captured.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web-root.json` | Root listing evidences package.json, package-lock.json, next.config.ts, Dockerfile, docker-compose.yml, README.md, .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web-workflows.json` | Workflow path .github/workflows/deploy.yml; ci_cd_present=true. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web__package.json.txt` | Dependency next ^15.3.6; package name nextn. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-web__Dockerfile.txt` | Dockerfile present at repository root (content captured). |
| docker-compose | `docker-compose.yml (root listing)` | docker-compose.yml present at repository root. |
| other | `https://github.com/eduvijna-ai/eduvijna-web` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0006.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0006.yaml`

## DISC-REP-0007 — `eduvijna-ai/Eduvijna-Cloud-Infra-Deploy`

- **Web:** https://github.com/eduvijna-ai/Eduvijna-Cloud-Infra-Deploy
- **Clone:** https://github.com/eduvijna-ai/Eduvijna-Cloud-Infra-Deploy.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** Terraform (tf files) + Helm + Kubernetes YAML
- **Package manager:** none evidenced
- **Classification (observation):** Infrastructure
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'Deploy targets for product repos not evidenced from collected root listing alone.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-root.json` | Root listing evidences providers.tf, variables.tf, outputs.tf, versions.tf, modules/, helm/, kubernetes yaml files, .github; no README.md. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-languages.json` | Languages: HCL dominant, Go Template secondary. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Eduvijna-Cloud-Infra-Deploy-workflows.json` | Workflows helm-deploy.yml and terraform.yml; ci_cd_present=true. |
| terraform | `providers.tf / variables.tf / outputs.tf / versions.tf (root listing)` | Terraform configuration files present at repository root. |
| kubernetes | `helm/ and yaml at root (root listing)` | helm/ directory and Kubernetes YAML filenames evidenced. |
| other | `https://github.com/eduvijna-ai/Eduvijna-Cloud-Infra-Deploy` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0007.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0007.yaml`

## DISC-REP-0008 — `eduvijna-ai/Learning-Journey-Api`

- **Web:** https://github.com/eduvijna-ai/Learning-Journey-Api
- **Clone:** https://github.com/eduvijna-ai/Learning-Journey-Api.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** Python FastAPI (requirements.txt)
- **Package manager:** pip (requirements.txt)
- **Classification (observation):** Experimental
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'Likely pairs with Learning-Journey-App by naming only; not evidenced.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api-root.json` | Root listing evidences requirements.txt, README.md, app/, routers/; no Dockerfile; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api-languages.json` | Languages: Python dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-Api__requirements.txt.txt` | fastapi==0.104.1, uvicorn[standard]==0.24.0 and related deps. |
| other | `https://github.com/eduvijna-ai/Learning-Journey-Api` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0008.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0008.yaml`

## DISC-REP-0009 — `eduvijna-ai/Learning-Journey-App`

- **Web:** https://github.com/eduvijna-ai/Learning-Journey-App
- **Clone:** https://github.com/eduvijna-ai/Learning-Journey-App.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** React Native ^0.72.17 (package.json)
- **Package manager:** npm (package-lock.json)
- **Classification (observation):** Experimental
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'axios present; BACKEND_* doc filenames evidenced without captured content.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App-root.json` | Root listing evidences package.json, package-lock.json, android/, ios/, README.md, BACKEND_* markdown files; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App-languages.json` | Languages: JavaScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Learning-Journey-App__package.json.txt` | react-native ^0.72.17, axios; name learning-journey-mobile. |
| other | `https://github.com/eduvijna-ai/Learning-Journey-App` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0009.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0009.yaml`

## DISC-REP-0010 — `eduvijna-ai/eduvijna-app`

- **Web:** https://github.com/eduvijna-ai/eduvijna-app
- **Clone:** https://github.com/eduvijna-ai/eduvijna-app.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** React Native 0.72.6 (package.json; not Expo)
- **Package manager:** npm (package-lock.json)
- **Classification (observation):** Experimental
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'axios present; no evidenced repository-to-repository link.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app-root.json` | Root listing evidences package.json, package-lock.json, android/, app.json, README.md; no .github; no eas.json. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-app__package.json.txt` | react-native 0.72.6, axios; name EduVijnaApp; no expo dependency. |
| other | `https://github.com/eduvijna-ai/eduvijna-app` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0010.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0010.yaml`

## DISC-REP-0011 — `eduvijna-ai/voice-clone`

- **Web:** https://github.com/eduvijna-ai/voice-clone
- **Clone:** https://github.com/eduvijna-ai/voice-clone.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** TTS + torch + FastAPI (requirements.txt)
- **Package manager:** pip (requirements.txt)
- **Classification (observation):** Research
- **discovery_view.relationships:** {'evidenced_dependencies': 'none evidenced', 'notes': 'No evidenced links to other EduVijna product repositories.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone-root.json` | Root listing evidences requirements.txt, README.md, LICENSE, CONTRIBUTING.md, deployment/, setup.py; no .github; no root Dockerfile. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone-languages.json` | Languages: Python dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone__requirements.txt.txt` | TTS, torch, fastapi, uvicorn and related research/API dependencies. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__voice-clone__deployment_Dockerfile.txt` | deployment/Dockerfile for Telugu Voice Cloning API (multi-stage Python 3.10). |
| other | `https://github.com/eduvijna-ai/voice-clone` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0011.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0011.yaml`

## DISC-REP-0012 — `eduvijna-ai/Edu-Chrome-Extension`

- **Web:** https://github.com/eduvijna-ai/Edu-Chrome-Extension
- **Clone:** https://github.com/eduvijna-ai/Edu-Chrome-Extension.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** React + Vite Chrome extension (package.json + @types/chrome)
- **Package manager:** npm and yarn (package-lock.json + yarn.lock)
- **Classification (observation):** Experimental
- **discovery_view.relationships:** {'evidenced_dependencies': 'UNKNOWN', 'notes': 'axios present; no evidenced repository link.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension-root.json` | Root listing evidences package.json, package-lock.json, yarn.lock, vite.config.ts, README.md; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension-languages.json` | Languages: JavaScript slightly ahead of TypeScript. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__Edu-Chrome-Extension__package.json.txt` | react, vite, axios, @types/chrome; chrome-webstore-upload scripts; name eduvijna-chrome-extension. |
| other | `https://github.com/eduvijna-ai/Edu-Chrome-Extension` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0012.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0012.yaml`

## DISC-REP-0013 — `eduvijna-ai/eduvijna`

- **Web:** https://github.com/eduvijna-ai/eduvijna
- **Clone:** https://github.com/eduvijna-ai/eduvijna.git
- **Ownership:** organisational=`eduvijna-ai`; technical=`UNKNOWN`; CODEOWNERS=`false`
- **Stack:** React + Vite (package.json name vite-react-typescript-starter)
- **Package manager:** npm (package-lock.json)
- **Classification (observation):** Experimental
- **discovery_view.relationships:** {'evidenced_dependencies': 'none evidenced', 'notes': 'No cross-repo dependency facts captured.'}

### Evidence

| Type | Location | Description |
|------|----------|-------------|
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-root.json` | Root listing evidences package.json, package-lock.json, vite.config.ts, Dockerfile; no README.md; no .github. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-languages.json` | Languages: TypeScript dominant. |
| github-workflow | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-workflows.json` | Workflows API HTTP 404; ci_cd_present=false. |
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna__package.json.txt` | Package name vite-react-typescript-starter; vite and @vitejs/plugin-react dependencies. |
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna__Dockerfile.txt` | Dockerfile present at repository root (content captured). |
| other | `https://github.com/eduvijna-ai/eduvijna` | GitHub URL from gh discovery; hosting_platform=github; visibility=private. |

Evidence detail: `discovery/inventories/repositories/evidence/DISC-REP-0013.md`  
Schema inventory: `discovery/inventories/repositories/entries/DISC-REP-0013.yaml`
