# Changelog

All notable changes to the EduVijna Architecture (EAO) repository are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this repository follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- EA-SYNC-01A: canonical AIEOS platform ADR family under `decisions/ADR-AIEOS-*` (022, 024–036, 036R1). ADR-AIEOS-023 is intentionally not reconstructed. Architecture status Frozen / Approved; production deployment and mutation remain unauthorized.
- EA-SYNC-01B deposits ADR-AIEOS-023R1 as the Frozen / Approved canonical Identity, Tenant & Security restatement, preserving historical ADR-023 as frozen-but-unavailable and making no production authorization.
- EA-SYNC-01C deposits ADR-AIEOS-037 (DigitalOcean production infrastructure baseline) and ADR-AIEOS-038 (cross-cloud managed Asset object-storage exception). Spaces remains rejected. Amazon S3 is a candidate only and is not selected. Architecture synchronization only; no infrastructure, AWS, or implementation authorization.

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
