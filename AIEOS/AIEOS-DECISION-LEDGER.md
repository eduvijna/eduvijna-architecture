# AIEOS — Decision Ledger

**Purpose:** Consolidated index/summary of major architecture decisions for EduVijna AIEOS  
**Nature:** Index only — **does not replace ADRs or EDRs**

---

## Authority note

Conflict preference:

1. Approved architecture decisions (authoritative detail lives in ADR/EDR files)  
2. Approved product / engineering documents  
3. Current source code / contracts  
4. AIEOS orientation documents

Do not invent decision IDs. Use verified ADR/EDR IDs when available.

---

## Ledger

| ID | Decision | Status | Related EBP | Related ADR/EDR | Notes |
|----|----------|--------|-------------|-----------------|-------|
| ADR-042 | Teacher OS Shell owns UX, not business capabilities | Accepted | EBP-001.1 | ADR-042 · EDR-002 | Generators/reports/analytics stay in existing modules |
| ADR-043 | Stable foundations before features (Foundation → Hardening → Review → Next) | Accepted | EBP-001 / EBP-000 | ADR-043 | Avoid feature→feature→refactor |
| ADR-044 | AI Platform behind stable product services; frontend never calls Agents/MCP directly | Accepted | EBP-001 | ADR-044 | Mission/Intent/Artifact façades; provider independence |
| ADR-045 | Teaching Intent owns goals; generators are capabilities | Accepted | EBP-001.3 | ADR-045 | Constitutional for Intent UX |
| ADR-046 | One Artifact status lifecycle for every type | Accepted | EBP-001.5+ | ADR-046 | Draft→…→Archived; no exceptions |
| ADR-047 | Outcome-first teaching language (Prepare Tomorrow) | Accepted | EBP-001.3 | ADR-047 | Not generator-menu primary model |
| ADR-048 | Review Queue owns approval (teacher judgement only); Approved ≠ Published | Accepted | EBP-001.5 | ADR-048 | Queue does not own generation/orchestration |
| EDR-001 | Use React Context for session-scoped Continuous Context | Accepted | EBP-001.6 | EDR-001 | Implementation only; PA session semantics unchanged |
| EDR-002 | Teacher OS Shell foundation implementation choices | Accepted | EBP-001.1 | EDR-002 | Flag provider, keep MainLayout, additive routes |
| — | Teacher OS uses existing Quiz-React repository (no new app) | Approved (EBP-001) | EBP-001 | — | Evolve existing eduvijna-web |
| — | Teacher OS consumes stable product services | Approved | EBP-001 | ADR-044 | Mocks today; real APIs later |
| — | Frontend does not directly call Agents/MCP | Approved | EBP-001 | ADR-044 | Binding for Teacher OS |
| — | Continuous Context is distinct from Teacher OS Context (shell selection) and Teacher Memory | Approved (PA + EDRs) | EBP-001.6 / 001.8 | EDR-001 · EDR-002 | Different domains |
| — | Teacher/School Context is not Teacher Memory | Approved (EBP-001.8 package) | EBP-001.8 | — | Read surface only |
| — | Existing generators should remain operational | Approved (EBP-001) | EBP-001 | ADR-042 | Behind flags / legacy paths |
| — | EBP-001.9 needs durable Content SoR for production Review Queue | Discovery finding | EBP-001.9 | — | Scenario C: generic SoR missing |
| — | DB creation for Content SoR is NOT yet authorized | Under architecture review | EBP-001.9 | — | Persistence preflight: DB CHANGE REQUIRED; no migrate authorization |
| EBP-000 | Engineering Constitution v1.0 frozen | Frozen | EBP-000 → EBP-001 | — | Implementation standards; not an ADR |

---

## ADR catalogue (Teacher OS set)

Authoritative records: `eduvijna-architecture/decisions/`

| ADR | Title |
|-----|-------|
| ADR-042 | Teacher OS Shell owns UX |
| ADR-043 | Stable foundations before features |
| ADR-044 | AI Platform behind stable product services |
| ADR-045 | Teaching Intent owns goals |
| ADR-046 | Artifact Status Lifecycle |
| ADR-047 | Outcome-first Prepare Tomorrow |
| ADR-048 | Review Queue owns approval |

Chronological index: `decisions/DECISION_LOG.md`

---

## EDR catalogue (implementation)

Authoritative records: `eduvijna-product/engineering/edrs/`

| EDR | Title |
|-----|-------|
| EDR-001 | Continuous Context via React Context |
| EDR-002 | Teacher OS Shell Foundation |

---

## Explicit non-decisions (do not treat as authorized)

| Topic | Status |
|-------|--------|
| Create generic `edu.contents` / Content SoR DB | Not authorized — under architecture review |
| Introduce Agents / MCP into Teacher OS frontend | Rejected by ADR-044 |
| Treat EBP-001.8 as Teacher Memory | Explicitly not |
| Treat discovery recommendations as ADR | Invalid — discovery ≠ ADR |
| Rename Teacher OS to AIEOS | Invalid — Teacher OS remains subsystem name |

---

## Related

- `AIEOS-MASTER-CONSTITUTION.md`
- `AIEOS-CURRENT-STATE.md`
- `AIEOS-ARCHITECTURE-JOURNEY.md`
