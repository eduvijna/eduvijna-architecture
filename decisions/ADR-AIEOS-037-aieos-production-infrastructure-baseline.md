---
id: ADR-AIEOS-037
title: AIEOS Production Infrastructure Baseline
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-19
last_updated: 2026-08-19
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-037 — AIEOS Production Infrastructure Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-19  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) · [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) · [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md)

**Catalogue note:** Frozen / Approved is architecture status. This ADR does **not** authorize infrastructure creation, OpenTofu apply, production deployment, credentials, DNS, Asset BlobStore provider selection, DigitalOcean Spaces, Asset HTTP, or PED-I03 mutation. [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) is the explicit later exception to same-cloud preference for Asset authoritative bytes. This ADR does **not** select Amazon S3.

---

## Context

AIEOS needs a frozen first-production infrastructure baseline so later provider, runtime, and storage decisions cannot silently reopen cloud, compute, database, or identity choices. DigitalOcean is selected as the production cloud. DigitalOcean selection does **not** automatically select DigitalOcean Spaces as the Asset BlobStore.

## Decision

### Cloud baseline

- DigitalOcean is the AIEOS production cloud baseline.
- First production is single-region.
- Initial production locality is **BLR1**.
- BLR1 is an operational locality decision, **not** a legal/data-residency mandate.
- Multi-region, cross-cloud compute, global traffic management, and customer-dedicated infrastructure are not part of first production.

### Runtime compute

- DigitalOcean App Platform is the primary first-production **stateless** compute platform.
- Intended workload classes include:
  - AIEOS API runtime
  - Temporal workers
  - event/workflow dispatch workers
  - scheduled/reconciliation jobs where suitable
- Kubernetes is **not** required for first production.
- DOKS/Kubernetes may be reconsidered only by a later architecture decision.

### OCI / release

Preserve [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md):

- GitHub Actions
- OCI images
- GitHub Container Registry (GHCR)
- immutable digests
- provenance / attestations

Production deploys by immutable digest. Mutable tags such as `latest` are not deployment authority. Build once; promote the same verified artifact identity.

### PostgreSQL

- DigitalOcean Managed PostgreSQL **18** is the production database class.
- Normal runtime PostgreSQL connectivity is **private**.
- Preserve:

  runtime DB identity
  ≠
  migrator identity
  ≠
  schema-owner identity
  ≠
  backup/restore authority

- PostgreSQL remains the domain business SoR under [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md).
- RLS remains defense in depth.
- A DigitalOcean administrative DB identity is **not** the ordinary AIEOS application authority.

### Network

- Production and non-production infrastructure are isolated.
- Production uses a dedicated private-network boundary where the platform supports it.
- PostgreSQL and AIEOS-operated internal/stateful services are not normal public-internet services.
- Public API ingress requires HTTPS/TLS.
- Internal/service connections require authenticated encrypted transport appropriate to the service.
- Private connectivity is required **where supported**; where a platform service only supports authenticated TLS egress, the architecture may use that rather than inventing unsupported private networking.

### Ingress

- A TLS-terminating production ingress class is required.
- A separate API gateway, CDN, WAF, or service mesh is **not** required for first production.
- Trusted-proxy semantics remain a separate implementation/configuration decision.

### Temporal

- Temporal remains the workflow engine under [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md).
- **Temporal Cloud** is the production Temporal service.
- AIEOS workers remain AIEOS-operated workloads.
- Temporal execution history is not domain business truth and is not authorization authority.
- Worker-to-Temporal connectivity uses authenticated TLS.

### NATS

- NATS JetStream remains the event broker selected by [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md).
- For the first production baseline, NATS JetStream remains **AIEOS-operated on private DigitalOcean infrastructure**.
- Exact node sizing, storage sizing, HA counts, and recovery SLO values are **not** frozen by this ADR.
- NATS availability does not replace PostgreSQL/outbox business truth.

### Infrastructure identity

- Infrastructure credentials never become AIEOS business authority.
- DigitalOcean identity/credential ≠ AIEOS Principal.
- Preserve [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md).
- Cloud/platform permissions do not authorize domain capabilities.

### Secrets

- First-production secret injection may use protected/encrypted runtime configuration supplied by the hosting platform.
- A separate secret-manager product is **not** required by this ADR.
- Secrets must not be committed into Git, OCI layers, OpenAPI, workflow history, logs, or security-audit payloads.
- Credentials must be independently rotatable and least privilege.

### OpenTofu

- OpenTofu remains the AIEOS infrastructure-as-code baseline ([ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md)).
- AIEOS-owned production infrastructure should be declarative through OpenTofu where applicable.
- Production OpenTofu apply requires **explicit approval**.
- Production and non-production state must be isolated.
- Remote state/locking mechanism is **not** selected by this ADR.
- No OpenTofu apply is authorized by this ADR.

### Environment isolation

- Production and non-production must not share authoritative production resources.
- Isolation applies as appropriate to: cloud project/account boundary, private network, PostgreSQL, NATS, Temporal namespace/account boundary, object storage, secrets.
- This is **not** tenant-per-environment architecture.

### Backup / restore / DR

- Production PostgreSQL requires managed backup/PITR class and an AIEOS restore-verification procedure.
- Backup/restore ownership must be explicit.
- Cross-store restore must preserve current authoritative governance.
- Exact RPO/RTO numeric SLOs are deferred to production-readiness governance.
- Multi-region DR is deferred.

### Encryption

Production requires:

- TLS for public ingress
- authenticated/encrypted service transport
- encryption-at-rest class for stateful production services and backups

Provider-managed encryption is acceptable for first production unless later legal/security requirements require customer-managed keys. No KMS product is selected here.

### Infrastructure administration

Infrastructure administration is separate from AIEOS application authorization and application break-glass.

Require:

- least privilege
- MFA for privileged human infrastructure access
- auditable production changes
- no universal shared infrastructure administrator credential
- ordinary application runtime has no infrastructure-control-plane authority

Production OpenTofu/deployment actions require governed approval.

### Observability

Preserve [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md):

- OpenTelemetry
- OTLP
- W3C Trace Context
- structured JSON logs

Long-term observability backend/vendor remains deferred. Observability failure must not become authorization ALLOW or business success.

### Mutation separation

Preserve [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) and PED-I03:

deploy ≠ live ≠ ready ≠ mutation authorized

Infrastructure deployment does not authorize product mutation.

### Asset storage non-selection

DigitalOcean cloud selection **does not** automatically select DigitalOcean Spaces as the Asset BlobStore. Asset BlobStore provider requires an independent frozen architecture decision.

[ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) later records a narrow cross-cloud managed Asset-storage exception. This ADR does **not** select Amazon S3 or any BlobStore provider.

## Binding invariants

| ID | Invariant |
|----|-----------|
| **PI-INV-01** | DigitalOcean is the production AIEOS cloud baseline. |
| **PI-INV-02** | First production is single-region. |
| **PI-INV-03** | BLR1 is the initial production locality and is not a legal residency mandate. |
| **PI-INV-04** | App Platform is primary first-production stateless compute. |
| **PI-INV-05** | Kubernetes is not required for first production. |
| **PI-INV-06** | Production OCI artifacts remain GHCR-hosted and are deployed by immutable digest. |
| **PI-INV-07** | DigitalOcean Managed PostgreSQL 18 is the production database class. |
| **PI-INV-08** | PostgreSQL is accessed through the private production network during normal runtime. |
| **PI-INV-09** | Runtime, migrator, schema-owner and backup authorities remain distinct. |
| **PI-INV-10** | Production and non-production infrastructure are isolated. |
| **PI-INV-11** | Public application ingress requires TLS. |
| **PI-INV-12** | Internal service connections require authenticated/encrypted transport appropriate to the service. |
| **PI-INV-13** | Temporal Cloud is the production Temporal service; AIEOS workers remain AIEOS-operated. |
| **PI-INV-14** | NATS JetStream remains AIEOS-operated on private DigitalOcean infrastructure for the first baseline. |
| **PI-INV-15** | Infrastructure credentials never become AIEOS business authorization. |
| **PI-INV-16** | OpenTofu is the authoritative IaC mechanism; production apply requires explicit approval. |
| **PI-INV-17** | Deploy/live/ready remains distinct from mutation authorization. |
| **PI-INV-18** | DigitalOcean selection does not automatically select DigitalOcean Spaces as the Asset BlobStore. |

## Explicit non-goals / deferred decisions

This ADR does **not** authorize:

- production infrastructure creation
- OpenTofu apply
- production deployment
- DNS modification
- production credentials
- Asset BlobStore provider
- DigitalOcean Spaces
- Asset HTTP
- Asset upload/download
- signed/presigned delivery
- CDN
- Asset events
- Asset purge/retention/legal hold
- Asset schema-owner activation
- PED-I03 Asset mutation activation
- production DB migration
- production mutation

## Consequences

- Later workstreams inherit DigitalOcean, BLR1, App Platform, Managed PostgreSQL 18, Temporal Cloud, and AIEOS-operated NATS as first-production baseline.
- A BlobStore provider still requires its own frozen ADR. Spaces is not selected here.
- [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) may authorize a narrow external managed object-store **candidate** path without reopening this cloud baseline.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) | Technology family; OCI, OTel, OpenTofu |
| [ADR-AIEOS-023R1](ADR-AIEOS-023R1-aieos-identity-tenant-security-canonical-restatement.md) | Infra credential ≠ Principal |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | PostgreSQL domain SoR |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | NATS JetStream |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Temporal |
| [ADR-AIEOS-029](ADR-AIEOS-029-production-environment-deployment-readiness-baseline.md) | Deploy/live/ready ≠ mutation |
| [ADR-AIEOS-033](ADR-AIEOS-033-asset-file-architecture.md) | Provider-neutral BlobStore; no provider here |
| [ADR-AIEOS-038](ADR-AIEOS-038-aieos-cross-cloud-asset-object-storage-exception.md) | Narrow later exception for Asset object storage only; S3 not selected here |
