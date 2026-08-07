---
id: AR-004-OQ
title: AR-004 Open Questions
owner: Enterprise Architecture Office
status: draft
version: 0.1.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-004 — Open Questions

## Metadata and identity

1. **ID sequence ownership** — Who allocates the next `DISC-*-NNNN` number (EAO central register vs per-programme)?
2. **source_repository cardinality** — For multi-repo applications, should `source_repository` be the primary repo only, with others listed in domain optional fields?
3. **confidence guidance** — Should EAO publish rubrics mapping evidence coverage to `HIGH` / `MEDIUM` / `LOW`?

## Evidence model

4. **Evidence type expansion** — Should `LICENSE`, `CODEOWNERS`, and manifest/YAML config gain first-class types, or remain `other` / `manual`?
5. **Evidence-to-field binding** — Is `description` sufficient, or should a later enhancement add explicit `supports: [field.path]` without redesigning now?
6. **Validator $ref resolution** — Which tooling standard should Wave 1 mandate for resolving relative refs to `common-metadata.schema.json`?

## Process

7. **Migration of AR-003 drafts** — Confirm no draft inventories exist that still use `discovery` / `evidence_sources`; if any appear later, what is the cutover rule?
8. **Changelog / VERSION** — Should EBP-003 land with a repository `CHANGELOG.md` / `VERSION` bump in the approval PR?

## Non-goals (confirm)

9. Confirm reviewers agree AR-004 does **not** include repository analysis, discovery reports, or application-repo changes.
