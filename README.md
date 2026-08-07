# EduVijna Architecture

Enterprise Architecture Office (EAO) governance repository for the EduVijna ecosystem.

This is **not** an application repository. It is the authoritative workspace for architecture governance, engineering standards stewardship, architecture reviews, decision records, discovery artefacts, and enterprise transformation guidance.

## Purpose

Provide a single, version-controlled home for how EduVijna designs, governs, and evolves its enterprise architecture.

## What this repository is

| Area | Role |
|------|------|
| Office | EAO operating model and workspace orientation |
| Governance | Policies, processes, and compliance artefacts |
| Discovery | Enterprise discovery programme outputs (future) |
| Architecture | Reference and target-state architecture artefacts (future) |
| Standards | Engineering and architecture standards (future) |
| Decisions | Architecture Decision Records (future) |
| Reviews | Architecture and design review artefacts (future) |
| Roadmap | Transformation and capability roadmap (future) |
| Blueprints | Engineering and delivery blueprints |
| Assets | Shared diagrams, templates, and supporting media |

## What this repository is not

- Application source code
- Runtime configuration or infrastructure-as-code for production services
- Product feature backlog
- Informal personal notes outside EAO process

## Current status

**Sprint EBP-001 — Repository bootstrap**

This release establishes the folder hierarchy, root documentation, GitHub contribution templates, and directory overview files only. Architecture documents, standards, ADRs, specifications, and discovery reports are intentionally deferred to later sprints (starting with Sprint A-002).

## Repository layout

```text
eduvijna-architecture/
├── office/          # EAO workspace and operating context
├── governance/      # Governance artefacts and processes
├── discovery/       # Enterprise discovery programme
├── architecture/    # Architecture artefacts
├── standards/       # Standards library
├── decisions/       # Architecture decisions
├── reviews/         # Review records and outcomes
├── roadmap/         # Transformation roadmap
├── blueprints/      # Engineering blueprints
└── assets/          # Shared supporting assets
```

Each top-level directory contains an `OVERVIEW.md` describing purpose, scope, ownership, contents, and exclusions.

## Getting started

1. Read this `README.md` and the relevant directory `OVERVIEW.md` files.
2. Follow `CONTRIBUTING.md` for contribution and review expectations.
3. Use GitHub issue and pull request templates for proposed changes.
4. Do not add architecture, standards, ADR, or discovery content until the corresponding sprint authorises it.

## Versioning

See `VERSION` for the current repository bootstrap version and `CHANGELOG.md` for release history.

## Ownership

Owned by the EduVijna Enterprise Architecture Office (EAO). Code owners are declared in `CODEOWNERS`.

## License

See `LICENSE`.
