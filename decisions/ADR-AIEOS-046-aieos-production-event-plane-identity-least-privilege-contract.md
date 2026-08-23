---
id: ADR-AIEOS-046
title: AIEOS Production Event Plane Identity & Least-Privilege Contract
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-23
last_updated: 2026-08-23
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-046 — AIEOS Production Event Plane Identity & Least-Privilege Contract

**Status:** Frozen / Approved  
**Date:** 2026-08-23  
**Related:** [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) · [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) · [ADR-AIEOS-044](ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) · [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md)

**Catalogue note:** Frozen / Approved is architecture authority only. It does **not** authorize production NATS provisioning, production credential creation, production stream creation, DigitalOcean mutation, App Platform deployment, Backend EVENT dispatcher daemon implementation, production database access, production dispatcher execution, commercial purchase, or production deployment.

Chief-facing alignment contract approved on **2026-08-23**.

Evidence SHAs at deposit (read-only gate):

- Architecture `origin/main`: `55c070357f1d51e564ea49276ff191b2baef98da`
- Infrastructure `origin/main`: `1249634403cacd9caec4ba48b72821e629b222f5`
- Backend `origin/main`: `36710be8a63636de3b063b44c08819d6c0468137` (read-only evidence/reference only)

---

## Context

[ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) established:

- CloudEvents 1.0 semantics
- NATS JetStream as the event broker
- transactional outbox publication
- at-least-once delivery
- least-privilege workload identities
- broker ≠ SoR
- domain code never publishes directly to NATS

ADR-AIEOS-025 contained earlier modular-first examples such as:

```text
AIEOS_EVENTS
aieos.event.v1.>
```

Those historical examples remain in the ADR-025 historical body and must not be erased. They are **not** current production stream/subject authority.

Later implemented AIEOS event contracts (including Content) use the namespace:

```text
io.eduvijna.aieos.<domain>...
```

Content event types are published as JetStream subjects in the form:

```text
io.eduvijna.aieos.content.<aggregate>.<fact>.v1
```

Examples:

```text
io.eduvijna.aieos.content.content.created.v1
io.eduvijna.aieos.content.content.version_created.v1
io.eduvijna.aieos.content.content.submitted_for_review.v1
io.eduvijna.aieos.content.content.review_approved.v1
io.eduvijna.aieos.content.content.review_changes_requested.v1
io.eduvijna.aieos.content.content.review_rejected.v1
io.eduvijna.aieos.content.content.published.v1
io.eduvijna.aieos.content.content.archived.v1
```

Production runtime and infrastructure design now require one aligned production namespace and identity boundary. ADR-046 freezes that **production specialization and authority boundary**.

**Precedence:** ADR-046 has current precedence for production stream name, stream subjects, EVENT publisher ACL, credential secret contract, authentication scheme, secret delivery, and stream-administration separation. Do not rewrite ADR-AIEOS-025's historical body solely to erase history.

Canonical AIEOS broker/event subject namespace:

```text
io.eduvijna.aieos.*
```

Production routing must **not** introduce or retain as current authority:

```text
aieos.event.v1.*
```

Production EVENT stream name must **not** be:

```text
AIEOS_EVENTS
```

---

## Decision

### Production stream

Freeze exactly:

```text
Stream:
AIEOS_EVENTS_PROD

Subjects:
io.eduvijna.aieos.>
```

The stream is:

- infrastructure-owned
- production environment specific
- created/verified before EVENT runtime activation
- never created by the EVENT dispatcher
- never automatically repaired or administered by the EVENT runtime

If required production stream configuration is absent or incompatible:

```text
EVENT publishing fails closed
        ↓
existing outbox retry/quarantine semantics
```

Do not silently create another stream.  
Do not fall back to Core NATS.  
Do not create `AIEOS_EVENTS` as production authority.

### Subject / event-type alignment

CloudEvent type is the JetStream publish subject. Production broker routing must remain:

```text
CloudEvent type / publisher subject
        ↓
io.eduvijna.aieos.content....
        ↓
EVENT publisher PUB ACL
io.eduvijna.aieos.content.>
        ↓
production stream coverage
io.eduvijna.aieos.>
```

Do not introduce a second broker-routing naming scheme. Do not translate `io.eduvijna.aieos....` into `aieos.event.v1....` inside future EVENT runtime.

### EVENT publisher identity

Freeze one workload-specific NATS identity for the production EVENT dispatcher. It is separate from:

- API
- WORKFLOW dispatcher
- Temporal worker
- migrator
- database deployment administrator
- NATS stream administrator
- any human/operator identity

EVENT publisher authority:

```text
PUB:
io.eduvijna.aieos.content.>

SUB:
_INBOX.>
```

plus only the NATS response semantics required for publish acknowledgements.

The EVENT publisher must **not** receive:

```text
$JS.API.>
```

stream create/update/delete authority.

It must not receive:

- consumer administration
- server administration
- account administration
- subscribe-all
- publish-all
- unrelated domain publisher authority
- stream-management authority
- `>` wildcard authority

Receiving or publishing an event remains separate from AIEOS business authorization.

Stream coverage is not EVENT publisher authority: subjects under `io.eduvijna.aieos.>` that are outside `io.eduvijna.aieos.content.>` remain denied to the EVENT publisher.

### Stream administrator

Freeze a separate:

```text
streamadmin
```

NATS user/identity.

Purpose:

- governed stream creation
- governed stream configuration
- governed stream verification
- explicitly authorized maintenance

`streamadmin` is:

```text
NOT EVENT runtime identity
NOT API runtime identity
NOT WORKFLOW runtime identity
NOT application business identity
```

Its credential must never be injected into the EVENT dispatcher. EVENT runtime must not be capable of assuming or switching to `streamadmin`.

### Authentication

Production EVENT broker authentication is:

```text
NATS JWT
+
NKey
+
.creds material
```

Do not use as the production baseline:

- username/password
- static bearer token
- unauthenticated NATS
- seed embedded in NATS URL
- credential committed to repository

The EVENT credential is independently rotatable.

### Secret delivery

The single production secret environment variable is:

```text
AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS
```

It contains the `.creds` material delivered through:

```text
DigitalOcean App Platform
encrypted environment secret
```

The application must later consume this material **in memory**. Runtime construction contract:

```text
encrypted env
    ↓
AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS
    ↓
parse .creds material in memory
    ↓
user_jwt_cb
+
signature_cb
    ↓
nats.connect(...)
```

Do not require production creation of a credential file on disk merely because `nats-py` supports `user_credentials=<path>`.

Do not freeze:

```text
AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS_FILE
```

as production authority. Any earlier `_FILE` proposal is superseded by ADR-046.

No credential contents may appear in logs, exception messages, config repr, CI artifacts, source, OpenTofu state, or PR comments.

### Transport

Preserve the already-approved production connectivity contract:

```text
private broker endpoint
+
TLS required
+
certificate verification required
```

Forbidden:

```text
tls=false
verify=false
plaintext production connection
public unauthenticated broker endpoint
```

Exact private endpoint/DNS value remains environment configuration. Do not freeze an ephemeral IP.

### Producer correctness

Preserve:

```text
authoritative domain mutation
+
transactional outbox intent
        ↓
COMMIT
        ↓
EVENT dispatcher
        ↓
JetStream publish
        ↓
broker ACK
        ↓
mark outbox PUBLISHED
```

Publisher sets the JetStream deduplication identity from the stable event ID according to the existing Backend publisher contract. At-least-once remains the correctness model. Broker deduplication does not create exactly-once business semantics.

### Environment isolation

`AIEOS_EVENTS_PROD` is the production stream. Do not share it with local development, unit tests, integration tests, or preview environments. Test/disposable stream names are implementation/test concerns and must not be presented as production authority.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| A46-INV-01 | Production EVENT stream name is exactly `AIEOS_EVENTS_PROD`. |
| A46-INV-02 | Production stream subjects are exactly `io.eduvijna.aieos.>`. |
| A46-INV-03 | EVENT publisher PUB is exactly `io.eduvijna.aieos.content.>`. |
| A46-INV-04 | EVENT publisher SUB is `_INBOX.>` plus only publish-acknowledgement response semantics. |
| A46-INV-05 | EVENT runtime has no `$JS.API` stream-administration authority. |
| A46-INV-06 | Stream administration uses a separate `streamadmin` identity. |
| A46-INV-07 | Production auth is JWT + NKey `.creds` only. |
| A46-INV-08 | Production secret env is exactly `AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS`. |
| A46-INV-09 | Secret delivery is App Platform encrypted env; runtime materialization is in-memory callbacks. |
| A46-INV-10 | `_FILE` credential path is not production authority. |
| A46-INV-11 | Absent/incompatible stream → fail closed into outbox retry/quarantine; no Core NATS fallback. |
| A46-INV-12 | `aieos.event.v1.*` and `AIEOS_EVENTS` are not current production authority. |
| A46-INV-13 | CloudEvent type equals JetStream subject; no second routing scheme. |
| A46-INV-14 | TLS + private endpoint + certificate verification required for production. |
| A46-INV-15 | Production stream is not shared with local/test/preview environments. |

---

## Explicit non-authorizations

ADR-046 does **not** authorize:

```text
production NATS provisioning
production credential creation
production stream creation
DigitalOcean mutation
App Platform deployment
Backend daemon implementation
production DB access
production dispatcher execution
commercial purchase
production deployment
```

It freezes only the contract that later implementation/provisioning must satisfy.

Backend EVENT dispatcher implementation remains **NOT YET AUTHORIZED**.

---

## Consequences

- Infrastructure source may encode and verify this contract without creating production resources.
- Future Backend EVENT dispatcher source must consume `AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS` in memory via `user_jwt_cb` + `signature_cb`.
- Historical ADR-025 modular-first stream/subject examples remain historical; current summaries and production config must follow ADR-046.
- Catalogue and current-state surfaces must present `AIEOS_EVENTS_PROD` / `io.eduvijna.aieos.>` as production authority.

---

## Related ADRs

| ID | Relationship |
|----|--------------|
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | CloudEvents / JetStream / outbox baseline; historical modular-first stream examples superseded for production by this ADR |
| [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | Content event types under `io.eduvijna.aieos.content.*` |
| [ADR-AIEOS-037](ADR-AIEOS-037-aieos-production-infrastructure-baseline.md) | AIEOS-operated NATS JetStream on private DigitalOcean infrastructure |
| [ADR-AIEOS-044](ADR-AIEOS-044-aieos-bootstrap-production-preapply-execution-baseline.md) | NATS Bootstrap / private networking pre-apply freezes |
| [ADR-AIEOS-045](ADR-AIEOS-045-aieos-dispatcher-tenant-candidate-discovery-authority.md) | EVENT dispatcher candidate discovery; daemon still not authorized |
