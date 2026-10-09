# Practice 01 — Requirement Discovery

- **Track:** B — Optional mentorship
- **Status:** Ready to practice (curriculum draft, not a standard)
- **Maps to Track A:** [FDL v1.0: Intake / Discovery / Requirements](../workflows/feature-delivery-lifecycle.md)
- **Scenario domain:** Fictional personal finance application
- **Prerequisites:** None
- **Skill dependency:** None

## Objective

Learn to turn an ambiguous request into a testable, valuable problem definition **without immediately prescribing a feature**.

By the end, the learner should be able to:
1. Separate **problem, desired outcome and proposed solution**.
2. Ask neutral questions about actual recent behavior and workarounds.
3. Capture **observations, assumptions, constraints, and open questions** separately.
4. Identify critical product/data uncertainties and explain their risk.
5. Decide whether evidence is sufficient for exploration versus implementation.

## Case: personal finance

**Initial fictional request:** "I want to record my income and expenses and see my monthly remaining money."

**Interview developments from the example scenario** (fictional; *not* validated product requirements):
- User often notices low balance near the end of the month.
- User rarely records individual expenses; viewing transaction history feels time-consuming.
- User wants to understand major spending areas and possible spending reductions.
- User uses two bank accounts, credit card, and cash.
- User is hesitant about direct bank account integration over privacy concerns.
- User is willing to try importing a statement about once a month.
- Cash is used frequently, but daily detailed entry feels burdensome; rough periodic totals might be acceptable.
- "Remaining money" may mean what is available to spend **after upcoming essential obligations**, not a raw bank balance.

**Do not assume** statement formats, automatic categorization accuracy, a validated MVP, or user acceptance of any particular solution.

## Practice modes

### Guided discovery (default when asked to mentor)
The mentor acts as stakeholder. The learner asks a question; the mentor responds with specific, realistic evidence. After each exchange, provide compact feedback:
- What was learned?
- Was the question neutral or leading?
- What uncertainty remains?
- What is a stronger follow-up and why?

Avoid evaluating the learner personally. Focus on the reasoning.

### Synthesis practice
Have the learner draft:
- Problem statement and target user
- Desired outcome / possible success signal
- Evidence vs assumption vs unknown
- Constraints and critical risks
- Candidate scope slice and deferred scope
- Decision: ready for exploration or ready for implementation (with rationale)

### Decision challenge
Ask which uncertainty to investigate first, and why. Review the reasoning as **assumption → risk → impact**, while allowing parallel investigation of independent questions.

## Review rubric

| Criterion | Strong evidence | Common failure |
| --- | --- | --- |
| Problem framing | Describes user pain and outcome independently of a UI/feature | Repeats proposed solution as problem |
| Elicitation | Uses open questions about recent actual behavior | Yes/no questions that inject assumptions |
| Evidence discipline | Separates observed statements from interpretations | Treats hypothetical needs as confirmed |
| Feasibility | Identifies data acquisition, coverage, trust and effort issues | Designs calculations without viable data |
| Scope | Defines a small testable learning step | Assumes every interesting feature is MVP |
| Readiness | States what is known enough, what remains risky, and next validation | Demands perfect knowledge or codes blindly |

## Example mentorship prompt

"วิท เข้า Track B — Requirement Discovery ใช้ Personal Finance case เดิม ขอให้มึงเป็น stakeholder แล้วช่วย review คำถามกูทีละข้อ โดยไม่เฉลยก่อน"

## Suggested next exercise

Synthesize the available interview notes into **Problem / Constraints / Open Questions**, then select one uncertainty to investigate next. A good summary is brief and does not invent requirements.

## Connection back to Track A

Output is a *practice artifact*, not automatically an accepted product requirement. The corresponding engineering workflow should prescribe ways to gather and validate genuine stakeholder evidence, not hard-code the case's fictional answers.
