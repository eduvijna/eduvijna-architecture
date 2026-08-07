# Repository Dependency Map — Discovery Wave 1

**Review package:** DISC-REP-001  
**Discovered at:** 2026-08-07T14:00:00Z  

Relationships are recorded **only where evidence exists** in Wave 1 artefacts. Presence of HTTP clients (e.g. axios) without a captured target URL or documented peer repository is **not** treated as an evidenced link.

## Summary

| From | To | Relationship | Evidence | Status |
|------|----|--------------|----------|--------|
| *(none)* | *(none)* | — | No structured cross-repository dependency with named peer was captured in `_raw` manifests/READMEs | **none evidenced** |

## Notes by repository

| Repository | Client library / doc signal | Peer repository | Status |
|------------|----------------------------|-----------------|--------|
| eduvijna/Eduvijna | axios in package.json | UNKNOWN | no evidenced peer |
| eduvijna/Eduvijna-App | axios in package.json | UNKNOWN | no evidenced peer |
| eduvijna-ai/eduvijna-api | `FRONTEND_BACKEND_API_MAPPING.md` filename at root | UNKNOWN (content not captured) | document present; targets UNKNOWN |
| eduvijna-ai/Quiz-React | axios in package.json | UNKNOWN | no evidenced peer |
| eduvijna-ai/eduvijna-web | no axios in package.json interest scan | none evidenced | none evidenced |
| eduvijna-ai/Learning-Journey-App | axios; BACKEND_* markdown filenames | UNKNOWN | UNKNOWN |
| eduvijna-ai/Learning-Journey-Api | FastAPI service | UNKNOWN | none evidenced |
| eduvijna-ai/eduvijna-app | axios in package.json | UNKNOWN | no evidenced peer |
| eduvijna-ai/Edu-Chrome-Extension | axios in package.json | UNKNOWN | no evidenced peer |
| eduvijna-ai/voice-clone | none evidenced | none evidenced | none evidenced |
| eduvijna-ai/eduvijna | none evidenced | none evidenced | none evidenced |
| eduvijna-ai/Eduvijna-Cloud-Infra-Deploy | Terraform/Helm deploy repo | UNKNOWN deploy targets | module contents not captured |
| eduvijna/eduvijna-architecture | governance only | none evidenced | none evidenced |

## Verdict

**No evidenced repository-to-repository dependency edges** were established in Wave 1. Further discovery should capture API base URLs, OpenAPI servers, compose service images, and the body of `FRONTEND_BACKEND_API_MAPPING.md` / BACKEND_* docs.
