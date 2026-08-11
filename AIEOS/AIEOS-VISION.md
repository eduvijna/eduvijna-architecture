# AIEOS — Vision

**Product:** EduVijna  
**Full identity:** Artificial Intelligence Engineering Education Operating System (**AIEOS**)  
**Founder:** Sreekanth  
**Architecture role:** Chief AI Enterprise Architect — ChatGPT

---

## Authority note

This document describes long-term product vision using existing repository documentation as primary source material, under the approved AIEOS product identity.

It does **not** replace ADRs, EDRs, EBP documents, APIs, schemas, or code.

Conflict preference:

1. Approved architecture decisions  
2. Approved product / engineering documents  
3. Current source code / contracts  
4. AIEOS orientation documents

**CURRENT / PLANNED / FUTURE / DEFERRED** labels below prevent future concepts from being read as committed implementation.

---

## Identity stack

```text
EduVijna
   ↓
Artificial Intelligence Engineering Education
Operating System
(AIEOS)
   ↓
Teacher OS  (subsystem — current major delivery program)
```

AIEOS is the **complete** product. Teacher OS is a **subsystem**.

EduVijna is an AI-engineered education operating system — not merely a collection of AI features.

---

## North Star outcomes (from product North Star)

Primary mission (product docs): give teachers time and clarity to teach well.

Headline metric direction: **Hours returned to teaching per teacher per week.**  
Twin trust proxy: **% of AI outputs approved with ≤2 edits.**

Product promise (Teacher OS lens): prepare with confidence; teach with clarity; follow up with insight — in less time than today.

---

## Conceptual areas

### Teacher OS — CURRENT major program

**Status:** CURRENT (Wave 1 foundation in progress / partially delivered)

Teacher OS is the teacher-centric operating subsystem for teaching life:

- Today's Mission briefing (not menu-first home)
- Outcome navigation: Today · Prepare · Teach · Assess · Improve · Library · AI Assistant · Settings
- Teaching Intent (goals; generators are capabilities — ADR-045)
- Continuous Context (session / Intent thread — distinct from Teacher Memory)
- Review Queue (approval cockpit — ADR-048)
- Teacher/School Context read surfaces (not Teacher Memory)
- Daily Learning Loop as the compounding teaching cycle (product architecture)

Wave 1 philosophy: vertical-slice foundation on existing `Quiz-React` + `eduvijna-api`, behind feature flags.

### Student-facing capabilities

**Status:** FUTURE / DEFERRED relative to Wave 1

Product architecture defines Student OS boundaries (consume Published/Assigned artefacts; practice; personal learning view).  
EBP-001 explicitly out of scope for Student OS.

### School / administrative capabilities

**Status:** PLANNED / FUTURE (product phases); Admin / Principal boundaries documented

- **Admin Console / ERP** — school tenancy, master data, School Context supply (product boundaries).
- **Principal OS** — school academic health / adoption oversight (later product phase).
- School Context inheritance is a core Teacher OS abstraction (read surface CURRENT in EBP-001.8; deeper institutional completeness FUTURE).

### AI Engineering Platform

**Status:** CURRENT platform capabilities exist; FUTURE sophistication planned behind stable services

Approved architecture (**ADR-044**): Teacher OS talks only to stable application services.  
AI Orchestrator, Agents, MCP, LLM providers, and tool routing are backend / platform internals.

Existing Platform AI / content generation capabilities are to be **reused**, not rewritten into the Teacher OS shell (**ADR-042**).

### AI provider abstraction

**Status:** PLANNED / FUTURE (architecture intent)

ADR-044 benefits include provider independence (swap / multi-provider routing without UI rewrite).  
Detailed Provider Gateway specification beyond ADR-044: **Pending formal architecture decision** / not treated as authorized Wave 1 build.

### RAG

**Status:** FUTURE / not established as an authorized AIEOS Wave 1 commitment in current EBP-001 evidence

Product/AI opportunity material may discuss retrieval-augmented assistance; Wave 1 blueprint does not authorize a RAG platform programme.

### Evaluation

**Status:** PLANNED / FUTURE (product Daily Loop / Assess depth)

Assisted evaluation and assessment loops appear in product vision and later roadmap phases.  
Not the current EBP-001.9 focus.

### Policies / governance

**Status:** CURRENT (architecture + engineering governance)

- Architecture decisions via ADRs  
- Engineering Constitution (EBP-000)  
- Feature flags, security/tenancy standards, review packages  
- Architecture precedes implementation  

### Workflows

**Status:** CURRENT conceptual model; PARTIAL implementation

Canonical Teacher OS flow (product architecture):

```text
Teacher → Teaching Intent → Continuous Context → Capability Orchestration
        → Review Queue → Ready to Publish → Publish (explicit)
```

Wave 1 has delivered UI spine pieces; full orchestration depth and durable generate→queue wiring remain incomplete (see Current State).

### Future Agents

**Status:** FUTURE / DEFERRED — not currently authorized for premature introduction

May exist later **behind** stable product services (ADR-044). Frontend must not call Agents directly.

### Future MCP

**Status:** FUTURE / DEFERRED — not currently authorized for premature introduction

Same boundary as Agents (ADR-044).

### Future Orchestration

**Status:** PLANNED product concept; DEFERRED as full Prepare multi-artefact orchestration depth in EBP-001

Orchestration *entry points* and Intent→capabilities model are architectural (ADR-042 / ADR-045).  
Full orchestration engines and multi-artefact Prepare depth are later EBP work.

### Future intelligent education capabilities

**Status:** FUTURE vision (PA roadmap Phases 2–5)

Product roadmap phases (not sprint commitments):

1. Teacher OS Foundation  
2. Teaching Assistant depth  
3. Student Intelligence (teacher-mediated)  
4. School Intelligence  
5. Parent Intelligence  

These are vision sequencing — not automatic authorization.

### Teacher Memory

**Status:** DEFERRED (product-defined; not implemented as true Memory)

Teacher Memory is a durable, teacher-owned profile that compounds.  
EBP-001.8 delivered Teacher/School Context **read surfaces** and explicitly is **not** Teacher Memory.

### Publishing

**Status:** DEFERRED relative to current Review Queue slices

**Approved ≠ Published** (ADR-048). Publish/assign is an explicit later step; out of scope for EBP-001.5 Review Queue approval semantics.

---

## What AIEOS is not

- Not “Teacher OS only”
- Not a chatbot bolted onto ERP
- Not a content dump or generator zoo as the primary mental model
- Not autonomous publisher to students/parents
- Not an excuse to introduce Agents/MCP/new DBs/new repos without architecture authorization

---

## Related

- `AIEOS-NORTH-STAR.md`
- `AIEOS-MASTER-CONSTITUTION.md`
- `AIEOS-CURRENT-STATE.md`
- `AIEOS-ROADMAP.md`
- Product sources: `eduvijna-product/vision/*`, `eduvijna-product/product-architecture/*`
- Architecture sources: `eduvijna-architecture/decisions/ADR-042` … `ADR-048`
