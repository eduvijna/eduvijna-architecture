# Discovery Wave 1 — Technology Inventory

**Review package:** DISC-TECH-001
**Discovered at:** 2026-08-07T16:00:00Z
**Scope:** Technologies evidenced across Wave 1 repositories (`eduvijna` user + `eduvijna-ai` org)
**Observation count:** 84

## Repository coverage

| Repository | Technology observations | Notes |
|------------|-------------------------|-------|
| `eduvijna-architecture` | 0 | No application technology manifests evidenced at repository root. |
| `Eduvijna` | 6 | Expo, Jest, React, React Native, TypeScript, npm |
| `Eduvijna-App` | 5 | Expo, React, React Native, TypeScript, npm |
| `eduvijna-api` | 11 | Docker, FastAPI, GitHub Actions, Python, SQLAlchemy, openai, pip, psycopg2-binary, pytest, redis (Python client), uvicorn |
| `Quiz-React` | 10 | Docker, Firebase, GitHub Actions, Playwright, React, TypeScript, Vite, Vitest, npm, yarn |
| `eduvijna-web` | 8 | Docker, GitHub Actions, Next.js, Node.js, React, Tailwind CSS, TypeScript, npm |
| `Eduvijna-Cloud-Infra-Deploy` | 5 | GitHub Actions, HCL, Helm, Kubernetes, Terraform |
| `Learning-Journey-Api` | 5 | FastAPI, Python, openai, pip, uvicorn |
| `Learning-Journey-App` | 6 | JavaScript, Jest, Node.js, React, React Native, npm |
| `eduvijna-app` | 6 | Jest, Node.js, React, React Native, TypeScript, npm |
| `voice-clone` | 9 | Docker, FastAPI, Python, TTS, pip, pytest, torch, transformers, uvicorn |
| `Edu-Chrome-Extension` | 6 | JavaScript, React, TypeScript, Vite, npm, yarn |
| `eduvijna` | 7 | Docker, Node.js, React, Tailwind CSS, TypeScript, Vite, npm |

## Observations catalogue

| ID | Technology | Category | Version | Repository | Confidence |
|----|------------|----------|---------|------------|------------|
| DISC-TECH-0001 | TypeScript | language | ^5.3.3 | `Eduvijna` | HIGH |
| DISC-TECH-0002 | Expo | framework | ~54.0.35 | `Eduvijna` | HIGH |
| DISC-TECH-0003 | React Native | framework | 0.81.5 | `Eduvijna` | HIGH |
| DISC-TECH-0004 | React | framework | 19.1.0 | `Eduvijna` | HIGH |
| DISC-TECH-0005 | npm | tool | UNKNOWN | `Eduvijna` | HIGH |
| DISC-TECH-0006 | Jest | tool | ^29.7.0 | `Eduvijna` | HIGH |
| DISC-TECH-0007 | TypeScript | language | ^5.3.3 | `Eduvijna-App` | HIGH |
| DISC-TECH-0008 | Expo | framework | ~52.0.0 | `Eduvijna-App` | HIGH |
| DISC-TECH-0009 | React Native | framework | 0.76.9 | `Eduvijna-App` | HIGH |
| DISC-TECH-0010 | React | framework | 18.3.1 | `Eduvijna-App` | HIGH |
| DISC-TECH-0011 | npm | tool | UNKNOWN | `Eduvijna-App` | HIGH |
| DISC-TECH-0012 | Python | language | 3.11 | `eduvijna-api` | HIGH |
| DISC-TECH-0013 | FastAPI | framework | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0014 | uvicorn | runtime | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0015 | pip | tool | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0016 | Docker | platform | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0017 | GitHub Actions | tool | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0018 | SQLAlchemy | library | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0019 | openai | library | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0020 | redis (Python client) | library | >=5.0.0 | `eduvijna-api` | HIGH |
| DISC-TECH-0021 | psycopg2-binary | library | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0022 | pytest | tool | UNKNOWN | `eduvijna-api` | HIGH |
| DISC-TECH-0023 | TypeScript | language | ~5.8.3 | `Quiz-React` | HIGH |
| DISC-TECH-0024 | React | framework | ^19.1.0 | `Quiz-React` | HIGH |
| DISC-TECH-0025 | Vite | tool | ^6.3.5 | `Quiz-React` | HIGH |
| DISC-TECH-0026 | npm | tool | UNKNOWN | `Quiz-React` | HIGH |
| DISC-TECH-0027 | yarn | tool | UNKNOWN | `Quiz-React` | HIGH |
| DISC-TECH-0028 | Docker | platform | UNKNOWN | `Quiz-React` | HIGH |
| DISC-TECH-0029 | GitHub Actions | tool | UNKNOWN | `Quiz-React` | HIGH |
| DISC-TECH-0030 | Vitest | tool | ^2.1.8 | `Quiz-React` | HIGH |
| DISC-TECH-0031 | Playwright | tool | ^1.51.0 | `Quiz-React` | HIGH |
| DISC-TECH-0032 | Firebase | library | ^12.8.0 | `Quiz-React` | HIGH |
| DISC-TECH-0033 | TypeScript | language | ^5 | `eduvijna-web` | HIGH |
| DISC-TECH-0034 | Next.js | framework | ^15.3.6 | `eduvijna-web` | HIGH |
| DISC-TECH-0035 | React | framework | ^19.2.1 | `eduvijna-web` | HIGH |
| DISC-TECH-0036 | Node.js | runtime | 20 | `eduvijna-web` | HIGH |
| DISC-TECH-0037 | npm | tool | UNKNOWN | `eduvijna-web` | HIGH |
| DISC-TECH-0038 | Docker | platform | UNKNOWN | `eduvijna-web` | HIGH |
| DISC-TECH-0039 | GitHub Actions | tool | UNKNOWN | `eduvijna-web` | HIGH |
| DISC-TECH-0040 | Tailwind CSS | library | ^3.4.1 | `eduvijna-web` | HIGH |
| DISC-TECH-0041 | HCL | language | UNKNOWN | `Eduvijna-Cloud-Infra-Deploy` | HIGH |
| DISC-TECH-0042 | Terraform | tool | UNKNOWN | `Eduvijna-Cloud-Infra-Deploy` | HIGH |
| DISC-TECH-0043 | Helm | tool | UNKNOWN | `Eduvijna-Cloud-Infra-Deploy` | HIGH |
| DISC-TECH-0044 | Kubernetes | platform | UNKNOWN | `Eduvijna-Cloud-Infra-Deploy` | HIGH |
| DISC-TECH-0045 | GitHub Actions | tool | UNKNOWN | `Eduvijna-Cloud-Infra-Deploy` | HIGH |
| DISC-TECH-0046 | Python | language | UNKNOWN | `Learning-Journey-Api` | HIGH |
| DISC-TECH-0047 | FastAPI | framework | 0.104.1 | `Learning-Journey-Api` | HIGH |
| DISC-TECH-0048 | uvicorn | runtime | 0.24.0 | `Learning-Journey-Api` | HIGH |
| DISC-TECH-0049 | pip | tool | UNKNOWN | `Learning-Journey-Api` | MEDIUM |
| DISC-TECH-0050 | openai | library | 1.3.5 | `Learning-Journey-Api` | HIGH |
| DISC-TECH-0051 | JavaScript | language | UNKNOWN | `Learning-Journey-App` | HIGH |
| DISC-TECH-0052 | React Native | framework | ^0.72.17 | `Learning-Journey-App` | HIGH |
| DISC-TECH-0053 | React | framework | 18.2.0 | `Learning-Journey-App` | HIGH |
| DISC-TECH-0054 | npm | tool | UNKNOWN | `Learning-Journey-App` | HIGH |
| DISC-TECH-0055 | Node.js | runtime | >=18 | `Learning-Journey-App` | HIGH |
| DISC-TECH-0056 | Jest | tool | ^29.5.0 | `Learning-Journey-App` | HIGH |
| DISC-TECH-0057 | TypeScript | language | 4.8.4 | `eduvijna-app` | HIGH |
| DISC-TECH-0058 | React Native | framework | 0.72.6 | `eduvijna-app` | HIGH |
| DISC-TECH-0059 | React | framework | 18.2.0 | `eduvijna-app` | HIGH |
| DISC-TECH-0060 | npm | tool | UNKNOWN | `eduvijna-app` | HIGH |
| DISC-TECH-0061 | Node.js | runtime | >=16 | `eduvijna-app` | HIGH |
| DISC-TECH-0062 | Jest | tool | ^29.2.1 | `eduvijna-app` | HIGH |
| DISC-TECH-0063 | Python | language | 3.10 | `voice-clone` | HIGH |
| DISC-TECH-0064 | FastAPI | framework | >=0.104.0 | `voice-clone` | HIGH |
| DISC-TECH-0065 | uvicorn | runtime | >=0.24.0 | `voice-clone` | HIGH |
| DISC-TECH-0066 | TTS | framework | >=0.22.0 | `voice-clone` | HIGH |
| DISC-TECH-0067 | torch | library | 2.1.0 | `voice-clone` | HIGH |
| DISC-TECH-0068 | transformers | library | 4.35.0 | `voice-clone` | HIGH |
| DISC-TECH-0069 | pip | tool | UNKNOWN | `voice-clone` | HIGH |
| DISC-TECH-0070 | Docker | platform | UNKNOWN | `voice-clone` | HIGH |
| DISC-TECH-0071 | pytest | tool | >=7.4.0 | `voice-clone` | HIGH |
| DISC-TECH-0072 | JavaScript | language | UNKNOWN | `Edu-Chrome-Extension` | HIGH |
| DISC-TECH-0073 | TypeScript | language | ~5.8.3 | `Edu-Chrome-Extension` | HIGH |
| DISC-TECH-0074 | React | framework | ^19.1.0 | `Edu-Chrome-Extension` | HIGH |
| DISC-TECH-0075 | Vite | tool | ^6.3.5 | `Edu-Chrome-Extension` | HIGH |
| DISC-TECH-0076 | npm | tool | UNKNOWN | `Edu-Chrome-Extension` | HIGH |
| DISC-TECH-0077 | yarn | tool | UNKNOWN | `Edu-Chrome-Extension` | HIGH |
| DISC-TECH-0078 | TypeScript | language | ^5.5.3 | `eduvijna` | HIGH |
| DISC-TECH-0079 | React | framework | ^18.3.1 | `eduvijna` | HIGH |
| DISC-TECH-0080 | Vite | tool | ^5.4.2 | `eduvijna` | HIGH |
| DISC-TECH-0081 | npm | tool | UNKNOWN | `eduvijna` | HIGH |
| DISC-TECH-0082 | Node.js | runtime | 20 | `eduvijna` | HIGH |
| DISC-TECH-0083 | Docker | platform | UNKNOWN | `eduvijna` | HIGH |
| DISC-TECH-0084 | Tailwind CSS | library | ^3.4.1 | `eduvijna` | HIGH |

## Schema

- Template: `discovery/templates/technology-template.yaml`
- Schema: `discovery/schemas/technology.schema.json`
- Categories (approved schema enum): language | framework | runtime | library | tool | platform | other | UNKNOWN
