# Changelog

All notable changes to the EduVijna Architecture (EAO) repository are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this repository follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- ADR-AIEOS-047 deposits AIEOS Production Workflow Plane Identity & Least-Privilege Contract as **Frozen / Approved**: Temporal Cloud first-production hosting specialization of ADR-AIEOS-026; environment-isolated Namespace Endpoint connection mode; TLS + certificate verification required; distinct Temporal Cloud Service Account + API key identities for WORKFLOW_DISPATCHER and TEMPORAL_WORKER; minimum stable provider RBAC Account Read + target Namespace Write (no Namespace Admin / Account Admin); Custom Roles not required (Pre-Release as of 2026-08-23); WORKFLOW_DISPATCHER fenced to governed ContentReviewWorkflowV1 / `aieos.content.review` / `review_decision_recorded` start/describe/history/signal/result operations; dispatcher Temporal env `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_*` with secret `AIEOS_WORKFLOW_DISPATCHER_TEMPORAL_API_KEY`; existing worker `AIEOS_TEMPORAL_*` family preserved and not shared. Distinct from Teacher OS ADR-047. Architecture authority only; no Temporal Cloud provisioning, credentials, Backend WORKFLOW dispatcher implementation, DigitalOcean mutation, OpenTofu apply, or deployment.
- ADR-AIEOS-046 deposits AIEOS Production Event Plane Identity & Least-Privilege Contract as **Frozen / Approved**: production stream `AIEOS_EVENTS_PROD` with subjects `io.eduvijna.aieos.>`; EVENT publisher PUB `io.eduvijna.aieos.content.>` and SUB `_INBOX.>`; JWT + NKey `.creds` via App Platform encrypted env `AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS` consumed in-memory (`user_jwt_cb` + `signature_cb`); separate `streamadmin`; EVENT runtime has no `$JS.API` stream-admin authority; supersedes ADR-025 modular-first `AIEOS_EVENTS` / `aieos.event.v1.>` for production authority. Architecture authority only; no production NATS provisioning, credentials, stream creation, Backend daemon, DigitalOcean mutation, or deployment.
- ADR-AIEOS-045 deposits dispatcher tenant-candidate discovery authority as **Frozen / Approved**: pending-work candidate discovery from `integration.outbox_messages` and workflow intent queues; dedicated NOLOGIN NOBYPASSRLS candidate-reader identities; SECURITY DEFINER owned by candidate-reader not schema owner; dispatcher login remains NOBYPASSRLS with EXECUTE-only access; no cross-tenant payload visibility; committed outbox/intent delivery not suppressed by later tenant suspension alone. Architecture authority only; no backend, database migration, role creation, dispatcher daemon execution, or production authorization.
- EA-SYNC-01A: canonical AIEOS platform ADR family under `decisions/ADR-AIEOS-*` (022, 024–036, 036R1). ADR-AIEOS-023 is intentionally not reconstructed. Architecture status Frozen / Approved; production deployment and mutation remain unauthorized.
- EA-SYNC-01B deposits ADR-AIEOS-023R1 as the Frozen / Approved canonical Identity, Tenant & Security restatement, preserving historical ADR-023 as frozen-but-unavailable and making no production authorization.
- EA-SYNC-01C deposits ADR-AIEOS-037 (DigitalOcean production infrastructure baseline) and ADR-AIEOS-038 (cross-cloud managed Asset object-storage exception). Spaces remains rejected. Amazon S3 is a candidate only and is not selected. Architecture synchronization only; no infrastructure, AWS, or implementation authorization.
- EA-SYNC-01D deposits ADR-AIEOS-038R1 as the Frozen / Approved current first-production DigitalOcean-only Asset storage hosting direction. ADR-AIEOS-038 is retained as an approved historical decision. Amazon S3 no longer advances for first production. Spaces remains rejected. No BlobStore provider is selected. AWS-BOOT-01 and PED-I10B7C-TV02 are cancelled. Architecture synchronization only.
- EA-SYNC-01E deposits ADR-AIEOS-039 as the Frozen / Approved first-production Asset BlobStore provider selection (MinIO AIStor, AIEOS-operated, DigitalOcean hosting). Spaces, Garage, and Amazon S3 are not open implementation choices. Topology, substrate, SKU, and commercial tier remain unselected. Architecture synchronization only; no purchase, infrastructure, adapter, or production authorization.
- EA-SYNC-01F deposits ADR-AIEOS-040 as the Frozen / Approved first-production AIStor topology (8 nodes × 1 dedicated DigitalOcean Volume/node, XFS, one erasure set N=8 EC:3, private BLR1). Empirically validated by PED-I10B7E-TV04-R2. Production compute sizing, Volume capacity, commercial purchase, backup/DR, adapter, and deployment remain open. Architecture synchronization only; no infrastructure, implementation, or production authorization.
- EA-SYNC-01G deposits ADR-AIEOS-040R1 (Bootstrap & Scale Production topology) and ADR-AIEOS-041 (Asset Backup & Recovery Architecture). ADR-AIEOS-040 is retained as historical Frozen / Approved evidence and reclassified as Scale Production. Bootstrap is single-node AIStor Free (6 × ~190 GiB, N=6/K=3/M=3, EC:3, BLR1). Backup is SFO3 Spaces Standard, non-authoritative, Versioning, verified ≤1h. Architecture synchronization only; no infrastructure, implementation, purchase, or production authorization.
- EA-SYNC-01H deposits Bootstrap architecture closure: ADR-AIEOS-042 (Asset binary delivery & 32 MiB Bootstrap media profile), ADR-AIEOS-043 (private AIStor service boundary & primary namespace), and ADR-AIEOS-041R1 (backup execution/manifest/recovery authority; no required Bootstrap Asset events; 7-day PITR Phase-0). ADR-AIEOS-041 remains Frozen / Approved. Commercial envelope ≤ USD 240 / hard ceiling USD 250 preserved without provisioning authorization. Architecture synchronization only; no backend, cloud, credential, OpenTofu, deployment, or purchase authorization.
- ADR-AIEOS-044 deposits Bootstrap production pre-apply execution baseline: Founder DO ceiling/target restated; 2026-08-21 RED commercial evidence (~USD 294.05); NATS single-node Bootstrap; VPC/CIDR; Spaces OpenTofu state; AIStor identity/admin/TLS freezes; App Platform sizing implementation-gated; Temporal Cloud separated commercially. Architecture freeze ≠ production apply. No backend, infrastructure, cloud, credential, OpenTofu, or purchase authorization.
- ADR-AIEOS-044R1 deposits production state namespace collision resolution: supersedes ADR-044 §F literal only to `eduvijna-aieos-tofu-state-prod-blr1` (BLR1); classifies NYC3 `eduvijna-aieos-tofu-state-prod` as UNATTRIBUTED / PRE-EXISTING / NON-AUTHORITATIVE / HOLD; suspends Stage 1 execution until merge, infra reconciliation, and fresh authorization. Architecture deposit only; no cloud, backend, infrastructure, credential, OpenTofu, deletion, or Stage 1 resumption authority.
- ADR-AIEOS-044R2 deposits production state region availability resolution: moves forward OpenTofu state from BLR1 / `eduvijna-aieos-tofu-state-prod-blr1` to SFO3 / `eduvijna-aieos-tofu-state-prod-sfo3` because DigitalOcean disables new Spaces creates in BLR1; workload region remains BLR1; Stage 1 remains suspended. Architecture deposit only; no cloud, backend, infrastructure, credential, OpenTofu, or Stage 1 resumption authority.

### Changed

- ADR-AIEOS-045 freeze finalization: architecture status recorded as Frozen / Approved. Decision semantics unchanged. Dispatcher daemon implementation, database candidate-function migration, production candidate-reader role provisioning, and production deployment remain not authorized.

- Post-Stage-3B Architecture Execution-State Reconciliation: records Stage 3A formally closed (bounded refresh-only plan; native S3 live-lock cycle validated; zero managed resources; no DigitalOcean workload mutation) and Stage 3B formally closed (exact inspected refresh-only saved-plan apply; first authoritative SFO3 remote tfstate materialized at serial 1 with zero managed resources; `tofu state list` empty; current persistent lock absent). Pre-Stage-3B output-semantics correction merged and verified. Commercial blocker remains RED. Normal production workload plan/apply remains unauthorized. No ADR changed.
- Post-Stage-2 Production State Current-State Reconciliation: records Stage 1 formally closed (SFO3 bucket `eduvijna-aieos-tofu-state-prod-sfo3` exists, private, Versioning Enabled; permanent bucket-scoped readwrite state credential established outside Git; temporary fullaccess provisioning key destroyed) and Stage 2 formally closed (OpenTofu 1.12.5 S3 backend initialized against SFO3 with `use_lockfile=true`; credentials not persisted). Remote tfstate object and persistent lock object remain absent. No plan/apply occurred. Production workload remains BLR1. Commercial blocker remains in force. No ADR changed.
- ADR-AIEOS-044 v1.0.1 corrects the setup-opentofu action mapping: `a1320f892987e89d278cc92dc5adc984fb93aca4` is **v2.0.2**, not v1.0.8. No architecture topology, commercial authority, implementation authority, infrastructure, credential, or production authorization changes.

### Deprecated

- Nothing yet.

### Removed

- Nothing yet.

### Fixed

- Nothing yet.

### Security

- Nothing yet.

## [1.0.0] - 2026-08-07

### Added

- `meta/` with `repository-manifest.yaml`, `taxonomy.yaml`, and `artifact-index.yaml`.
- `templates/document-header.md` repository metadata standard.
- `references/glossary.md` with agreed terminology.
- `decisions/DECISION_LOG.md` explaining the decision log versus ADRs.
- `templates/OVERVIEW.md` and `meta/OVERVIEW.md`.

### Changed

- Renamed `assets/` to `references/` and updated repository references.
- Rewrote `README.md` and `CONTRIBUTING.md` for production governance use.
- Rewrote all directory `OVERVIEW.md` files in operational language.
- Improved GitHub issue templates, pull request template, and `CODEOWNERS`.

### Removed

- Placeholder and bootstrap-oriented wording from overview and contribution docs.

## [0.1.0] - 2026-08-07

### Added

- Initial repository bootstrap per Engineering Blueprint EBP-001.
- Root documentation: `README.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `CODEOWNERS`, `LICENSE`, `VERSION`, `.gitignore`.
- GitHub configuration: pull request template, issue templates, and empty `workflows` folder.
- Top-level workspace directories with `OVERVIEW.md` files.

[Unreleased]: https://github.com/eduvijna/eduvijna-architecture/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/eduvijna/eduvijna-architecture/compare/v0.1.0...v1.0.0
[0.1.0]: https://github.com/eduvijna/eduvijna-architecture/releases/tag/v0.1.0
