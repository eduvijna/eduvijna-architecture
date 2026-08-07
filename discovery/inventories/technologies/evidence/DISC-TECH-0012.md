# Evidence Summary — DISC-TECH-0012

**Technology:** `Python`
**Source repository:** `eduvijna-api`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** language

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-python-eduvijna-api |
| name | Python |
| category | language |
| version_observed | 3.11 |
| usage_context | Application language; Dockerfile base image python:3.11-slim. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| dockerfile | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__Dockerfile.txt` | FROM python:3.11-slim. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api-languages.json` | GitHub languages API lists Python as dominant language. |
| requirements-txt | `discovery/inventories/repositories/evidence/_raw/eduvijna-ai__eduvijna-api__requirements.txt.txt` | Python dependency manifest requirements.txt present. |

## Unknowns

- `technology.support_status`
