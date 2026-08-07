# EduVijna Architecture

Enterprise Architecture Office repository for the EduVijna ecosystem.

## Mission

Establish and steward coherent enterprise architecture, governance, and engineering practice so EduVijna capabilities evolve with clarity, accountability, and lasting architectural integrity.

## Purpose

This repository is the governance workspace of the EduVijna Enterprise Architecture Office (EAO). It defines how architecture is governed, how standards and decisions are managed, how reviews are conducted, and how enterprise discovery and transformation guidance are organised.

It is not an application repository and does not contain implementation code.

## Repository Scope

In scope:

- Enterprise architecture governance
- Engineering and architecture standards stewardship
- Architecture reviews and decision management
- Enterprise discovery programme artefacts
- Transformation roadmap and engineering blueprints
- External references and agreed terminology
- Repository metadata and taxonomy

Out of scope:

- Application source code
- Runtime configuration and infrastructure-as-code for product services
- Product feature backlogs
- Informal notes outside EAO process

## Repository Structure

| Directory | Role |
|-----------|------|
| `office/` | Architecture Office operating context |
| `governance/` | Governance policies and processes |
| `discovery/` | Enterprise discovery programme |
| `architecture/` | Architecture artefacts |
| `standards/` | Standards library |
| `decisions/` | Architecture decisions and decision log |
| `reviews/` | Architecture and design reviews |
| `roadmap/` | Transformation roadmap |
| `blueprints/` | Engineering blueprints |
| `references/` | External references and glossary |
| `meta/` | Repository manifest, taxonomy, and artefact index |
| `templates/` | Shared document standards and templates |

## Engineering Lifecycle

1. **Discover** — Establish current-state understanding through authorised discovery work.
2. **Decide** — Record significant decisions and maintain the decision log.
3. **Define** — Publish architecture, standards, and blueprints under EAO stewardship.
4. **Review** — Conduct architecture reviews before material change is accepted.
5. **Guide** — Sequence transformation through roadmap artefacts.
6. **Govern** — Maintain process integrity through contribution and ownership controls.

## Artifact Types

| Type | Location | Description |
|------|----------|-------------|
| Office | `office/` | Operating model and office orientation |
| Governance | `governance/` | Policies and process artefacts |
| Discovery | `discovery/` | Discovery plans and findings |
| Architecture | `architecture/` | Architecture artefacts and views |
| Standard | `standards/` | Approved standards |
| Decision | `decisions/` | ADRs and the decision log |
| Review | `reviews/` | Review records and outcomes |
| Roadmap | `roadmap/` | Transformation sequencing |
| Blueprint | `blueprints/` | Engineering blueprint directives |
| Reference | `references/` | External references and glossary |
| Metadata | `meta/` | Manifest, taxonomy, artefact index |

Artefact front matter follows `templates/document-header.md`. Taxonomy categories are defined in `meta/taxonomy.yaml`.

## Contribution Workflow

1. Confirm the change belongs in this governance repository.
2. Open an issue using the appropriate template when the change is material.
3. Branch from `main` and keep the change focused.
4. Submit a pull request; merge requires review.
5. Architecture Review is required for architecture, standards, decisions, and related material changes.
6. Follow `CONTRIBUTING.md` for quality, identifiers, cross-references, and versioning.

## Repository Roadmap

| Wave | Focus |
|------|--------|
| Complete | Repository foundation and refinement (EBP-001, EBP-002) |
| Next | Enterprise Discovery Wave 1 |
| Later | Standards, ADRs, architecture models, and platform specifications under authorised sprints |

See `VERSION`, `CHANGELOG.md`, and `meta/repository-manifest.yaml` for current repository metadata.
