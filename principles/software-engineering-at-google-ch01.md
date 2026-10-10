# Software Engineering at Google — Chapter 1: What Is Software Engineering? (v0.1)

- **Status:** Draft — study notes and candidate principles, not accepted My Engineer policy
- **Reviewed:** 2026-10-10
- **Source:** Titus Winters (author), Tom Manshreck (editor), *Software Engineering at Google* (2020), Chapter 1
- **Primary text:** https://abseil.io/resources/swe-book/html/ch01.html
- **Reading completeness:** **Entire published Chapter 1 read**, including examples, conclusion, TL;DR and endnotes
- **Thai companion:** [chapter-01.th.md](./software-engineering-at-google-ch01.th.md)
- **Method:** Original analytical paraphrase. **[SOURCE]** represents the author's argument; **[SYNTHESIS]** represents My Engineer's application. Ideas are located using the linked section headings; no long passage is copied.
- **Scope:** Engineering Foundations, with overlaps into Architecture, Delivery, Operations and Leadership
- **Important:** This is one organization's experience, not a universal or up-to-date empirical standard.

## Executive interpretation

**[SOURCE]** The chapter argues that software engineering is more than the initial creation of code: it includes sustaining and changing software throughout its **useful maintenance lifetime**, across **people and organizational scale**, while making **trade-offs** under uncertainty. Good practices depend strongly on context. Software that only needs to run once has fundamentally different maintenance economics from infrastructure expected to last decades.

**[SYNTHESIS]** My Engineer should not optimize its workflow for output volume, ceremony count or AI coding speed. It should help make *appropriate, revisable decisions* and retain the ability to deliver valuable change at sustainable cost.

## 1. Opening: programming, engineering, and sustainability

**[SOURCE — opening]** The chapter distinguishes three dimensions: **time, scale, trade-offs**. Programming creates code; engineering includes keeping it useful as circumstances and collaborators change. “Sustainable” means the organization retains the **ability** to respond to worthwhile changes—not that every possible upgrade must always be performed. Google explicitly says its scale is not everyone's scale.

**[SYNTHESIS]** Before applying a standard, ask: How long might this live? Who maintains it? What is expensive to change? A throwaway parser proof-of-concept and a long-running financial ledger should not inherit identical rigor.

**Caveat:** Expected lifetime and change frequency are forecasts, not guarantees.

## 2. Time and Change

**[SOURCE]** Projects shift from “works today” to “can still change tomorrow” when operating environments, dependencies and requirements evolve. Long-delayed upgrades become painful because assumptions accumulate, expertise is missing and the change size grows. Sustainability can justify making upgrades repeatedly manageable; however, upgrades must still be evaluated for value relative to cost.

**[SOURCE — Hyrum's Law]** Users may rely on *observable behavior* even when it was never promised in an API contract. This makes interface changes risky. Example: iteration order from hash-based collections is unspecified, yet downstream software can start relying on its observed order. An RPC consumer may accidentally depend on the serialized order.

**[SYNTHESIS]** In Technical Discovery, inspect actual consumers and behavior, not only interface specs. In Solution Design, plan compatibility tests and migration when behavior might change.

**[SOURCE — Why Not Just Aim for “Nothing Changes”?]** Even stable systems face security vulnerabilities, changing hardware economics and external dependencies. Avoiding needless change is sensible; being **unable** to change creates exposure. Long-term maintenance requires the capability and practice of adapting, rather than a demand to upgrade everything immediately.

**Critical distinction:** An unspecified behavior is **not automatically an acceptable dependency**, but observed compatibility risks still exist and need analysis.

## 3. Scale and Efficiency

**[SOURCE]** Engineering processes themselves must scale: developer time, build/test computation, codebase size, migration burden and organizational communication. A task performed repeatedly may grow disproportionately costly as systems and organizations grow; such gradual deterioration is easy to miss.

**[SOURCE — Policies That Don't Scale]** Forcing each dependent team to do its own migration spreads repeated expertise costs and coordination. Google's **Churn Rule** shifts certain migration work toward the infrastructure owners or backward-compatible updates. Long-lived feature branches also accumulate expensive reintegration overhead as more people branch.

**[SOURCE — Policies That Scale Well]** The **Beyoncé Rule** places compatibility coverage into shared CI rather than relying on ad hoc, undocumented checks known only to consumers. Shared expert assistance and knowledge forums can multiply their effects across a large organization.

**[SOURCE — Compiler Upgrade]** A historically painful compiler upgrade exposed hidden reliance on old behavior and inadequate tests. Later investment in tooling, consistency, expertise, routine upgrades and clear policies reduced repeated costs. The first upgrade can be much harder than subsequent upgrades.

**[SOURCE — Shifting Left]** Finding defects earlier—at design, review, static analysis or CI—usually makes them cheaper to resolve than after release. This is a **layered** defense, not a claim that one tool catches every defect.

**[SYNTHESIS]** Evaluate a policy not only by today's effort but also by **repeated cost per user/service/engineer**. Automate only repeated, measurable toil that is worth centralizing. Ensure critical consumer expectations are represented by tests.

**Context limits:** Churn Rule and Beyoncé Rule are Google's organizational policies, not laws that every team should adopt unchanged. Centralized migration can be infeasible across untrusted external clients or independent organizations.

## 4. Trade-offs and Costs

**[SOURCE]** Engineering decisions should have reasons, owners and an escalation path—not authority as the entire rationale. Cost includes **money, compute, people, transaction cost, opportunity cost and social impact**, not merely budget. Data are valuable, but estimates, precedent, qualitative reasoning and unmeasurable values also matter.

**[SOURCE — Markers]** The marker-supply example illustrates that tiny direct savings can impose larger hidden costs by interrupting developer work. **[SYNTHESIS]** Compare the cost of maintaining a manual approval with interruptions and delays it imposes.

**[SOURCE — Inputs to Decision Making]** Where costs are measurable, build shared, reasonable comparisons. Where they are not, use informed judgment and make uncertainty explicit. Decisions should be justified by constraints or best available evidence, not personal authority.

**[SOURCE — Distributed Builds]** Shared distributed builds reduced local build time but later obscured bloated dependencies and resource consumption. A successful optimization can create fresh incentives and second-order costs.

**[SOURCE — Deciding Between Time and Scale]** Forking may optimize local control and immediate changes, but duplicated forks make security updates and coordinated maintenance harder. General reuse offers consistency yet can impose unwanted generality. The right choice depends on lifetime, boundary and downstream obligations.

**[SOURCE — Revisiting Decisions]** New evidence can invalidate assumptions or change what was once a sound decision. Revisiting a decision and admitting mistakes are necessary engineering and leadership capabilities.

**[SYNTHESIS]** A decision record should state *why now, alternatives, cost dimensions, assumptions, owner and review triggers*. An ADR is justified for consequential choices, not every local function.

## 5. Software Engineering Versus Programming, conclusion and endnotes

**[SOURCE]** The chapter avoids claiming engineering is morally superior to programming. A short-lived utility need not carry the same testing, dependency management or deployment investment as a decades-long project. The value lies in fitting practices to context. The conclusion and TL;DR reinforce lifetime, sustainability, implicit API dependencies, scalable repeated work, evidence-informed trade-offs and ability to revise decisions.

**Footnote interpretation:** The source uses **maintenance lifetime**, not process execution time. It explicitly discusses imperfect definitions and cites related work, but a cited historical example should not be mistaken for a current benchmark.

## 6. Candidate Principles Registry (proposed only)

The names and taxonomy below are **My Engineer synthesis**, **not** formal chapter-defined principle IDs. Each needs review and independent corroboration before acceptance.

| ID | Candidate | Source anchors | When useful | Costs / failure cases |
| --- | --- | --- | --- | --- |
| **EF-G01** | Design for the Expected Lifetime of Software | Opening; Time and Change; Engineering Versus Programming | Pick rigor appropriate to maintenance duration | Lifetime predictions wrong; over-engineering short-lived work |
| **EF-G02** | Preserve the Ability to Change | Time and Change; Why Not Just Aim for Nothing Changes? | Dependency upgrades, recovery paths, evolvable modules | Upgrade and abstraction overhead; changes without benefit |
| **EF-G03** | Treat Observable Behavior as a Compatibility Risk | Hyrum's Law; Hash Ordering | Public APIs, service consumers, migrations | Mistaking all accidental behavior for an eternal contract |
| **EF-G04** | Make Recurring Work Scale Sustainably | Scale and Efficiency; Churn Rule; Compiler Upgrade | Growing codebases, migrations, shared tooling | Premature centralization; tooling complexity |
| **EF-G05** | Make Important Expectations Visible in Automated Feedback | Beyoncé Rule; Shifting Left | CI, consumer contracts, critical regressions | False coverage, expensive flaky CI, checks omitted from integration |
| **EF-G06** | Evaluate Full Costs and Revisit Decisions | Trade-offs; Distributed Builds; Forking; Revisiting Decisions | Architecture, delivery, management decisions | Analysis paralysis; false quantitative precision |

### Two worked candidates with Knowledge Model layers

#### EF-G03 — Observable Behavior and Compatibility

**Understanding**
- Definition: Consumers can form dependencies on outputs and behaviors beyond formal documentation.
- Mechanism: As clients/users grow, the chance of hidden dependence on something observable increases.
- Illustration: Unspecified order in a collection appears stable long enough for consumers to start assuming it.

**Engineering Judgment**
- Apply: API changes, response ordering, schema migrations, dependency/runtime upgrades.
- Do not over-apply: Do not freeze every accidental behavior. Evaluate affected consumers and cost to migrate.
- Trade-offs: Preserving accidental behavior slows improvement; breaking it without assessment causes regressions.
- Alternatives: Contract tests, explicit guarantees, staged migrations, compatibility windows.
- Failure modes: “Contract doesn't mention it, so no consumer will break”; treating Hyrum's Law as a ban on change.

**Evidence & Evolution**
- Primary: Chapter 1, “Hyrum's Law” and “Example: Hash Ordering” (source above).
- Independent corroboration: **Pending**; search API compatibility research and current contract-testing guidance.
- Confidence: **High** that chapter argues this; **Unassessed** for universal policy application.
- Review trigger: Real consumer incidents; evidence of differing compatibility practices.
- Related workflows: Technical Discovery, Solution Design, Implementation/Verification (future).

#### EF-G06 — Full-Cost, Revisable Decisions

**Understanding**
- Definition: Compare viable actions across financial, technical, human, opportunity and societal costs, using available evidence and judgment.
- Mechanism: Explicit alternatives reveal hidden costs; periodic review avoids locking in outdated assumptions.
- Illustration: Distributed builds saved developer time but created additional resource/incentive problems.

**Engineering Judgment**
- Apply: Important architectural boundaries, build/release tooling, dependency forking, staffing/process policy.
- Do not over-apply: A low-risk reversible change should not require a formal multi-hour evaluation.
- Trade-offs: Analysis effort versus risk of an unsupported decision.
- Alternatives: Short decision note; timeboxed spike; ADR for high-impact, long-lived decisions.
- Failure modes: Invented precision, sunk-cost bias, “because we always do this”.
  
**Evidence & Evolution**
- Primary: Chapter 1, “Trade-offs and Costs,” “Distributed Builds,” “Deciding Between Time and Scale,” “Revisiting Decisions.”
- Independent corroboration: **Pending**.
- Confidence: **High** as textual attribution; **Unassessed** for our local application.
- Review trigger: Changed constraints, cost trends, user outcomes, new research.
- Related workflows: Solution Design, Delivery Planning (currently Draft).

## 7. Mapping to My Engineer (hypotheses, not retroactive endorsements)

- **Feature Delivery Lifecycle v1.0:** proportional process and revisiting evidence (EF-G01/G06).
- **Requirement Discovery v1.0:** capture assumptions and product change needs (EF-G06).
- **Technical Discovery v1.0:** inspect undocumented behavior and consumers (EF-G03).
- **Solution Design v1.0:** reason about change cost and alternatives (EF-G02/G06).
- **Delivery Planning v0.1 (Draft):** make repeated work and feedback economical (EF-G04/G05).

**No current workflow status changes.** Local My Engineer policies (e.g., one core artifact, risk-based depth) are *our decisions*; the chapter does not prescribe them literally.

## 8. Critical review: what not to import blindly

1. **Scale mismatch:** Google examples assume many engineers, mature central tooling and organizational authority. Solo/team benefit may differ.
2. **Time mismatch:** Book appeared in 2020. Current build systems, AI coding workflows, supply-chain attacks and contract tooling warrant fresh checks.
3. **Metrics gaps:** No quantitative claim in this document establishes that any one My Engineer practice improves lead time or incidents.
4. **Hyrum's Law limits:** A warning about real-world dependencies, not a demand for perpetual backward compatibility.
5. **Shift-left limits:** Earlier checks complement, not replace, production observability, monitoring or incident response.
6. **Centralization limits:** A single shared process/tool can itself become a bottleneck; validate before standardizing.

## 9. Next action / approval boundary

Review whether these **six candidates** have the right scope and whether **EF-G03 + EF-G06** deserve deeper independent evidence gathering first. Only after explicit review and approval should they enter an Accepted Registry. Keep the English and Thai versions synchronized when revising.

**Source:** https://abseil.io/resources/swe-book/html/ch01.html — complete primary chapter, freely readable, with section anchors. This original summary is not a verbatim translation or reproduction of the book.
