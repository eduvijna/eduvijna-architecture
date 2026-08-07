---
id: AR-003-OQ
title: AR-003 Open Questions
owner: Enterprise Architecture Office
status: draft
version: 0.2.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-003 — Open Questions

Questions for Architecture Review before treating the Discovery Specification Framework as authoritative.

## Field model and enums

1. **Boolean encoding** — Templates encode booleans as strings (`true` | `false` | `UNKNOWN`) for uniform unknown handling. Confirm this is preferred over JSON booleans + separate unknown flags.
2. **Enum exhaustiveness** — Are domain enums (application_type, infra_type, AI asset_type, debt category, etc.) sufficient for Wave 1, or should they be constrained to a governed vocabulary in `references/` / `meta/`?
3. **Optional block shape** — Optional fields are grouped under `*_optional` objects. Prefer flat optional properties at the domain object instead?

## Process and placement

4. **Inventory storage location** — Where should *completed* inventories live (e.g. `discovery/inventories/`, programme folders, or external working repos)? Templates currently do not prescribe a storage root beyond “authorised discovery working location.”
5. **Programme ID format** — What canonical form should `programme_id` take (blueprint ID, issue ID, charter ID)?
6. **Validation tooling** — Should EAO mandate a specific validator (e.g. AJV, check-jsonschema) in a later blueprint, or remain tooling-agnostic for now?
7. **Changelog / version bump** — Should this framework introduction update `CHANGELOG.md` / `VERSION` in the same PR as approval?

## Governance boundaries

8. **Security inventories** — Do security discovery items require Security / InfoSec co-review before `approved` status, or is EAO review sufficient for inventory capture?
9. **Technical debt severity** — Who is authorised to set `severity: critical`, and must that trigger a linked review or decision artefact?

## Non-goals (confirm)

10. Confirm reviewers agree this package does **not** include analysis of EduVijna application repositories or discovery report content.
