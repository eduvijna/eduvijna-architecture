---
id: ADR-044
title: AI Platform behind stable product services
owner: EduVijna Enterprise Architecture Office · Product Architecture
status: approved
version: 1.0.0
created: 2026-08-10
last_updated: 2026-08-10
reviewers:
  - Founder / Product Architecture
  - Principal Software Engineer
---

# ADR-044 — AI Platform Behind Stable Product Services

**Status:** Approved  
**Date:** 2026-08-10  
**Related:** ADR-042 · ADR-045 · PA-001 · EBP-000 · FEATURE_BOUNDARIES.md  
**Note:** Outcome-first “Prepare Tomorrow” language lives in **ADR-047**. Artifact lifecycle is **ADR-046**.

---

## Context

Teacher OS and the AI platform will grow more sophisticated (orchestrators, agents, MCP tools, multiple LLM providers). If the frontend binds to agents, MCP, or a specific model vendor, every AI evolution forces UI rewrites and leaks implementation into the product surface.

## Decision

**Teacher OS communicates only with stable application services** — for example:

- `MissionService`
- `IntentService`
- `ArtifactService`
- (and later peer product services as defined by architecture)

**AI Orchestrator, Agents, MCP servers, LLM providers, and tool routing remain internal implementation details of the backend.**

**The frontend must never call an agent or MCP tool directly.**

```text
Teacher OS (React)
        ↓
Stable application services (Mission / Intent / Artifact / …)
        ↓
Backend AI Platform (internal)
        ↓
Orchestrator · Agents · MCP · LLM providers · tool routing
```

## Benefits

1. **Provider independence** — Replace Gemini with OpenAI, Anthropic, local models, or multi-provider routing **without changing the UI**.  
2. **Incremental intelligence** — Evolve from simple APIs to a sophisticated multi-agent system **behind** the same service contracts.  
3. **Preserve product architecture** — Teaching Intent, Mission, Review Queue, and Shell stay stable while the AI platform becomes more capable (ADR-042 / ADR-045).

## Consequences

**Positive**

- Clear frontend/backend AI boundary  
- Aligns with shell owning UX, not generators (ADR-042)  
- Enables ADR-045 Intent → services → capabilities without UI coupling to agents  

**Negative / follow-ups**

- Backend must publish stable service contracts and version them carefully  
- Observability of AI internals stays server-side (no agent IDs in Teacher OS UI unless productized as status copy)

## Alternatives considered

1. **Frontend calls MCP / agents directly** — Rejected: couples UX to AI topology; blocks provider swaps.  
2. **UI talks only to a single “LLM chat” endpoint** — Rejected: collapses Intent/Mission/Artifact product model into chat plumbing.

## Compliance

- Quiz-React / Teacher OS must depend on application service façades only (mocks today, real APIs later).  
- New features that import agent/MCP clients into the web app are **non-compliant**.  
- **EBP-003** (if/when defined) is out of scope of this ADR authoring pass — do not invent or modify EBP-003 here.

## Out of scope

- Implementing the AI platform itself  
- EBP-003  
- Changing existing Platform AI module internals in this decision record
