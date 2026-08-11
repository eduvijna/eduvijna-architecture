# AIEOS — Master Constitution

**Product:** EduVijna — Artificial Intelligence Engineering Education Operating System (AIEOS)  
**Scope:** Permanent constitutional principles governing AIEOS  
**Nature:** Orientation / constitutional layer — **not** an ADR or EDR

---

## Authority hierarchy

This constitution orients AIEOS. It does **not** replace ADRs, EDRs, EBP documents, APIs, schemas, or code.

When conflicts exist:

1. Approved architecture decisions (ADRs)  
2. Approved product / engineering documents (including frozen EBP-000 Engineering Constitution where applicable)  
3. Current source code / contracts  
4. AIEOS orientation documents (this file and siblings)

Where a policy is not yet formally decided, it is marked: **Pending formal architecture decision.**

---

## 1. Product identity

- Product name: **EduVijna**
- Full identity: **Artificial Intelligence Engineering Education Operating System (AIEOS)**
- AIEOS is the complete product vision and product system identity.

---

## 2. Founder / architecture roles

- **Founder:** Sreekanth
- **Architecture role:** Chief AI Enterprise Architect — ChatGPT
- Architecture stewardship for AIEOS orientation and major architectural direction remains with the architecture role; implementation executors do not redefine approved architecture unilaterally.

---

## 3. AIEOS definition

AIEOS is EduVijna as an **AI-engineered education operating system**: a coherent product and platform system for education work — not a loose set of AI demos, generators, or disconnected features.

Teacher OS, Student-facing capabilities, School/administrative capabilities, and the AI Engineering Platform are conceptual areas within or under AIEOS as evidenced by product/architecture documentation — with clear CURRENT / PLANNED / FUTURE / DEFERRED distinctions (see `AIEOS-VISION.md`).

---

## 4. Product hierarchy

```text
EduVijna AIEOS  (complete product)
   └── Teacher OS  (subsystem — current major program)
   └── Student / Parent / Principal / Admin surfaces  (product architecture boundaries; not AIEOS-complete substitutes)
   └── AI Engineering Platform  (behind stable product services — ADR-044)
```

**Teacher OS is a subsystem of AIEOS. Teacher OS is not the complete AIEOS product.**

---

## 5. Architecture-first principle

Architecture precedes implementation.

- Approved product architecture and ADRs bind delivery.
- Code adapts to architecture; architecture does not silently bend to convenience PRs.
- Conflicts require architecture / product review before implementation continues.
- Aligns with EBP-000 Engineering Constitution: Preserve Architecture; vertical-slice delivery under approved boundaries.

---

## 6. Source-of-truth principle

| Concern | Primary home |
|---------|----------------|
| Architectural governance & ADRs | `eduvijna-architecture` |
| Product vision & Teacher OS product architecture | `eduvijna-product` |
| Teacher OS web implementation | `Quiz-React` (eduvijna-web) |
| Backend / API / domain services | `eduvijna-api` |

AIEOS orientation documents in this folder are a permanent orientation layer over those sources — not a parallel conflicting SoT.

---

## 7. Repository responsibility boundaries

| Repository | Responsibility |
|------------|----------------|
| **eduvijna-architecture** | Enterprise architecture governance, ADRs, reviews, discovery, standards stewardship, AIEOS orientation |
| **eduvijna-product** | Product vision, PA artefacts, EBP blueprints, engineering constitution / standards / EDRs (implementation decisions that do not change architecture) |
| **Quiz-React** | Teacher-facing web app / Teacher OS shell and UX |
| **eduvijna-api** | Application APIs, domain services, flags, persistence contracts |

Do not create a new application repository for Teacher OS. Teacher OS evolves existing EduVijna apps (EBP-001).

---

## 8. Service ownership

Verified approved boundaries include:

- **Teacher OS Shell owns UX**, not business capabilities (**ADR-042**).
- **Teaching Intent owns goals**; generators are capabilities (**ADR-045**).
- **Review Queue owns approval** — teacher judgement only (**ADR-048**).
- Teacher OS communicates with **stable application services** (e.g. Mission / Intent / Artifact façades), not AI internals (**ADR-044**).

Additional service ownership details not covered by approved ADRs: **Pending formal architecture decision.**

---

## 9. AI architecture boundaries

Per **ADR-044**:

- Frontend must **not** call Agents, MCP tools, or LLM providers directly.
- Orchestrator, Agents, MCP, LLM providers, and tool routing remain **backend / AI platform** concerns behind stable product services.
- Provider independence and incremental intelligence evolve behind service contracts.

Premature introduction of Agents / MCP / Orchestration into Teacher OS UI or as unapproved platform jumps is non-compliant.

---

## 10. Security and tenancy

Verified engineering expectations (EBP-000 / Security Standards):

- Use existing JWT / authorization patterns — do not invent parallel auth.
- Enforce school / tenant isolation on Artifact / Work / Queue queries.
- Feature flags must not grant privilege escalation.
- AI outputs require explicit teacher approval before publish (Constitution AI Review gate; ADR-046 / ADR-048).

Broader enterprise security model beyond current Teacher OS Wave 1 standards: **Pending formal architecture decision** where not already recorded in approved standards/ADRs.

---

## 11. Data ownership

Verified distinctions from product architecture / EDRs:

- **Continuous Context** — session / Intent thread (not durable Teacher Memory).
- **Teacher/School Context** — institutional / shell selection / school identity surfaces (EBP-001.8 read surface ≠ Teacher Memory).
- **Teacher Memory** — durable teacher-owned preferences / learned patterns (product vision; **deferred** as implementation).
- **Content SoR** — durable content / artifact system of record required for production Review Queue integration (EBP-001.9 discovery); creation **not authorized** by this constitution.

Exact Content SoR schema and ownership model: under architecture review / **Pending formal architecture decision.**

---

## 12. Lifecycle integrity

Per **ADR-046**: one Artifact status lifecycle for every Artifact type:

`Draft → Generating → Generated → In Review → Approved → Published → Archived`

- No type-specific alternate state machines.
- **Approved ≠ Published** (**ADR-048**).
- AI-produced student/parent-facing outputs must not skip human review.

---

## 13. Feature flags and safe rollout

Per EBP-000 / EBP-001:

- Teacher OS surfaces ship behind feature flags (default off in production until entitled rollout).
- Additive changes preferred; classic EduVijna paths remain when flags are off.
- Rollback by flag disable is a first-class safety control.

---

## 14. Testing and architecture review

- No merge without automated validation appropriate to the slice (EBP-000 Testing First).
- Material architecture / product architecture changes require Architecture / Product Architecture Review.
- Vertical slices: User Story → React → Backend → Tests → Review → Deploy (flagged).

---

## 15. Cursor implementation rules

- Cursor is an **implementation executor**.
- Cursor must not independently redefine approved architecture.
- Cursor must inspect and reuse existing capabilities before creating duplicates.
- Cursor must not introduce Agents, MCP, Orchestration, Teacher Memory, new databases, or new repositories unless authorized by approved architecture decisions and the active blueprint.
- Major decisions discovered during implementation must be escalated for documentation (ADR if architectural; EDR if implementation-only).

---

## 16. ADR / EDR governance

- **ADRs** record significant architecture decisions (home: `eduvijna-architecture/decisions/`).
- **EDRs** record implementation choices that do **not** change architecture (home: `eduvijna-product/engineering/edrs/`).
- If a choice would change architecture → ADR / Product Architecture update — not an EDR.
- This constitution does **not** create ADRs or EDRs.

---

## 17. EBP governance

- Engineering Blueprints (EBP-*) define how approved architecture is delivered in vertical slices.
- **EBP-000** Engineering Constitution v1.0 is **frozen** for Teacher OS implementation standards.
- **EBP-001** is the Teacher OS Foundation Wave 1 blueprint.
- Implementation follows blueprint + constitution + ADRs; discovery recommendations are **not** automatic implementation authorization.

---

## 18. Backward compatibility

- Never break existing schools (EBP-000).
- Classic landing, generators, ERP, and auth remain operational when Teacher OS flags are off.
- Prefer additive change; migrations must be reversible or dual-read where required.

---

## 19. Avoiding unnecessary duplication

- Inspect existing repositories, services, generators, and APIs before creating parallel systems.
- Teacher OS Shell must not re-implement generator / report / analytics engines (**ADR-042**).
- Prefer reuse of Platform AI and existing content capabilities under Intent / Review Queue.

---

## 20. Avoiding premature database changes

- Major database redesign is out of scope for EBP-001 Wave 1 blueprint as written.
- EBP-001.9 persistence preflight determined **DB CHANGE REQUIRED** for a durable Content SoR path.
- **Database creation / migration for Content SoR is NOT authorized by this constitution.**
- Record: **Persistence architecture is under architecture review.**

---

## 21. Agents / MCP / Orchestration introduction criteria

Agents, MCP, and Orchestration may be introduced only when:

1. Approved architecture (ADR / PA) authorizes the capability boundary, **and**
2. They remain behind stable product services (ADR-044), **and**
3. The active EBP / review explicitly scopes the work, **and**
4. Frontend does not call them directly.

Until then: **deferred / not currently authorized** for Teacher OS Wave 1 beyond documented orchestration *entry points* and product concepts.

Exact introduction criteria beyond ADR-044: **Pending formal architecture decision.**

---

## 22. Teacher OS boundary

Teacher OS:

- Is the teacher-centric operating subsystem for teaching life (Mission, Teaching Intent, Continuous Context, Review Queue, Daily Loop surfaces).
- Owns UX shell concerns per ADR-042; does not own business capability engines.
- Does **not** equal AIEOS in full (Student / Parent / Principal / Admin / broader AI platform remain distinct conceptual areas).

Out of Wave 1 scope (EBP-001): Student OS / Parent OS / Principal OS; new AI models/generators; full Prepare Tomorrow multi-artefact orchestration depth; major DB redesign; platform extraction.

---

## 23. Future evolution principle

AIEOS will evolve through sequenced product phases and EBPs (Teacher OS Foundation → deeper Teaching Assistant → Student / School / Parent intelligence per PA roadmap), while:

- Preserving architecture-first discipline  
- Reusing existing capabilities  
- Documenting major decisions  
- Avoiding premature platform complexity  

Future concepts in vision documents are **vision**, not automatic implementation commitments.

---

## Related orientation documents

- `AIEOS-NORTH-STAR.md`
- `AIEOS-VISION.md`
- `AIEOS-CURRENT-STATE.md`
- `AIEOS-ARCHITECTURE-JOURNEY.md`
- `AIEOS-DECISION-LEDGER.md`
- `AIEOS-ROADMAP.md`
