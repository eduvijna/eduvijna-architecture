---
id: AR-003-CHECKLIST
title: AR-003 Review Checklist
owner: Enterprise Architecture Office
status: draft
version: 0.2.0
created: 2026-08-07
last_updated: 2026-08-07
reviewers: []
---

# AR-003 — Checklist

Use during Architecture Review of the Discovery Specification Framework.

## A. Objective conformance

- [ ] Change creates reusable discovery templates and schemas only
- [ ] No EduVijna application repositories were analysed
- [ ] No discovery reports were generated
- [ ] No application repositories were modified

## B. Structure

- [ ] `discovery/OVERVIEW.md` describes purpose, ownership, and framework usage
- [ ] `discovery/VALIDATION_RULES.md` present
- [ ] `discovery/templates/` contains all ten domain templates
- [ ] `discovery/schemas/` contains all ten corresponding JSON schemas
- [ ] `discovery/examples/repository-example.yaml` present as the sole worked example

## C. Template quality (sample each domain)

- [ ] Required fields are defined
- [ ] Optional fields are defined
- [ ] Field comments describe intent
- [ ] Evidence Source section present
- [ ] Unknown Value Policy present
- [ ] Observations kept separate from facts
- [ ] Assumptions captured explicitly (structure present)
- [ ] Reproducibility section present

## D. Schema quality

- [ ] Each schema validates its corresponding completed inventory shape
- [ ] `discovery_type` const matches the domain
- [ ] Required vs optional sections align with the template
- [ ] Evidence, unknown-value policy, and reproducibility are enforced

## E. Validation rules

- [ ] Never guess
- [ ] Facts must have evidence
- [ ] Unknown is acceptable
- [ ] Observations separate from facts
- [ ] Assumptions recorded explicitly
- [ ] Inventories must be reproducible

## F. Worked example

- [ ] Repository example is complete and illustrative
- [ ] Example uses `UNKNOWN` where appropriate (does not guess)
- [ ] Example evidence sources map to asserted facts
- [ ] Example validates against `repository.schema.json`

## G. Governance / repository hygiene

- [ ] Artefacts belong under `discovery/` (framework) and `reviews/review-packages/AR-003/` (this package)
- [ ] No implementation / product code introduced
- [ ] Stable IDs used where assigned (`AR-003`, `DISC-VAL-001`, inventory ID patterns)
- [ ] `CONTRIBUTING.md` expectations considered (changelog/version if releasing)

## H. Package completeness at generation time

| Item | Result |
|------|--------|
| Overview + validation rules | Pass |
| Ten YAML templates | Pass |
| Ten JSON schemas | Pass |
| Repository worked example | Pass |
| Application repos untouched | Pass |
| No discovery reports | Pass |

## Reviewer decision

- [ ] Approved
- [ ] Approved with conditions (list in review notes)
- [ ] Changes requested
- [ ] Deferred

Reviewer: _________________  Date: _____________
