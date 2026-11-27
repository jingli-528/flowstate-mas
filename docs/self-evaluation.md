# FlowState: Self-Evaluation Record

This file records how the idea and the PRD were checked against the course checklists, what failed, and what changed as a result. Decisions triggered by these checks are logged in [decision-log.md](decision-log.md).

| Evaluation | PRD version | Date | Result |
|---|---|---|---|
| Idea checklist (project-idea-self-evaluation-checklist) | pre-PRD (spreadsheet idea) | 2026-11-27 | Passes problem fit, not rule-based, agentic and why-not-ChatGPT. **Fails MVP scope as written** → narrowed (*D-003*) |
| PRD checklist, Week 1 (prd-self-eval-checklist) | v0.1 draft | 2026-11-27 | 16 / 16 after fixes (3 items strengthened) |

---

## 1. Idea evaluation (before the PRD)

**Input evaluated:** the spreadsheet idea, "FlowState – Autonomous Plant-to-Logistics Synchronized Flow Optimization," narrowed to automotive Tier-1 suppliers (*D-001*). **Stage:** early validation (pre-PRD, no interviews yet).

**Complexity:** High. It needs real-time event ingestion (UNS/MQTT), graph-based impact tracing, LLM planning, deterministic constraint verification, contract RAG and write-back execution across MES, ERP and freight systems. That is only feasible in 6 weeks on synthetic data with a mock MES.

**Pros:**
- Very real, costly pain: automotive downtime runs $2.3M/hour, and suppliers pay for expedited freight.
- Strong agentic fit: detect → trace → plan → verify → cost → act, under a time limit.
- Messy multi-source inputs (carrier emails, EDI, contracts, machine events) defeat rule engines.
- A clear enterprise buyer with budget (VP Mfg Ops / CSCO) and a measurable ROI.
- The decision→outcome history compounds into a proprietary data moat.

**Cons:**
- Crowded neighbors: project44 agents (Apr 2026), Blue Yonder production scheduling (May 2026).
- "Autonomous" execution in a plant is a trust barrier; a wrong write could stop a line.
- Integration-heavy: real UNS/ERP access is slow to earn.
- The spreadsheet scope (cross-department KPI negotiation + full sync) fails "one job, one loop."
- No monetization or user-acquisition plan stated in the idea. This is a critical gap for stakeholder review.

**Suggestion:** This is venture-scale if it is positioned as a **line-down recovery decision engine, not a planning suite**. MVP: one persona (plant materials manager), one job (inbound delay or machine outage → verified, costed recovery plan → approve → execute), one loop, on synthetic data. Make execution *recommend + approve* only. Have a solver, not the LLM, decide feasibility. Defer cross-plant optimization, auto-execution and real integrations. Win condition: time-to-approved-recovery-plan under 20 minutes with zero infeasible plans shown as feasible. The moat is the decision→outcome library and being trusted to write back, not the ontology alone.

### Checklist mapping

| Checklist item | Result | Evidence / change |
|---|---|---|
| **1. Problem fit:** real pain, real people | Pass | Materials managers do this by hand today; Siemens 2024 data; Michigan Bar Journal on expedited freight |
| Repeated task (weekly+) | Pass | Large plants average 25 unplanned downtime incidents/month (Siemens 2024), plus inbound delays |
| Execution problem, not information | Pass | The pain is building and executing the plan, not knowing a fact |
| **2. Not rule-based** (≥ 2 of 3) | Pass (3/3) | Unstructured inputs; novel disruptions; part × line × carrier × contract combinations explode |
| **3. Agentic:** reasoning + decisions | Pass | Generates, verifies and ranks options |
| Memory / context over time | Pass | Incident memory of past disruptions and outcomes |
| "What should I do next?" moments | Pass | Orchestrator picks the next step; escalates on low confidence |
| **4. MVP scope:** one persona | **Fail → fixed** | Spreadsheet listed buyer + user + cross-department stakeholders. Now one operator persona (*D-003*) |
| One core job | **Fail → fixed** | "Synchronize + negotiate KPIs + optimize" → one job: disruption → approved recovery plan (*D-003*) |
| One agentic loop | Pass after narrowing | detect → impact → plan → verify → cost → approve → execute → learn |
| Demo with synthetic data | Pass | Synthetic UNS stream, mock ERP/EDI/MES, sample contracts |
| **5. Why not ChatGPT:** persistent memory/workflow | Pass | Incident memory and decision log |
| Structured outputs / tracking | Pass | Decision briefs, audit trail, metrics |
| Value compounds with repeated use | Pass | Decision→outcome library improves the priors |

---

## 2. PRD evaluation (v0.1, Week 1 sections)

**Overall score: 16 / 16 items passing** (after the fixes listed below; first pass was 13/16 with 3 items marked weak).

### Section 1: Problem Definition (8/8)

| Item | Status | Notes |
|---|---|---|
| 1.1 Problem & JTBD | ✅ | Four-question recovery problem, user and buyer JTBDs, day-in-the-life scenario |
| 1.2 Persona (role, industry, needs) | ✅ | Plant Materials Manager at an automotive Tier-1 JIT/JIS plant; attribute table; buyer and secondary users; out-of-scope list |
| 1.3 Validated with data | ✅ (with caveat) | Cited Siemens 2024 and Michigan Bar Journal. **Gap:** no primary interviews yet, and the Tier-1-specific cost per incident is an assumption (A1, A2). **Fix applied:** validation plan and kill criteria added (*D-009*). |
| 1.4 Worth solving | ✅ | Cost per hour, frequency, supplier pays the expedite; honest note on OEM vs Tier-1 exposure |
| 1.5 MOAT | ✅ | Four-layer moat table with an honest strength rating per layer |
| 1.6 Agentic vs rules | ✅ | Combinatorial explosion, novel disruptions, conflicting goals, rules can't read |
| 1.7 Unstructured data + why LLMs | ✅ | Emails/messages, contract PDFs, shift notes, heterogeneous EDI/UNS feeds |
| 1.8 Differentiated from ChatGPT/Copilot/Cowork | ✅ | Dedicated paragraph plus a competitor capability table including general assistants |

### Section 2: Solution Definition (4/4)

| Item | Status | Notes |
|---|---|---|
| 2.1 Visual user flow | ✅ | Mermaid flowchart with input, system and human lanes, including the verify and approve loops |
| 2.2 AI drawbacks addressed | ✅ | Six drawbacks with mitigations: hallucination, explainability, low confidence, memory, learning, safe failure |
| 2.3 User stories | ✅ | 11 stories with acceptance criteria and P0/P1/P2 |
| 2.4 Agent capabilities & behaviour | ✅ | Architecture diagram plus a per-agent table (autonomy level, HITL trigger, error handling) and an autonomy roadmap |

### Section 3: Core Metrics (4/4)

| Item | Status | Notes |
|---|---|---|
| 3.1 North Star | ✅ | Time-to-approved-recovery-plan, < 20 min target |
| 3.2 1–2 primary metrics | ✅ (fixed) | **First pass listed 5 "primary" metrics**, more than the checklist's 1–2. **Fix:** 2 business-outcome primaries (line-down minutes, premium freight $); quality metrics moved to their own "AI quality" table |
| 3.3 Secondary metrics | ✅ | Acceptance, overrides, weekly actives, cost per disruption, onboarding time, plus guardrails |
| 3.4 Measurable over time | ✅ (with caveat) | Every metric has a unit, target and tracking source. **Caveat:** baselines for TTARP and freight are assumptions until a design partner provides history |

### Top 3 priorities for v0.2
1. **Replace assumptions with primary evidence.** Run the 8–10 problem interviews and update A1/A2 and the TTARP baseline with real numbers. This is the single biggest credibility gain.
2. **Get one real disruption story** (anonymized) from a practitioner to replace the synthetic day-in-the-life scenario.
3. **Week 2 risk checklist per component**, starting with the two High-risk components (option generation, execution tools).

### Strengths
- An honest competitive framing: it names Blue Yonder as the closest threat instead of claiming a vacant market, and turns project44 into a possible channel.
- A safety design reviewers can check: solver-verified feasibility, citation-or-"unknown," and zero unapproved writes as hard guardrails.
- Traceability: every major choice links to a decision-log entry with a revisit trigger.

### Weeks 2–5 status
Marked preliminary on purpose (*D-007*). Prioritization risk table, roadmap, model requirements, data, Responsible AI and pricing sections exist as outlines and will be scored against their checklist items in the matching week.
