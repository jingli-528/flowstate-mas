# FlowState: Product Decision Log

This log records how FlowState's product decisions were made: the options considered, the evidence, what was traded away, and what would make us change course. It goes alongside [PRD.md](../PRD.md) and the checklist results in [self-evaluation.md](self-evaluation.md).

**Conventions**
- **Append-only.** Entries are never deleted or rewritten. When a decision changes, a new entry is added (`Supersedes D-00X`) and the old one is marked `Superseded by D-00Y`.
- **Status values:** Proposed · Accepted · Superseded.
- **Every PRD revision** bumps the PRD version, adds a row to the revision history, and adds an entry for each material change. The PRD references entries inline (e.g. *D-001*).
- **Revisit trigger:** each entry names the evidence that would reopen it, so decisions are tested rather than defended.

---

## Decision index

| ID | Date | Decision | Status | PRD sections affected |
|---|---|---|---|---|
| D-001 | 2026-11-27 | Target industry: automotive Tier-1 suppliers (JIT/JIS) | Accepted | 1 (problem, persona, worth solving) |
| D-002 | 2026-11-27 | Position as plant-level recovery decision that complements visibility and planning platforms | Accepted | 1 (competition, moat), W5 (GTM) |
| D-003 | 2026-11-27 | MVP narrowed to one persona, one job, one loop | Accepted | 1 (persona), 2 (stories), W2 (roadmap) |
| D-004 | 2026-11-27 | Solver decides feasibility, not the LLM; every write needs human approval in the MVP | Accepted | 2 (flow, agents), W3, W4 (RAI) |
| D-005 | 2026-11-27 | North Star = time-to-approved-recovery-plan | Accepted | 1 (metrics) |
| D-006 | 2026-11-27 | Repo Markdown is the PRD's source of truth | Accepted | All (format) |
| D-007 | 2026-11-27 | First draft scope: Week 1 written in full, Weeks 2–5 preliminary | Accepted | W2–W5 |
| D-008 | 2026-11-27 | Agent decomposition: 6 agents + deterministic verifier + learning loop | Accepted | 2 (architecture) |
| D-009 | 2026-11-27 | Explicit kill / pivot criteria tied to discovery interviews | Accepted | Assumptions |
| D-010 | 2026-11-27 | GTM wedge: free shadow-mode design-partner pilots | Accepted | W5 |

## Revision history (how the design changed per iteration)

| PRD version | Date | Decisions introduced | What changed in the PRD | Commit |
|---|---|---|---|---|
| v0.1 | 2026-11-27 | D-001 to D-010 | First draft from the course spreadsheet idea. Industry chosen; scope narrowed from "plant-to-logistics flow optimization" to "line-down recovery decision"; Week 1 written in full; Weeks 2–5 outlined. After the PRD self-eval: primary metrics cut from 5 to 2 (quality metrics split out); validation plan, kill criteria and GTM wedge added. | [2c85e8d](https://github.com/jingli-528/flowstate-mas/commit/2c85e8d) |

---

## Entries

### D-001: Target industry: automotive Tier-1 suppliers
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Context.** The original idea covered "manufacturing" generally. Course feedback (Mahesh Yadav) was to pick one specific industry. The goal is a problem painful enough to be commercially viable, ideally mission-critical, where minutes matter.

**Options considered**

| Option | For | Against |
|---|---|---|
| **Automotive Tier-1 (JIT/JIS)** | Highest measured downtime cost ($2.3M/hr, Siemens 2024); suppliers pay expedited freight; daily disruptions; one clear operator persona; synthetic data is easy to build | Crowded with planning and visibility vendors |
| Automotive Tier-2 / mid-market | Less competition; priced out of enterprise suites | Smaller budgets, lower digital maturity, weaker data access |
| Medical devices / pharma | Patient-safety stakes; strong audit-trail need | FDA/GxP validation slows autonomous AI; long buying cycles; hard 6-week MVP |
| Electronics (EMS) | Chronic component shortages; very messy multi-customer data | Pain is less "stop-the-line" urgent |
| Aerospace & defense | Very high cost per hour (AOG, final assembly) | Few, slow buyers; ITAR/export-control data limits |

**Decision.** Automotive Tier-1 suppliers shipping just-in-time / just-in-sequence to OEM assembly plants.

**Rationale and evidence.** Automotive has the highest cost per hour of unplanned downtime in Siemens' 2024 study, 2× its 2019 level, with explicit knock-on effects "down the supply chain" ([Siemens TCOD 2024](https://assets.new.siemens.com/siemens/assets/api/uuid:1b43afb5-2d07-47f7-9eb7-893fe7d0bc59/TCOD-2024_original.pdf)). Buyers generally expect suppliers to pay expedited freight, which can cost tens of thousands of dollars a day by air ([Michigan Bar Journal, 2019](https://www.michbar.org/file/barjournal/article/documents/pdf4article3774.pdf)). The pain lands on a specific budget owner.

**Trade-offs.** We take on a crowded neighborhood in exchange for the sharpest, best-documented pain. Tier-2 mid-market stays a later expansion.

**Consequences.** Persona, scenarios, data model (EDI 830/862/866, sequence slots) and competitive analysis are all automotive-specific.

**Revisit trigger.** Fewer than 3 of 10 discovery interviewees can name a recent disruption that cost premium freight or a chargeback (*D-009*).

---

### D-002: Positioning: plant-level recovery decision, complementary to incumbents
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Context.** A sanity check asked whether project44 and others already offer autonomous multi-agent systems for this problem.

**Findings.**
- project44 launched six logistics-side agents on April 8, 2026: freight procurement, disruption management, network ops, exceptions, slot booking and carrier onboarding. It orchestrates "across our own agents and the broader ecosystem" ([project44](https://www.project44.com/press-releases/project44-launches-ai-agent-portfolio-at-decision44-built-on-a-decade-of-context-skills-and-orchestration/)). None are production-scheduling agents.
- Blue Yonder launched production planning and scheduling at ICON (May 2026) that produces "feasible plans that account for real material and capacity availability" ([diginomica](https://diginomica.com/icon-2026-blue-yonders-cognitive-platform-keeps-taking-shape-and-customers-are-starting-see-results)). This is the closest threat.
- Kinaxis Maestro Agents work "with human-in-the-loop guardrails," and production-scheduling agents are "real today" in recommend-and-approve mode ([Tellius](https://www.tellius.com/resources/blog/agentic-ai-in-supply-chain-use-cases-platforms-and-whats-shipping-2026)).

**Options considered.** (a) Compete head-on as an agentic planning suite. (b) Build another logistics-visibility layer. (c) **Own the seam**: the minutes-scale plant recovery decision that joins shop-floor state, in-transit freight and contract exposure, integrating with incumbents.

**Decision.** (c). Position as "line-down recovery decision engine, not a planning suite."

**Rationale.** Each incumbent covers one side of the seam. A plant-level, minutes-scale decision with solver verification and contract-cited costing is not what any of them publicly offers today. project44's open orchestration turns a competitor into a possible channel.

**Trade-offs.** We depend on incumbents' data and APIs, and a partner could later build the same capability.

**Revisit trigger.** Blue Yonder, Kinaxis or project44 ships a plant-execution recovery agent that combines live shop-floor state with freight and contract context. Reviewed quarterly.

---

### D-003: MVP scope: one persona, one job, one loop
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Context.** The spreadsheet idea promised cross-silo data reconciliation, departmental KPI negotiation, closed-loop troubleshooting and UNS integration. The idea self-evaluation failed it on "one persona" and "one core job" ([self-evaluation.md](self-evaluation.md)).

**Options considered.** (a) Keep the broad "synchronized flow optimization" vision as the MVP. (b) Logistics-side only (ETA exceptions). (c) **Disruption → verified, costed recovery plan → approve → execute**, for the plant materials manager.

**Decision.** (c). KPI trade-offs survive as configurable *policy weights* inside ranking (US-009), not as a separate negotiation product.

**Trade-offs.** We defer multi-plant optimization, auto-execution and real integrations. The demo is less sweeping but credible in 6 weeks.

**Revisit trigger.** The MVP loop is demo-ready before week 5, which leaves room to pull P1 stories in.

---

### D-004: Deterministic verification and human approval gate
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Context.** In a plant, a confident but infeasible plan is worse than no plan. LLMs can produce plausible schedules that violate capacity or sequence rules.

**Options considered.** (a) LLM generates and judges plans. (b) LLM generates, and an LLM critic judges. (c) **LLM generates; a deterministic constraint solver judges feasibility; costs come only from rate cards and contracts; a human approves every write.**

**Decision.** (c). Autonomy expands later per playbook class only after launch thresholds are met.

**Rationale.** It separates creativity (LLM) from correctness (solver). It is auditable. It matches vendor guidance that "high-consequence actions require approval before execution" ([HiveMQ](https://www.hivemq.com/blog/unified-namespace-agentic-ai/)) and the market norm of "recommend; approve to release."

**Trade-offs.** More engineering (constraint modeling) and a slower path to "autonomous" claims.

**Revisit trigger.** After 3+ months in production, a playbook class shows ≥ 95% approve-as-is with zero safety incidents. That class becomes a candidate for auto-execute.

---

### D-005: North Star = time-to-approved-recovery-plan (TTARP)
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Options considered.** (a) Line-down minutes avoided. (b) Premium freight $ avoided. (c) **TTARP.**

**Decision.** (c) as North Star; (a) and (b) as primary outcome metrics.

**Rationale.** TTARP is directly controlled by the product, measurable from the first pilot week, and leads the money metrics, because faster decisions keep cheaper options available. Line-down minutes and freight depend on factors outside the product (supplier failures, OEM schedule changes) and are measured over quarters.

**Trade-offs.** TTARP could be gamed by fast but poor decisions, so it is paired with the feasibility guardrail and override tracking.

**Revisit trigger.** Pilot data shows TTARP improving while freight or line-down outcomes do not.

---

### D-006: Repo Markdown as the PRD's source of truth
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Options considered.** (a) Word .docx from the course template. (b) Claude Doc (inline comments, export to Word/PDF/Google Docs, no automatic GitHub sync; sharing outside an organization depends on account settings). (c) **Markdown in this repo.**

**Decision.** (c), with a .docx export on request for formal submission.

**Rationale.** Anyone can open a public repo link without an account. Git history shows how the product thinking evolved, which supports this log. Mermaid diagrams render on GitHub. The PRD lives next to the prototype code.

**Trade-offs.** Reviewers without GitHub accounts can't comment inline, and it is less polished than a formatted doc.

**Revisit trigger.** Course submission requires the template document format.

---

### D-007: First draft scope: Week 1 in full, Weeks 2–5 preliminary
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Options considered.** (a) Week 1 only. (b) **Week 1 in full plus outlines for Weeks 2–5.** (c) Full PRD now.

**Decision.** (b).

**Rationale.** The outlines show scalability thinking (roadmap, model choice, Responsible AI, pricing) for the pitch without front-loading work the later lessons will reshape.

**Revisit trigger.** None. Each week's lesson completes its section.

---

### D-008: Agent decomposition
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Options considered.** (a) A single agent with many tools. (b) **Specialized agents (intake, impact, planner, commercial, orchestrator, execution) plus a deterministic verifier tool and an offline learning loop.** (c) A fully decentralized agent swarm negotiating per department.

**Decision.** (b).

**Rationale.** Each agent has one job, a clear input/output contract and its own eval, which is what makes component-level risk assessment (Week 2) and evals (Week 3) possible. A central orchestrator keeps one accountable decision brief. The verifier is a tool, not an agent, because it must be deterministic. Option (c) is harder to evaluate and to explain to a plant manager.

**Trade-offs.** More inter-agent plumbing than a single agent, and less "emergent" than a swarm.

**Revisit trigger.** Evals show handoff losses between agents (context dropped) outweigh specialization benefits.

---

### D-009: Kill / pivot criteria
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Context.** The idea evaluation noted that kill criteria were missing. Key PRD numbers (baseline time-to-plan, cost per incident) are still assumptions.

**Decision.** The three criteria listed in the PRD's Assumptions section: median time-to-plan < 45 min → pivot to the costing step only; < 3/10 interviewees with a costly recent disruption → revisit *D-001*; no plant grants read-only access → rethink the data entry point.

**Rationale.** Pre-commits to the evidence that would change direction, so interview results drive decisions instead of confirming them.

**Revisit trigger.** Discovery interviews are complete. Record the outcome as a new entry.

---

### D-010: GTM wedge: shadow-mode design-partner pilots
**Status:** Accepted · **Date:** 2026-11-27 · **PRD version:** v0.1

**Context.** The idea evaluation flagged a missing user-acquisition and monetization plan.

**Options considered.** (a) Direct enterprise sales of a paid pilot. (b) **Free 90-day read-only shadow pilot at 2–3 plants, converting on measured results.** (c) Channel-first through platform marketplaces.

**Decision.** (b) first, then system integrators and platform agent ecosystems as channels.

**Rationale.** Shadow mode needs only read access, which lowers the trust barrier (*D-004*). It also produces the baseline data that replaces the PRD's assumptions and builds the decision→outcome library that is the core moat.

**Trade-offs.** No revenue during pilots, and it relies on partners investing engineering time.

**Revisit trigger.** Pilot plants decline to convert after 90 days despite hitting TTARP targets, which would mean the willingness-to-pay assumption is wrong.
