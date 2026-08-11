# AIEOS — Architecture Journey

**Purpose:** Chronological record of AIEOS architecture evolution  
**Rule:** Do not invent historical facts. Where evidence is missing: *Not established by current repository evidence.*

---

## Authority note

Conflict preference:

1. Approved architecture decisions  
2. Approved product / engineering documents  
3. Current source code / contracts  
4. AIEOS orientation documents

---

## 1. AIEOS product vision

| Field | Content |
|-------|---------|
| **Objective** | Establish EduVijna as an Artificial Intelligence Engineering Education Operating System (AIEOS) — complete product identity — with Teacher OS as a subsystem. |
| **Architectural reason** | Orient the ecosystem as an education OS, not a feature collection; keep architecture ahead of implementation. |
| **What was implemented** | Product vision / Teacher OS PA docs, North Star, Engineering Constitution, ADRs 042–048, EBP-001 blueprint; AIEOS orientation folder (this permanent knowledge foundation). |
| **What was deliberately NOT implemented** | Treating Teacher OS as the entire product; premature Agents/MCP/Orchestration/Memory platforms. |
| **Governing decisions** | Approved project identity (AIEOS); ADR-042…048; EBP-000; PA-001 / TLM-001 foundations. |
| **Current status** | Vision and governance active; AIEOS orientation documents established in `eduvijna-architecture/AIEOS/`. |

---

## 2. Teacher OS foundation

| Field | Content |
|-------|---------|
| **Objective** | Define Teacher OS as the teacher-centric operating subsystem (Mission, Intent, Memory, School Context, Daily Loop, Review Queue). |
| **Architectural reason** | Replace tool-zoo mental model with Intent → orchestration → Review Queue; preserve existing capability engines. |
| **What was implemented** | Product architecture package PA-001; feature boundaries; Continuous Context model; Artifact model; EBP-001 Wave 1 blueprint targeting `Quiz-React` + `eduvijna-api`. |
| **What was deliberately NOT implemented** | New Teacher OS application repository; Student/Parent/Principal OS in Wave 1; major DB redesign in Wave 1 scope. |
| **Governing decisions** | PA-001; FEATURE_BOUNDARIES; ENGINEERING_CONSTITUTION; later ADR-042…048. |
| **Current status** | Foundation approved as product/architecture direction; Wave 1 delivery in progress. |

---

## 3. EBP-001.1 — Teacher OS Shell

| Field | Content |
|-------|---------|
| **Objective** | Ship flag-gated Teacher OS chrome (shell, nav, routes, extension slots) without breaking classic EduVijna. |
| **Architectural reason** | Shell owns UX only; additive rollout; reusable foundation before features (ADR-042 / ADR-043 / EDR-002). |
| **What was implemented** | TeacherShell, TeacherOsRoutes, TeacherNavigation, TeacherOsFlagProvider, TeacherOsContext selections, telemetry façade; API `teacher_os_enabled` flag exposure. |
| **What was deliberately NOT implemented** | Generators inside shell; Mission as forced production default landing; Review badge live data; AI/Memory. |
| **Governing decisions** | ADR-042; EDR-002; EBP-000; EBP-001 Sprint 0. |
| **Current status** | APPROVED / CLOSED (EBP-001.9 discovery status). |

---

## 4. EBP-001.2 — Today's Mission

| Field | Content |
|-------|---------|
| **Objective** | Provide Today's Mission landing as the teacher briefing experience. |
| **Architectural reason** | Mission-first home vs module tile menu; compose day briefing through MissionService façade (ADR-044 service boundary). |
| **What was implemented** | TodayMissionPage and Mission cards; mock MissionService adapter path. |
| **What was deliberately NOT implemented** | Full ERP/timetable mission aggregate API as hard dependency; durable AI-prepared kits. |
| **Governing decisions** | PA Today's Mission; ADR-044; EBP-001 Sprint 1. |
| **Current status** | APPROVED / CLOSED as Mission UX slice; Mission data still mock-backed (partial vs blueprint ERP depth). |

---

## 5. EBP-001.3 — Teaching Intent

| Field | Content |
|-------|---------|
| **Objective** | Capture teacher goals via Teaching Intent experience (Prepare outcome language). |
| **Architectural reason** | Teaching Intent owns goals; generators are capabilities (ADR-045 / ADR-047). |
| **What was implemented** | Intent landing + wizard UX; mock Intent adapter. |
| **What was deliberately NOT implemented** | Orchestration; Review Queue; Agents; MCP; APIs; DB; AI generation. |
| **Governing decisions** | ADR-045; ADR-047; EBP-001.3. |
| **Current status** | APPROVED / CLOSED (UX). |

---

## 6. EBP-001.4 — Intent → Preparing Bridge

| Field | Content |
|-------|---------|
| **Objective** | Bridge Continue from Intent into Preparing Kit / Review Queue entry experience. |
| **Architectural reason** | Keep Intent → kit → Review path coherent without claiming full orchestration. |
| **What was implemented** | PreparingKitPage and Review Queue entry bridge; mock artifacts. |
| **What was deliberately NOT implemented** | Real content generation; full Review Queue (landed as 001.5); custom free-text Intent (deferred). |
| **Governing decisions** | ADR-045; EBP-001.4 review package notes. |
| **Current status** | APPROVED / CLOSED as bridge UX; generation not wired. |

---

## 7. EBP-001.5 — Review Queue

| Field | Content |
|-------|---------|
| **Objective** | Deliver Review Queue as approval cockpit (teacher judgement). |
| **Architectural reason** | Review Queue owns approval; Approved ≠ Published; one queue for all Artifact types (ADR-048 / ADR-046). |
| **What was implemented** | Review Queue list/detail UX; approve / reject / request-changes actions on mock Artifact service. |
| **What was deliberately NOT implemented** | Publish/assign; generation ownership; orchestration; durable Content SoR wiring. |
| **Governing decisions** | ADR-048; ADR-046; EBP-001.5. |
| **Current status** | APPROVED / CLOSED as UX/semantics slice; **not** durable generator integration. |

---

## 8. EBP-001.6 — Continuous Context

| Field | Content |
|-------|---------|
| **Objective** | Preserve session/Intent work thread across Preparing Kit and Review Queue navigation. |
| **Architectural reason** | Continuous Context is session continuity — distinct from Teacher Memory and School Context (PA + EDR-001). |
| **What was implemented** | ContinuousContext provider mounted once at Teacher OS layout; threadId shared across SPA navigation. |
| **What was deliberately NOT implemented** | Teacher Memory; AI; durable persistence; Content AI session binding depth. |
| **Governing decisions** | EDR-001; PA Continuous Context; EBP-001.6. |
| **Current status** | APPROVED / CLOSED as session React Context; not durable Memory. |

---

## 9. EBP-001.7 — Mission Service Hardening

| Field | Content |
|-------|---------|
| **Objective** | Harden MissionService as stable read-composition façade and Mission UX behaviours. |
| **Architectural reason** | Stable product services before deeper AI (ADR-043 / ADR-044); Mission must not own engines. |
| **What was implemented** | MissionService hardening (mock adapter), Continuous Context snapshot plumbing, outcome CTA / empty-state fixes evidenced in tests. |
| **What was deliberately NOT implemented** | Backend Mission aggregate as mandatory; Agents/MCP; ERP deep integration. |
| **Governing decisions** | ADR-043; ADR-044; EBP-001.7. |
| **Current status** | APPROVED / CLOSED. |

---

## 10. EBP-001.8 — Teacher / School Context

| Field | Content |
|-------|---------|
| **Objective** | Provide Teacher/School Context **read surface** using existing school identity APIs. |
| **Architectural reason** | Institutional context inheritance without claiming Teacher Memory; reuse existing `my-school` authorization/tenancy. |
| **What was implemented** | School name hydration via existing `GET /api/v1/school-management/my-school`; School/Teacher Context cards; no new DB/API. |
| **What was deliberately NOT implemented** | Teacher Memory; preferences; inferred personalization; MissionService/ContinuousContext redesign; Agents/MCP/Orchestration. |
| **Governing decisions** | EBP-001.8 package; ADR-042 shell context boundary; PA School Context vs Memory distinction. |
| **Current status** | Implemented — awaiting architecture review per product package; discovery status lists APPROVED / CLOSED for Wave 1 slice tracking. **Not** Teacher Memory. |

---

## 11. EBP-001.9 — Discovery / preflight

| Field | Content |
|-------|---------|
| **Objective** | Determine the next correct Wave 1 engineering slice after 001.8 from blueprint + repository evidence; inspect content/review/persistence contracts. |
| **Architectural reason** | Do not invent roadmap; close Wave 1 acceptance gaps before premature AI/Memory/Agents; respect ADR-044 service boundary. |
| **What was implemented** | Discovery only: Phase 0 alignment; open-question resolution; content contract preflight; persistence preflight (**DB CHANGE REQUIRED**); Content SoR verification (Scenario C — no generic Content SoR). |
| **What was deliberately NOT implemented** | Code, APIs, flags, migrations, DB creation, ADRs/EDRs, Review Queue durable integration. |
| **Governing decisions** | Existing ADR-042…048; EBP-001 acceptance criteria; discovery recommendations **not** equal to implementation authorization. |
| **Current status** | **Discovery / architecture review.** Recommended next feature: Review Queue ↔ Existing Generators Integration. **Persistence architecture is under architecture review. DB creation is NOT authorized.** |

---

## Gaps / missing chronology

Where older pre-Teacher-OS platform history (earlier Platform AI packages, ERP modules, etc.) is relevant but not part of this AIEOS journey spine: **Not established by current repository evidence** as a fully sequenced AIEOS chronology in this folder — treat as adjacent capability history under product/API repos.
