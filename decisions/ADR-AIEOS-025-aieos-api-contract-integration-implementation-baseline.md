---
id: ADR-AIEOS-025
title: "AIEOS API Contract & Integration Implementation Baseline"
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-18
last_updated: 2026-08-18
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-025 — AIEOS API Contract & Integration Implementation Baseline

**Status:** Frozen / Approved  
**Date:** 2026-08-18  
**Related:** [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) · [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) · [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) · [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md)

**Catalogue note:** Frozen / Approved is architecture status. It is not implemented, merged, production-authorized, or deployed.

---

## Context

AIEOS needs one synchronous API contract family and one reliable integration/event family so domains do not invent ad-hoc HTTP, messaging, or retry semantics. The event broker is selected here; it was **not** selected by [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md).

## Decision

### Synchronous API

- REST / JSON
- `/api/v1`
- OpenAPI 3.1
- Stable `operationId`
- RFC 9457 Problem Details
- Stable machine-readable AIEOS error codes
- ETag + If-Match
- HTTP 412 for stale revision
- HTTP 428 when a required precondition is missing
- `Idempotency-Key` with a transactionally coordinated idempotent outcome
- Opaque cursor pagination
- Stable keyset semantics
- No implicit snapshot guarantee
- Typed frontend clients generated from an approved OpenAPI contract

### Events

- CloudEvents 1.0 semantics
- UUIDv7 event identity
- Versioned event types
- Explicit tenant context
- Correlation and causation
- Actor provenance
- Minimal business payload
- No reusable authorization snapshots inside events

### Transport

- NATS JetStream
- `AIEOS_EVENTS` logical stream baseline
- Replay-oriented retention
- Durable pull consumers
- Explicit ACK
- Bounded retry / MaxDeliver
- At-least-once processing
- No global ordering assumption
- Aggregate revision used for freshness/order

### Reliable publication

In the authoritative transaction:

- authoritative domain mutation
- PostgreSQL transactional outbox
- required audit intent (where required)

**Domain code never publishes directly to NATS.**

### Consumer reliability

- Durable consumer
- Inbox / dedupe
- Idempotent processing
- Durable side effect committed before broker ACK
- Broker deduplication is an optimization only, not the SoR

### Failure / recovery

- Finite retry
- Explicit failure classification
- Durable quarantine
- Owned remediation
- Controlled replay
- MaxDeliver is not itself business quarantine
- Replay preserves event identity

### External integration

- Explicit integration / anti-corruption adapters
- Authenticated inbound webhooks
- External-event dedupe
- Ambiguous external outcomes require reconciliation / provider idempotency
- Never blind retry

## Binding invariants

- Workload-specific NATS identity and least privilege.
- Receiving an event is not authorization to act.
- API ≠ Event.
- Event ≠ Workflow.
- Workflow ≠ Domain State.
- Broker ≠ SoR.
- Legacy API is not an AIEOS runtime dependency.

## Explicit non-goals / deferred decisions

- Asset HTTP / binary delivery contract is **not** frozen by this ADR.
- Asset events / Asset outbox are **not** frozen ([ADR-AIEOS-036](ADR-AIEOS-036-asset-authorization-transactional-security-audit-baseline.md) explicitly defers them).
- Production broker topology, hosting, and deployment remain independently unauthorized.

## Consequences

- Integration adapters own NATS publication; domains own mutation + outbox intent.
- Consumers must be idempotent; at-least-once is expected.
- OpenAPI remains the frontend contract source; clients do not bind to undocumented HTTP.

## Related ADRs

| ID | Relationship |
|----|----------------|
| [ADR-AIEOS-022](ADR-AIEOS-022-aieos-platform-technology-baseline.md) | Technology family; event broker was deferred there |
| [ADR-AIEOS-024](ADR-AIEOS-024-aieos-data-resource-sor-implementation-baseline.md) | Transactional mutation + outbox/audit intent |
| [ADR-AIEOS-026](ADR-AIEOS-026-aieos-workflow-implementation-baseline.md) | Workflow ≠ event ≠ domain state |
| [ADR-AIEOS-028](ADR-AIEOS-028-security-audit-mutation-accountability.md) | Audit ≠ event |
