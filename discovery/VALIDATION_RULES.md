---
id: DISC-VAL-001
title: Discovery Validation Rules
owner: Enterprise Architecture Office
status: draft
version: 0.3.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# Discovery Validation Rules

Mandatory rules for all Enterprise Discovery inventories produced under the Discovery Specification Framework. These rules apply to every domain template under `discovery/templates/`.

## 1. Never guess

- Do not invent, infer without evidence, or approximate facts to fill a field.
- If a value cannot be established from an admissible evidence source, record `UNKNOWN` (or leave optional fields omitted).
- Pattern matching, “typical for this stack,” and tribal knowledge are not substitutes for evidence.

## 2. Facts must have evidence

- Every asserted fact must be supported by at least one entry in `evidence`.
- Each evidence entry is a structured object with:
  - `type` — one of the supported evidence types (see below)
  - `location` — path or locator (e.g. `package.json`)
  - `description` — what fact(s) the evidence supports
- Free-form or narrative-only evidence is not permitted.
- Facts without evidence are invalid and must be corrected or marked `UNKNOWN` before review.

### Supported evidence types

| Type | Typical use |
|------|-------------|
| `package-json` | Node/JavaScript dependency and project metadata |
| `pyproject` | Python project metadata (`pyproject.toml`) |
| `requirements-txt` | Python requirements files |
| `dockerfile` | Container image definition |
| `docker-compose` | Compose service topology |
| `csproj` | .NET project file |
| `solution` | .NET solution file |
| `github-workflow` | GitHub Actions workflow |
| `terraform` | Terraform configuration |
| `kubernetes` | Kubernetes manifests |
| `readme` | README or equivalent documentation file |
| `manual` | Authorised manual observation / interview note with locator |
| `other` | Evidenced source not covered above (describe clearly) |

## 3. Unknown is acceptable

- `UNKNOWN` is a valid, preferred outcome when evidence is missing or inaccessible.
- Fields listed under `unknown_value_policy.fields_marked_unknown` must use the configured marker (default: `UNKNOWN`).
- Prefer an explicit unknown over a plausible but unsupported value.

## 4. Observations are separate from facts

- **Facts** are evidenced statements recorded in domain fields.
- **Observations** are non-authoritative notes, hypotheses, or qualitative remarks recorded only under `observations`.
- Observations must never be promoted into domain fact fields without new evidence and an inventory revision.
- Do not mix observational language into required fact fields.

## 5. Record assumptions explicitly

- Any working assumption required to interpret the inventory must appear under `assumptions`.
- Each assumption must state what is assumed, why it was needed, and its impact if wrong.
- Assumptions are not facts. They do not satisfy evidence requirements for fact fields.

## 6. Every inventory must be reproducible

- Complete `reproducibility` so another Discovery Engineer can replay the capture method.
- Record method, tooling (if any), and replay notes sufficient to regenerate the same factual claims from the same sources.
- Opaque “we looked around” inventories fail validation.

## 7. Schema and template conformance

- Completed inventories must validate against the corresponding JSON Schema in `discovery/schemas/`.
- Common `metadata` and `evidence` must conform to `discovery/schemas/common-metadata.schema.json`.
- Domain schemas must reference common metadata; they must not redefine metadata independently.
- Required fields defined by the template/schema must be present.
- Optional fields may be omitted; when present, they must satisfy the schema and evidence rules.

## 8. Common metadata (required)

Every inventory must include `metadata` with:

| Field | Rule |
|-------|------|
| `discovery_id` | Stable **inventory** ID per the convention below |
| `discovery_type` | Domain type matching the template |
| `source_repository` | Repository name that was the discovery source (e.g. `eduvijna-api`) |
| `discovered_at` | ISO-8601 timestamp (e.g. `2026-08-07T14:00:00Z`) |
| `discovered_by` | Person or role who captured the inventory |
| `confidence` | `HIGH` \| `MEDIUM` \| `LOW` |
| `version` | Inventory **record** content version |
| `inventory_version` | Discovery Specification Framework version that generated the inventory |

Do not invent alternate metadata structures per domain.

### Inventory version vs record version

| Field | Identifies |
|-------|------------|
| `metadata.inventory_version` | Which Discovery Framework revision produced the inventory (e.g. `1.1.0`) |
| `metadata.version` | Content revision of this specific inventory record |

## 9. Discovery ID vs subject ID

These are different concepts:

| Identifier | Example | Identifies |
|------------|---------|------------|
| `metadata.discovery_id` | `DISC-REP-0001` | The **inventory** artefact |
| Domain subject ID (e.g. `repository.repository_id`) | `REPO-eduvijna-api` | The **asset** under discovery |

Rules:

- Never use `DISC-*` as a repository (or other asset) ID.
- Never use `REPO-*` as a discovery inventory ID.
- Subject IDs remain stable across inventory revisions; `discovery_id` identifies one inventory capture/version lineage.

### Discovery ID convention

| Prefix | Domain |
|--------|--------|
| `DISC-REP-0001` | Repository |
| `DISC-APP-0001` | Application |
| `DISC-DB-0001` | Database |
| `DISC-API-0001` | API |
| `DISC-INF-0001` | Infrastructure |
| `DISC-AI-0001` | AI |
| `DISC-SEC-0001` | Security |
| `DISC-CAP-0001` | Capability |
| `DISC-TECH-0001` | Technology |
| `DISC-DEBT-0001` | Technical debt |

### Repository ID convention

- Pattern: `REPO-<stable-token>` (letters, digits, `.`, `_`, `-`).
- Prefer a durable normalised token derived from owner/name (e.g. `REPO-eduvijna-ai-eduvijna-api`).
- Do not reuse a `repository_id` for a different repository.

## 10. Repository lifecycle, criticality, and classification

Required on repository inventories. Lifecycle ≠ classification.

### Lifecycle (frozen)

`production` | `active` | `experimental` | `archived` | `planned` | `deprecated` | `UNKNOWN`

### Criticality (frozen)

`Critical` | `High` | `Medium` | `Low` | `UNKNOWN`

Use for risk assessment, modernization planning, and incident management when evidenced or explicitly authorised. Otherwise `UNKNOWN`.

### Classification (frozen vocabulary)

`Governance` | `Engineering` | `Platform` | `Runtime` | `Infrastructure` | `Research` | `Experimental` | `Archive` | `UNKNOWN`

- This vocabulary is **approved and frozen**.
- Do not invent alternate classification labels.
- If uncertain, use `UNKNOWN` and explain in `observations`.

## 11. CODEOWNERS and default branch

- **CODEOWNERS:** Evidence-first. If the file is not found at the authorised path(s), record `codeowners_present: false`. Do not infer ownership from team names or tribal knowledge.
- **Default branch:** Capture via host API / `gh` metadata in discovery runs when available. If not captured, `UNKNOWN`.

## 12. Relationships (future model)

Do not invent `depends_on` edges without evidence.

When evidenced repository-to-repository relationships appear, record:

```yaml
relationships:
  - target: REPO-eduvijna-api   # peer repository_id
    confidence: HIGH             # HIGH | MEDIUM | LOW
```

Until this structure is schema-enforced, keep relationship claims out of fact fields unless authorised by a later framework revision.

### Deferred to other discovery domains

| Concern | Target discovery |
|---------|------------------|
| API / client–server relationships | `DISC-API-*` |
| Capability pairing (e.g. Learning Journey app↔api) | `DISC-CAP-*` |
| Deployment targets / infra wiring | `DISC-INF-*` |
| Lockfile / package-manager authority | `DISC-TECH-*` |

## 13. Authorisation and scope

- Discovery work must be authorised under an EAO programme / blueprint / intake before inventories are treated as authoritative.
- Stay within the authorised scope; out-of-scope findings belong in observations or a separate authorised intake.
- Do not modify application repositories as part of discovery capture.

## 14. Versioning

- Increment `metadata.version` when factual content of an inventory changes.
- Update `metadata.inventory_version` when the generating framework revision changes.
- Retain prior inventory versions for traceability; do not silently rewrite approved history.

## Validation checklist

Before Architecture Review, confirm:

- [ ] No guessed or inferred facts
- [ ] Every fact linked to structured `evidence`
- [ ] `metadata` complete including `inventory_version`
- [ ] `discovery_id` and subject IDs are not conflated
- [ ] `discovered_at` is ISO-8601
- [ ] Repository inventories include `lifecycle`, `criticality`, `classification`
- [ ] Classification uses the frozen vocabulary only
- [ ] Unknowns marked explicitly where needed
- [ ] Deferred concerns not forced into repository discovery
- [ ] Schema validation passes (domain schema + common metadata)
