---
id: ADR-043
title: Stable foundations before features
owner: EduVijna Enterprise Architecture Office · Product Architecture
status: approved
version: 1.0.0
created: 2026-08-10
last_updated: 2026-08-10
reviewers:
  - Founder / Product Architecture
  - Principal Software Engineer
---

# ADR-043 — Stable Foundations Before Features

**Status:** Accepted  
**Date:** 2026-08-10  
**Related:** EBP-000 Vertical Slice Delivery · EBP-001 · ENGINEERING_CONSTITUTION.md

---

## Context

Rapid product pressure can produce a sequence of feature PRs that never harden the platform. That creates fragile Teacher OS surfaces, repeated shell edits, and expensive refactors after teachers are already using the product.

## Decision

Future engineering slices **must** follow this order:

```text
Foundation
    ↓
Hardening
    ↓
Review
    ↓
Next Capability
```

### Forbidden pattern

```text
Feature → Feature → Feature → Refactor
```

“Refactor later” after a stack of unhardened features is **non-compliant**.

### What each stage means

| Stage | Meaning |
|-------|---------|
| **Foundation** | Structural slice that later work builds on (e.g. Shell, Mission landing chrome, Intent contract) |
| **Hardening** | Tests, a11y, flags, telemetry, performance, empty/error paths, rollback verification |
| **Review** | Architecture / engineering review package; approval before the next capability |
| **Next Capability** | Only then start the next vertical teacher outcome |

Vertical-slice delivery (EBP-000) still applies *within* each capability: User Story → React → Backend → Tests → Review → Deploy. ADR-043 governs **sequencing across capabilities**.

## Consequences

**Positive**

- Stable Teacher OS chrome before heavy orchestration  
- Review packages remain meaningful gates  
- Reduces debt that blocks Pilot/GA  

**Negative / follow-ups**

- Throughput of new capabilities is intentionally paced  
- Product roadmap must schedule hardening, not only features  

## Alternatives considered

1. **Feature velocity first, harden at Pilot** — Rejected: Pilot users inherit instability; rollback cost rises.  
2. **Big-bang platform rewrite then features** — Rejected: contradicts vertical-slice and backward-compatibility principles.

## Compliance

EBP task plans and sprint breakdowns must show Foundation → Hardening → Review before opening the next capability epic.
