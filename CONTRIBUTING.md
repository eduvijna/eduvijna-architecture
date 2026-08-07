# Contributing to EduVijna Architecture

Thank you for contributing to the EduVijna Enterprise Architecture Office (EAO) repository.

This repository governs architecture and engineering process for the EduVijna ecosystem. Contributions must preserve clarity, traceability, and reviewability.

## Before you contribute

1. Confirm the change belongs in this governance repository (not an application repo).
2. Read the `OVERVIEW.md` for the directory you intend to change.
3. Prefer small, reviewable pull requests over large unstructured dumps.
4. Do not invent architecture, standards, ADRs, or specifications outside an authorised sprint or work item.

## Contribution types

| Type | Examples | Expected process |
|------|----------|------------------|
| Bootstrap / workspace | Folder structure, templates, overview text | PR with clear rationale |
| Governance process | Contribution rules, review checklists | EAO review required |
| Architecture content | Reference models, views, catalogues | Authorised sprint; architecture review |
| Standards | Coding, API, security, data standards | Authorised sprint; standards review |
| Decisions | ADRs | Authorised decision process |
| Discovery | Discovery reports and inventories | Authorised discovery sprint |
| Editorial | Typos, broken links, formatting | Lightweight PR |

## Workflow

1. **Open an issue** using the appropriate GitHub issue template when the change is non-trivial.
2. **Create a branch** from the default branch using a descriptive name (for example `docs/ebp-001-bootstrap` or `chore/update-overview`).
3. **Make focused changes** that match the issue and directory scope.
4. **Update `CHANGELOG.md`** under `[Unreleased]` when the change is user-visible to consumers of this repository.
5. **Open a pull request** using the pull request template.
6. **Request review** from the owners listed in `CODEOWNERS`.
7. **Merge only after approval** and any required checks pass.

## Pull request expectations

- Describe *why* the change is needed, not only *what* changed.
- Link related issues, blueprints, or sprint identifiers (for example `EBP-001`, `A-002`).
- Keep scope aligned with the acceptance criteria of the governing blueprint or work item.
- Do not commit secrets, credentials, or personal data.
- Do not add empty placeholder architecture documents “for later.”

## Review expectations

Reviewers should verify:

- Correct directory placement and ownership
- Scope compliance (no premature architecture/standards/ADR/discovery content during bootstrap phases)
- Clarity and professional tone
- Consistency with existing overview and contribution guidance

## Code of conduct for contributions

- Be precise and constructive in review comments.
- Prefer evidence and references over opinion when debating architecture direction.
- Escalate unresolved disagreements through EAO governance channels, not ad-hoc merges.

## Questions

For process questions, open a `governance` or `general` issue, or contact the Enterprise Architecture Office via the channels defined by EduVijna leadership.
