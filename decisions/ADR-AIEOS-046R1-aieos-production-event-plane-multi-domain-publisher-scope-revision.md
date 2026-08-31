---
id: ADR-AIEOS-046R1
title: AIEOS Production Event Plane Multi-Domain Publisher Scope Revision
owner: EduVijna Enterprise Architecture Office · Chief AI Enterprise Architect
status: approved
version: 1.0.0
created: 2026-08-31
last_updated: 2026-08-31
reviewers:
  - Chief AI Enterprise Architect
  - Founder / Product Architecture
---

# ADR-AIEOS-046R1 — AIEOS Production Event Plane Multi-Domain Publisher Scope Revision

**Status:** Frozen / Approved  
**Founder / Product Architecture approval date:** 2026-08-31  
**Related:** [ADR-AIEOS-046](ADR-AIEOS-046-aieos-production-event-plane-identity-least-privilege-contract.md) · [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) · [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) · [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md)

**Catalogue note:** Frozen / Approved is **ARCHITECTURE AUTHORITY ONLY**. This ADR is a **NARROW FORWARD REVISION** of [ADR-AIEOS-046](ADR-AIEOS-046-aieos-production-event-plane-identity-least-privilege-contract.md). It **MUST NOT** rewrite or replace the historical ADR-AIEOS-046 body. It narrowly supersedes the production EVENT publisher domain ACL embodied in ADR-AIEOS-046 **A46-INV-03** and any equivalent Content-only publisher wording. All other ADR-AIEOS-046 invariants remain binding unless this revision explicitly restates them unchanged. ADR-AIEOS-046 remains historical base authority. ADR-AIEOS-046R1 becomes **current authority** for production EVENT publisher domain scope. Do **not** authorize production NATS provisioning, production credential creation, production stream creation, DigitalOcean mutation, App Platform deployment, Backend EVENT dispatcher changes, production database access, production dispatcher execution, commercial purchase, or production deployment.

Evidence SHAs at deposit (read-only gate):

- Architecture `origin/main`: `a17aba7d1386f211bd185c14475b09dbf1545b26`
- Backend `origin/main`: `f93c74bcb7f6dc98235e160330db517809a93185` (read-only evidence only)
- Infrastructure `origin/main`: `39e4306b28e29283c2096a2bb981221c8dfc2b98` (read-only evidence only)

---

## Context

[ADR-AIEOS-046](ADR-AIEOS-046-aieos-production-event-plane-identity-least-privilege-contract.md) froze the production EVENT plane identity and least-privilege contract, including:

- production stream `AIEOS_EVENTS_PROD`
- production stream subjects `io.eduvijna.aieos.>`
- EVENT publisher PUB authority limited to `io.eduvijna.aieos.content.>`
- one workload-specific EVENT dispatcher identity
- separate `streamadmin`
- JWT + NKey + `.creds` via `AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS`

[ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) requires TeachingAssignment mutation events and transactional event/audit intent under the Teaching domain. Those events require the Teaching event namespace:

```text
io.eduvijna.aieos.teaching....
```

ADR-AIEOS-046 previously limited the production EVENT publisher identity to Content subjects only. Therefore this revision adds the minimum Teaching-domain publisher permission required without granting platform-wide event publishing authority.

Publication, TeachingAssignment, external delivery, learner attempt, submission, and grading remain distinct authorities.

---

## Decision

### Revision relationship and precedence

ADR-AIEOS-046R1 is a **narrow forward revision** of ADR-AIEOS-046.

| Concern | Historical base authority | Current authority |
|---------|----------------------------|-------------------|
| Production stream name | ADR-AIEOS-046 | ADR-AIEOS-046 (unchanged) |
| Production stream subjects | ADR-AIEOS-046 | ADR-AIEOS-046 (unchanged) |
| EVENT publisher identity model | ADR-AIEOS-046 | ADR-AIEOS-046 (unchanged) |
| EVENT publisher PUB domain scope | ADR-AIEOS-046 `io.eduvijna.aieos.content.>` only | **ADR-AIEOS-046R1** closed multi-prefix set below |
| Stream administration separation | ADR-AIEOS-046 | ADR-AIEOS-046 (unchanged) |
| Credential / auth / transport | ADR-AIEOS-046 | ADR-AIEOS-046 (unchanged) |

The ADR-AIEOS-046 historical body remains unchanged and authoritative for all non-superseded concerns.

### Production stream (unchanged)

Freeze exactly:

```text
Stream:
AIEOS_EVENTS_PROD

Subjects:
io.eduvijna.aieos.>
```

Stream coverage is **not** EVENT publisher authority. Subjects under `io.eduvijna.aieos.>` that are outside the authorized publisher domain set remain denied to the EVENT publisher.

### Production EVENT publisher PUB authority (superseded scope)

The production EVENT publisher PUB authority is exactly the currently authorized domain set:

```text
PUB:
io.eduvijna.aieos.content.>
io.eduvijna.aieos.teaching.>
```

These two PUB permissions are an **EXPLICIT CLOSED SET** for current authority.

Do **not** authorize:

```text
io.eduvijna.aieos.>
```

as a general publisher wildcard.

Do **not** authorize:

```text
>
```

or unrelated domain subjects.

Do not grant future domains implicitly. Future domain publication authority requires separate governed expansion.

### Production EVENT publisher SUB authority (unchanged)

```text
SUB:
_INBOX.>
```

plus only the NATS response semantics genuinely required for JetStream publish acknowledgements.

### Subject / event-type alignment

CloudEvent `type` continues to equal the JetStream publish subject. Canonical broker/event namespace remains:

```text
io.eduvijna.aieos.<domain>...
```

Production broker routing must remain:

```text
CloudEvent type / publisher subject
        ↓
io.eduvijna.aieos.<domain>....
        ↓
EVENT publisher PUB ACL
(closed domain set)
        ↓
production stream coverage
io.eduvijna.aieos.>
```

Do not introduce a second broker-routing naming scheme.

### EVENT publisher identity (unchanged)

The same workload-specific EVENT dispatcher identity remains the production publisher identity. Do **not** create one publisher identity per domain in this revision.

The EVENT publisher must **not** receive:

```text
$JS.API.>
```

Stream administration remains a separate `streamadmin` identity per ADR-AIEOS-046.

### TeachingAssignment event family

Freeze the initial TeachingAssignment event namespace exactly:

```text
io.eduvijna.aieos.teaching.assignment.created.v1
io.eduvijna.aieos.teaching.assignment.due_updated.v1
io.eduvijna.aieos.teaching.assignment.closed.v1
io.eduvijna.aieos.teaching.assignment.cancelled.v1
```

These are Teaching-domain events. They **MUST NOT** be disguised as:

```text
io.eduvijna.aieos.content....
```

They **MUST NOT** reuse:

```text
content.published.v1
```

as assignment truth.

### Authentication, secret delivery, and transport (unchanged)

Production EVENT broker authentication remains JWT + NKey + `.creds`.

The single production secret environment variable remains:

```text
AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS
```

Secret delivery remains DigitalOcean App Platform encrypted environment material; runtime consumption remains in-memory via `user_jwt_cb` + `signature_cb`.

TLS, private endpoint, and certificate verification remain mandatory.

### Producer correctness (unchanged)

Transactional outbox + at-least-once semantics remain unchanged per ADR-AIEOS-025 and ADR-AIEOS-046.

### Important implementation consequence

Future Backend implementation must **NOT** represent production EVENT publisher authority as one global prefix such as:

```text
io.eduvijna.aieos.
```

Current authority is a closed multi-prefix / subject-set contract:

```text
io.eduvijna.aieos.content.>
io.eduvijna.aieos.teaching.>
```

Implementation may represent this as an explicit collection/set of authorized prefixes/subjects.

This architecture deposition does **not** modify Backend code.

---

## Binding invariants

| ID | Invariant |
|----|-----------|
| R1-INV-01 | Production stream remains exactly `AIEOS_EVENTS_PROD`. |
| R1-INV-02 | Production stream subjects remain exactly `io.eduvijna.aieos.>`. |
| R1-INV-03 | Production EVENT publisher PUB authority is exactly the currently authorized domain set: `io.eduvijna.aieos.content.>` and `io.eduvijna.aieos.teaching.>`. |
| R1-INV-04 | The EVENT publisher does **NOT** receive `io.eduvijna.aieos.>` publisher authority. |
| R1-INV-05 | EVENT publisher SUB remains `_INBOX.>` plus only required publish-ack response semantics. |
| R1-INV-06 | EVENT publisher still has no `$JS.API.>` stream-administration authority. |
| R1-INV-07 | Stream administration remains a separate `streamadmin` identity. |
| R1-INV-08 | The same workload-specific EVENT dispatcher identity remains the production publisher identity; no one publisher identity per domain in this revision. |
| R1-INV-09 | Production broker authentication remains JWT + NKey + `.creds`. |
| R1-INV-10 | Production secret remains exactly `AIEOS_EVENT_DISPATCHER_NATS_CREDENTIALS`. |
| R1-INV-11 | Secret delivery remains DigitalOcean App Platform encrypted environment material; runtime consumption remains in-memory. |
| R1-INV-12 | TLS, private endpoint, and certificate verification remain mandatory. |
| R1-INV-13 | CloudEvent type equals JetStream subject. |
| R1-INV-14 | Transactional outbox + at-least-once semantics remain unchanged. |
| R1-INV-15 | No unrelated future AIEOS domain publisher authority is implied. |

ADR-AIEOS-046 invariants not superseded by this revision remain in force.

---

## Explicit non-authorizations

This approval/deposition does **NOT** authorize:

```text
production NATS mutation
production NATS user/credential regeneration
production credential rotation
stream creation
stream update
stream deletion
DigitalOcean mutation
App Platform mutation
deployment
production EVENT execution
Infrastructure implementation
Backend implementation
Backend event dispatcher changes
TeachingAssignment application/API implementation
OpenAPI changes
Frontend changes
LMS integration
production database mutation
commercial purchase
production rollout
```

**TOS-DEV06-I03** remains **NOT AUTHORIZED** until this architecture revision is merged and post-merge closed.

---

## Consequences

- Catalogue and current-state surfaces must present ADR-AIEOS-046 as historical/base production EVENT plane contract and ADR-AIEOS-046R1 as current publisher-domain-scope authority.
- Future Infrastructure source may encode the revised closed multi-prefix publisher ACL without creating production resources.
- Future Backend EVENT dispatcher source must represent authorized publisher domains as an explicit prefix/subject set, not as one global `io.eduvijna.aieos.` prefix.
- TeachingAssignment events must use the Teaching namespace; they must not masquerade as Content events.
- ADR-AIEOS-046 historical body remains unchanged.

---

## Related ADRs

| ID | Relationship |
|----|--------------|
| [ADR-AIEOS-046](ADR-AIEOS-046-aieos-production-event-plane-identity-least-privilege-contract.md) | Historical/base production EVENT plane contract; A46-INV-03 publisher scope superseded for current authority by this revision |
| [ADR-AIEOS-053](ADR-AIEOS-053-aieos-teaching-assignment-classroom-delivery-authority.md) | Requires TeachingAssignment mutation events; motivates minimum Teaching-domain publisher permission |
| [ADR-AIEOS-025](ADR-AIEOS-025-aieos-api-contract-integration-implementation-baseline.md) | CloudEvents / JetStream / outbox baseline |
| [ADR-AIEOS-027](ADR-AIEOS-027-aieos-generic-content-implementation-baseline.md) | Content event types under `io.eduvijna.aieos.content.*` |
