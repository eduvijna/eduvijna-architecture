---
id: ADR-AIEOS-051
title: AIEOS Backend Production OCI Build, Provenance & First-Publication Architecture
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-25
last_updated: 2026-08-25
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-051 — AIEOS Backend Production OCI Build, Provenance & First-Publication Architecture

**Status:** Frozen / Approved  
**Date:** 2026-08-25  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) · [ADR-AIEOS-048R1](ADR-AIEOS-048R1-aieos-app-platform-provider-compliant-naming.md) · [ADR-AIEOS-048R2](ADR-AIEOS-048R2-aieos-app-platform-runtime-ownership-boundary.md) · [ADR-AIEOS-049](ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md) · [ADR-AIEOS-050](ADR-AIEOS-050-aieos-app-platform-release-controller-implementation-architecture.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR freezes the **production Backend OCI build, provenance, and first-publication architecture**. Founder / Chief Architecture approval already granted the freeze and authorized proceeding to **WPI-OCI-I01** source implementation and offline validation only. This deposition does **not** authorize DigitalOcean registry publication, registry login, credential creation, App Platform mutation, TV01 App CREATE, or production deployment. Do **not** rewrite ADR-AIEOS-048 / 048R1 / 048R2 / 049 / 050 historical bodies.

**ID family note:** `ADR-AIEOS-051` is part of the AIEOS platform ADR family (`ADR-AIEOS-*`). It is distinct from Teacher OS ADR numbering.

---

## Governed baselines (deposition record)

```text
Architecture   origin/main = 26f9fb02cc522779b5f75456c12bc84354634edd
Infrastructure origin/main = 205e15bda0047b42aef1c4f67bb09fe4f156440a
Backend        origin/main = 8f4dd172e6a0ba8b4ad944b0ae22060442356342
```

---

## Context

[ADR-AIEOS-048](ADR-AIEOS-048-aieos-first-production-app-runtime-oci-delivery-contract.md) established that first-production WORKFLOW_DISPATCHER and TEMPORAL_WORKER consume **one common Backend OCI image** under `eduvijna-registry` / `aieos-backend`, with production authority bound to an **immutable manifest digest** (not `latest` / tags alone).

[ADR-AIEOS-049](ADR-AIEOS-049-aieos-app-platform-state-free-deployment-plane.md) / [ADR-AIEOS-050](ADR-AIEOS-050-aieos-app-platform-release-controller-implementation-architecture.md) froze App Platform release-controller behavior and implementation architecture, including OCI digest authority for App CREATE/UPDATE.

WPI-AP-DP-TV01 remains authorized for disposable live validation but is **PAUSED** at its OCI digest gate because no provenance-bound production `aieos-backend` manifest digest yet exists.

This ADR freezes the Backend **production OCI build / provenance / first-publication** architecture so WPI-OCI-I01 (source + offline CI) and a later separately authorized WPI-OCI-P01 (live first publication) remain separated and reviewable.

Exact production Dockerfile / provenance tooling / publication credentials do **not** exist yet in Backend as authorized outcomes of this Architecture deposition alone. WPI-OCI-I01 is the next source/offline gate after this ADR merges.

---

## Decision

### 1. Artifact role

The first-production AIEOS Backend runtime artifact is:

```text
ONE common Backend OCI image
```

used by both:

- WORKFLOW_DISPATCHER  
- TEMPORAL_WORKER  

Published under the existing DigitalOcean Container Registry:

```text
registry   = eduvijna-registry
repository = aieos-backend
```

Both workloads consume the **SAME immutable manifest digest**.

They remain **separate** App Platform Apps with distinct run commands, configuration, and secret boundaries under ADR-AIEOS-048 / 049 / 050.

**Do not** include API runtime or EVENT dispatcher in the first-production OCI workload slice.

### 2. Backend source authority

Production OCI build source is:

```text
eduvijna-aieos-backend
```

An artifact is eligible only when built from an **exact governed clean Backend Git SHA**.

Initial governed candidate source:

```text
8f4dd172e6a0ba8b4ad944b0ae22060442356342
```

Source identity must be embedded in OCI metadata and provenance.

**Dirty-tree production artifacts are forbidden.**

### 3. Production Dockerfile

Future implementation source:

```text
deploy/oci/Dockerfile.backend-runtime
```

The existing:

```text
deploy/oci/Dockerfile.api-runtime-probe
```

remains **NON_PRODUCTION_RUNTIME_PROBE** and **MUST NOT** be promoted, renamed, or treated as the production Backend image.

Production Dockerfile requirements:

| Requirement | Frozen posture |
|-------------|----------------|
| Python | 3.14.7 |
| uv | 0.12.4 |
| Lockfile | committed `uv.lock` |
| Dependencies | production only |
| Base image | immutable digest-pinned |
| Platform (first production) | `linux/amd64` |
| Runtime user | non-root |
| UID/GID | 10001 posture |
| Secrets / runtime credentials in image/layers | **FORBIDDEN** |
| Mutable package-resolution behavior | **FORBIDDEN** |
| `.git` metadata in image | **FORBIDDEN** |
| Development/test deps in final image | **FORBIDDEN** |

Exact Dockerfile content is **NOT** authorized by this Architecture deposition; WPI-OCI-I01 owns source implementation after merge.

### 4. Entrypoint model

The image contains both governed runtime modules:

```text
aieos.platform.runtime.entrypoints.workflow_dispatcher_main
aieos.platform.runtime.entrypoints.temporal_worker_main
```

App Platform supplies the workload-specific run command.

The image **MUST NOT** silently choose one of the two production workloads when no governed App run command is supplied.

Default image command must therefore be **fail-closed / non-serving** and must **not** accidentally start:

- workflow dispatcher  
- Temporal worker  
- API server  
- EVENT dispatcher  

### 5. OCI labels / identity

Required OCI metadata includes at minimum:

```text
org.opencontainers.image.title
org.opencontainers.image.description
org.opencontainers.image.version
org.opencontainers.image.source
org.opencontainers.image.revision
```

plus EduVijna classification/provenance labels sufficient to prove:

- artifact = AIEOS Backend runtime  
- source Git SHA  
- application version  
- production-runtime artifact classification  

The **revision** value must equal the exact governed Backend SHA.

### 6. Pre-publication build validation

Before any future publication, offline / credential-free validation must prove at minimum:

- source SHA exactness  
- clean source  
- lockfile current  
- Python = 3.14.7  
- uv = 0.12.4  
- production Dockerfile parses/builds  
- exact expected build platform  
- non-root runtime  
- source revision label exact  
- application version exact  
- both required worker modules present/importable  
- no runtime secret required during build  
- no production credential present  
- image is not the old NON_PRODUCTION runtime probe  
- default command does not start a production workload  
- no mutable deployment authority such as `latest`  

**WPI-OCI-I01** adds source / offline CI only.

Ordinary PR CI receives **no** DigitalOcean or registry credential.

### 7. Provenance receipt

Freeze a sanitized OCI provenance receipt model.

**Before publication** it may include:

- Backend Git SHA  
- Architecture SHA  
- Infrastructure SHA  
- application version  
- Python version  
- uv version  
- build platform  
- Dockerfile SHA-256  
- `uv.lock` SHA-256  
- immutable base-image digest  
- build/CI identity  
- OCI config/label identity  
- source cleanliness result  
- validation result  

**After a separately authorized live publication** it additionally records:

- registry = `eduvijna-registry`  
- repository = `aieos-backend`  
- source-SHA tag  
- immutable manifest digest  
- provider/request identity where non-secret  
- publication/reconciliation classification  

Receipt **MUST NOT** contain:

- registry credential  
- PAT  
- Docker auth material  
- Authorization header  
- production runtime secret  
- token hash  
- secret fingerprint  

### 8. First-publication tag / digest authority

First publication uses a convenience source tag:

```text
git-<FULL_BACKEND_GIT_SHA>
```

For the initial candidate:

```text
git-8f4dd172e6a0ba8b4ad944b0ae22060442356342
```

The tag itself is **NOT** deployment authority.

Production / runtime authority is only:

```text
sha256:<64 lowercase hex manifest digest>
```

after provider read-back and provenance reconciliation.

**Forbidden** as production artifact authority:

- `latest`  
- mutable tags  
- repository name alone  
- source tag alone  
- legacy `eduvijna-api`  
- legacy `eduvijna-web`  
- NON_PRODUCTION runtime-probe image  
- unverified local image  

`deploy_on_push` remains **false**.

This ADR does **not** invent or deposit any concrete production `aieos-backend` manifest digest.

### 9. Tag immutability

Before future first publication:

- prove the exact source-SHA tag does **not** already exist  
- do **not** overwrite/repoint an existing source-SHA tag  
- if it already exists: **read-only reconciliation** is required  
- **no blind replacement**  

### 10. Publication credential model

A future OCI publication credential is separate from:

- App release credential  
- App bootstrap credential  
- TV01 release credential  
- TV01 janitor credential  

Future publication credential scope posture:

```text
registry:read
registry:update
```

only, subject to **immediate provider-scope revalidation** before creation.

**Forbidden:**

- `api:write`  
- `registry:create`  
- `registry:delete`  
- `app:create` / `app:update` / `app:delete`  
- project mutation  
- VPC mutation  
- database mutation  
- unrelated cloud authority  

Credential creation / publication is **NOT** authorized by ADR-AIEOS-051 or WPI-OCI-I01.

### 11. Secret / Docker auth posture

Future publication credentials are transient.

Docker / registry authentication material:

- must not enter Git  
- must not enter repo-local config  
- must not enter logs  
- must not enter provenance receipt  
- must not enter Actions artifacts  
- must not enter screenshots  

Future live publication uses an isolated temporary Docker credential directory **outside** repositories and destroys it afterward.

**No publication credential exists during WPI-OCI-I01.**

### 12. Publication exactly-once / ambiguity

Future first publication is a **separately authorized** live gate (WPI-OCI-P01).

If push / publication result is ambiguous:

- **DO NOT** blindly republish  
- perform **read-only reconciliation** against registry `eduvijna-registry`, repository `aieos-backend`, exact source-SHA tag, expected source identity, and expected manifest/provenance evidence  
- classify **committed** only when provider state deterministically proves the intended artifact exists  

### 13. TV01 relationship

WPI-AP-DP-TV01 remains **AUTHORIZED** but **PAUSED** at its OCI digest gate.

TV01 resumes only after a separately authorized first publication produces a provenance-bound immutable manifest digest.

ADR-AIEOS-051 approval does **NOT** authorize:

- TV01 App CREATE yet  
- registry publication  
- PAT creation  
- App Platform mutation  
- production deployment  

### 14. Implementation sequence

Freeze:

1. ADR-AIEOS-051 architecture deposition  
2. **WPI-OCI-I01** — production Dockerfile + provenance tooling + credential-free offline CI / source validation  
3. exact-source review  
4. merge authorization  
5. normal merge + post-merge verification  
6. separately authorized **WPI-OCI-P01** first live publication  
7. capture immutable manifest digest and provenance receipt  
8. resume already-authorized **WPI-AP-DP-TV01** using that digest  
9. production deployment remains a later separate gate  

---

## Explicit non-authorizations

ADR-AIEOS-051 does **NOT** authorize:

- DigitalOcean API call  
- DigitalOcean PAT creation  
- registry login  
- Docker registry credential creation  
- repository creation by push  
- OCI publication  
- OCI promotion  
- OCI retagging  
- OCI deletion  
- App Platform mutation  
- TV01 App CREATE  
- production App CREATE  
- production deployment  
- GitHub production Environment / secret creation  
- OpenTofu apply  

**WPI-OCI-I01** = AUTHORIZED **SOURCE / OFFLINE IMPLEMENTATION ONLY** (after this ADR merges).  
**WPI-OCI-P01** live first publication = **NOT AUTHORIZED**.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A51-INV-01 | First-production Backend artifact is one common OCI image for WORKFLOW_DISPATCHER and TEMPORAL_WORKER under `eduvijna-registry` / `aieos-backend`; same immutable digest; no API/EVENT slice. |
| A51-INV-02 | Production builds require exact clean Backend Git SHA identity embedded in metadata/provenance; dirty-tree artifacts forbidden. |
| A51-INV-03 | Production Dockerfile path is `deploy/oci/Dockerfile.backend-runtime`; `Dockerfile.api-runtime-probe` remains NON_PRODUCTION and must not be promoted. |
| A51-INV-04 | Default image command is fail-closed/non-serving; App Platform supplies workload run commands. |
| A51-INV-05 | OCI revision label equals exact governed Backend SHA; required OCI identity labels present. |
| A51-INV-06 | Offline credential-free validation precedes any publication; ordinary PR CI has no registry credential. |
| A51-INV-07 | Provenance receipt is sanitized; never credentials / auth headers / production secrets / token hashes. |
| A51-INV-08 | Source-SHA tag is convenience only; production authority is immutable `sha256:` manifest digest after read-back reconciliation. |
| A51-INV-09 | Source-SHA tags are immutable; no blind overwrite; existing tag requires read-only reconciliation. |
| A51-INV-10 | Future publication credential is registry:read + registry:update only (revalidate before creation); separate from App/TV01 credentials; not authorized here. |
| A51-INV-11 | Ambiguous publication ⇒ read-only reconciliation only; no blind republish. |
| A51-INV-12 | WPI-AP-DP-TV01 remains authorized but paused on OCI digest until separately authorized first publication. |

---

## Architecture relationship

| ADR | Role |
|-----|------|
| ADR-AIEOS-022 | platform technology baseline |
| ADR-AIEOS-026 | workflow implementation baseline (Temporal) |
| ADR-AIEOS-048 | historical/base App runtime + OCI-delivery contract |
| ADR-AIEOS-048R1 | **CURRENT** App Platform naming authority |
| ADR-AIEOS-048R2 | **CURRENT** App ownership-boundary authority |
| ADR-AIEOS-049 | **CURRENT** state-free deployment-plane behavior |
| ADR-AIEOS-050 | **CURRENT** release-controller implementation architecture |
| ADR-AIEOS-051 | **CURRENT** Backend production OCI build / provenance / first-publication architecture |

---

## Consequences

### Positive

- Explicit production OCI architecture for the common Backend runtime image without authorizing registry publication  
- Clear separation of WPI-OCI-I01 (source/offline) from WPI-OCI-P01 (live publication) and TV01 resume  
- Digest-only runtime authority remains consistent with ADR-048 / 049 / 050  

### Negative / residual risk

- No production `aieos-backend` manifest digest exists until separately authorized WPI-OCI-P01  
- TV01 remains paused until that digest exists  
- Provider scope posture must be revalidated immediately before any future publication credential creation  

### Explicit non-authorizations

See Decision non-authorizations section. Architecture freeze ≠ OCI publication, credential issuance, App mutation, or production deployment.

---

## Status

**Frozen / Approved** — architecture source authority only.

Production Backend OCI architecture = **DESIGN FROZEN**.  
**WPI-OCI-I01** = AUTHORIZED **SOURCE / OFFLINE IMPLEMENTATION ONLY** (after deposition merge).  
**WPI-OCI-P01** live first publication = **NOT AUTHORIZED**.  
**WPI-AP-DP-TV01** = **AUTHORIZED BUT PAUSED ON OCI MANIFEST DIGEST**.  
Production deployment = **NOT AUTHORIZED**.
