# Decisions — Overview

## Purpose

Record significant architecture and engineering decisions for the EduVijna ecosystem in a durable, reviewable form.

## Scope

- Architecture Decision Records (ADRs) governed by EAO process
- The chronological decision log summarising approved decisions
- Traceability from decisions to architecture, standards, and roadmap work

## Ownership

EduVijna Enterprise Architecture Office (EAO). Decision authors may include domain architects; publication remains under EAO stewardship.

## Contents

- `DECISION_LOG.md` — chronological summary of approved architecture decisions
- Individual ADRs when authorised and approved

### Teacher OS (2026-08-10)

| ID | Title |
|----|-------|
| [ADR-042](ADR-042-teacher-os-shell-owns-ux.md) | Shell owns UX — not generators/business logic |
| [ADR-043](ADR-043-stable-foundations-before-features.md) | Foundation → Hardening → Review → Next Capability |
| [ADR-044](ADR-044-ai-platform-behind-stable-services.md) | AI Platform behind stable product services |
| [ADR-045](ADR-045-teaching-intent-owns-goals.md) | Teaching Intent owns goals — generators are capabilities (**constitutional**) |
| [ADR-046](ADR-046-artifact-status-lifecycle.md) | Artifact Status Lifecycle — one lifecycle for every artifact |
| [ADR-047](ADR-047-outcome-first-prepare-tomorrow.md) | “Help me prepare tomorrow” — outcome-first language |
| [ADR-048](ADR-048-review-queue-owns-approval.md) | Review Queue owns approval — teacher judgement only |

## Exclusions

- Informal chat decisions without recorded process
- Standards text itself (see `standards/`)
- Detailed design specifications unless captured as a decision record
- Discovery findings that have not been converted into decisions
- Engineering implementation choices that do **not** change architecture (see product repo `engineering/edrs/` — EDRs)
