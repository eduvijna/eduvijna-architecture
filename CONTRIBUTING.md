# Contributing to EduVijna Architecture

Contributions to this Enterprise Architecture Office (EAO) repository must preserve governance integrity, traceability, and reviewability.

## Rules

### No implementation code

Do not add application source code, scripts that implement product behaviour, infrastructure-as-code for product runtimes, or executable services. This repository is for architecture governance artefacts only.

### Pull Request required

All changes land through a pull request to the default branch. Direct commits to `main` are not permitted under normal process.

### Architecture Review required

Material changes to architecture, standards, decisions, reviews, discovery outputs, roadmaps, or blueprints require Architecture Review before approval. Editorial corrections may follow a lighter review path when they do not alter meaning.

### Stable artifact IDs

Assigned artefact identifiers are stable. Do not reuse, renumber, or silently repurpose IDs. If an artefact is superseded, retain history and mark status appropriately.

### Markdown quality

- Use clear headings and concise prose
- Prefer tables where they improve scanability
- Ensure links resolve within the repository
- Follow `templates/document-header.md` for new Markdown documents

### Cross references

Link related artefacts by stable ID and path. Prefer repository-relative links. Keep terminology aligned with `references/glossary.md` and `meta/taxonomy.yaml`.

### Versioning expectations

- Update artefact `version` when meaning changes
- Record consumer-visible repository changes in `CHANGELOG.md`
- Align repository version in `VERSION` and `meta/repository-manifest.yaml` when releasing

## Workflow

1. Open an issue for non-trivial work.
2. Create a descriptive branch from `main`.
3. Make focused changes within the correct directory.
4. Update `CHANGELOG.md` under `[Unreleased]` when appropriate.
5. Open a pull request using the PR template.
6. Request review from owners in `CODEOWNERS`.
7. Merge only after required approvals.

## Contribution types

| Type | Process |
|------|---------|
| Governance / process | PR + EAO review |
| Architecture / standards / decisions | PR + Architecture Review |
| Discovery / roadmap / blueprints | PR + Architecture Review |
| References / glossary / metadata | PR + EAO review |
| Editorial | Lightweight PR |

## Review checklist

Reviewers verify:

- Correct directory placement and ownership
- No implementation code introduced
- Stable IDs preserved
- Cross-references and glossary consistency
- Markdown quality and working links
- Version and changelog expectations met

## Questions

Use a General or Governance issue template, or contact the Enterprise Architecture Office through EduVijna leadership channels.
