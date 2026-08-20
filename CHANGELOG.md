# Changelog

All notable changes to the EduVijna Architecture (EAO) repository are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this repository follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- EA-SYNC-01A: canonical AIEOS platform ADR family under `decisions/ADR-AIEOS-*` (022, 024–036, 036R1). ADR-AIEOS-023 is intentionally not reconstructed. Architecture status Frozen / Approved; production deployment and mutation remain unauthorized.
- EA-SYNC-01B deposits ADR-AIEOS-023R1 as the Frozen / Approved canonical Identity, Tenant & Security restatement, preserving historical ADR-023 as frozen-but-unavailable and making no production authorization.
- EA-SYNC-01C deposits ADR-AIEOS-037 (DigitalOcean production infrastructure baseline) and ADR-AIEOS-038 (cross-cloud managed Asset object-storage exception). Spaces remains rejected. Amazon S3 is a candidate only and is not selected. Architecture synchronization only; no infrastructure, AWS, or implementation authorization.
- EA-SYNC-01D deposits ADR-AIEOS-038R1 as the Frozen / Approved current first-production DigitalOcean-only Asset storage hosting direction. ADR-AIEOS-038 is retained as an approved historical decision. Amazon S3 no longer advances for first production. Spaces remains rejected. No BlobStore provider is selected. AWS-BOOT-01 and PED-I10B7C-TV02 are cancelled. Architecture synchronization only.
- EA-SYNC-01E deposits ADR-AIEOS-039 as the Frozen / Approved first-production Asset BlobStore provider selection (MinIO AIStor, AIEOS-operated, DigitalOcean hosting). Spaces, Garage, and Amazon S3 are not open implementation choices. Topology, substrate, SKU, and commercial tier remain unselected. Architecture synchronization only; no purchase, infrastructure, adapter, or production authorization.
- EA-SYNC-01F deposits ADR-AIEOS-040 as the Frozen / Approved first-production AIStor topology (8 nodes × 1 dedicated DigitalOcean Volume/node, XFS, one erasure set N=8 EC:3, private BLR1). Empirically validated by PED-I10B7E-TV04-R2. Production compute sizing, Volume capacity, commercial purchase, backup/DR, adapter, and deployment remain open. Architecture synchronization only; no infrastructure, implementation, or production authorization.
- EA-SYNC-01G deposits ADR-AIEOS-040R1 (Bootstrap & Scale Production topology) and ADR-AIEOS-041 (Asset Backup & Recovery Architecture). ADR-AIEOS-040 is retained as historical Frozen / Approved evidence and reclassified as Scale Production. Bootstrap is single-node AIStor Free (6 × ~190 GiB, N=6/K=3/M=3, EC:3, BLR1). Backup is SFO3 Spaces Standard, non-authoritative, Versioning, verified ≤1h. Architecture synchronization only; no infrastructure, implementation, purchase, or production authorization.
- EA-SYNC-01H deposits Bootstrap architecture closure: ADR-AIEOS-042 (Asset binary delivery & 32 MiB Bootstrap media profile), ADR-AIEOS-043 (private AIStor service boundary & primary namespace), and ADR-AIEOS-041R1 (backup execution/manifest/recovery authority; no required Bootstrap Asset events; 7-day PITR Phase-0). ADR-AIEOS-041 remains Frozen / Approved. Commercial envelope ≤ USD 240 / hard ceiling USD 250 preserved without provisioning authorization. Architecture synchronization only; no backend, cloud, credential, OpenTofu, deployment, or purchase authorization.

### Changed

- Nothing yet.

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
