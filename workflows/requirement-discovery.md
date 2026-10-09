# Requirement Discovery Workflow — v1.0

- **Status:** Accepted — My Engineer Track A workflow
- **Approved:** 2026-10-10 by repository owner
- **Version:** 1.0
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Foundation:** [Accepted FDL v1.0](./feature-delivery-lifecycle.md)
- **Practice companion:** [Track B: Requirement Discovery](../practice/01-requirement-discovery.md) (optional; never a prerequisite)
- **Scale:** Solo engineer → delivery team → product organization
- **Example:** Fictional personal finance application only; no internal company data

## Approval scope

The five activities, single-summary artifact, human validation, and **risk/uncertainty-proportional discovery** are approved as the baseline. Discovery depth is determined by the material uncertainty and consequences of being wrong, not a universal checklist or ceremony. This workflow permits small changes to use a short issue and ambiguous/high-risk work to require deeper evidence. Acceptance does **not** mean every example hypothesis is validated, nor that future skills, templates, or automation are approved.

## Purpose and problem solved

**Problem:** An idea, stakeholder statement, or incomplete story is often treated as a ready-made feature. Teams build on hidden assumptions, miss user behavior and constraints, and incur avoidable rework.

**Outcome:** Produce *enough validated shared understanding* to decide whether to investigate, prototype, proceed to technical exploration, defer, or decline. Do **not** pretend that all requirements are known or implementation-ready.

**Not in scope:** Architecture selection, implementation task breakdown, full technical specifications, or inventing user desires. Those can follow in later FDL activities.

## Working principles

1. Separate **problem → desired outcome → candidate solution**; a requested feature is not automatically the best solution.
2. Ask about actual behavior and observable evidence before asking users to confirm a proposed feature.
3. Label **Confirmed**, **Assumed**, **Unknown**, and **Decision**; confirmation requires a source or designated stakeholder, not an AI guess.
4. Keep discovery iterative: new technical findings may require new user questions.
5. Use the smallest artifact that supports the next decision. A minor change may require only a concise issue.
6. Use AI to suggest questions, summarize permitted evidence and identify inconsistencies; no automated approval or fictitious validation.
7. The workflow is **risk proportional** and **method agnostic**; it is not a Scrum ceremony or a phase gate requiring a meeting.

## Entry points

- One-line idea or verbal request.
- Jira/issue story with partial or contradictory context.
- Customer/user feedback or reported operational problem.
- Proposed enhancement to an existing product.
- A greenfield product opportunity (which can expand into broader product discovery).

Minimum intake: original request in the requester's words; known requester/contact (or mark unknown); urgency/context, if available.

## Five activities (iterate, combine, or revisit)

| Activity | Questions and techniques | Output (in the single summary) | Why it exists |
| --- | --- | --- | --- |
| **1. Frame the problem** | Who experiences what difficulty? What happened? Why now? What if nothing changes? Distinguish observed pain from requested implementation. | Problem statement, actors, initial evidence | Avoid solution-first scope creep |
| **2. Discover user context** | Ask for recent concrete examples, current workarounds, frequency, friction, involved stakeholders, and actual constraints. Avoid leading questions. | Current behavior, evidence, user journey fragments | Reveal real needs and adoption risks |
| **3. Analyze needs and uncertainty** | Identify capabilities, major alternate scenarios, access/data/privacy concerns, business rules, feasibility unknowns, constraints. Classify facts vs hypotheses. | Needs, boundaries, critical assumptions, risks, open questions | Prevent silent assumptions and premature design |
| **4. Define outcome and candidate scope** | What user/business change indicates success? What is the smallest testable slice? What is deliberately excluded? Are solutions options rather than promises? | Outcome signals, candidate in/out scope, dependencies | Bound investment and make learning testable |
| **5. Validate and route** | Playback understanding to decision owner/users as available. Record agreement and unresolved items. Decide next investigation. | Named decider, validation evidence, next action and owner | Ensure shared understanding and actionable next step |

### Suggested elicitation moves

- Ask "**Walk me through the last time this happened**" rather than "Would you like an automated dashboard?"
- Probe **frequency/severity**, **exceptions**, **workaround**, and **cost of not solving**.
- Test a hypothesis with evidence or a small prototype; do not declare it a requirement merely because it sounds useful.
- Capture disagreement explicitly; do not smooth over contradictions.

## One required artifact: Discovery Summary

**Consumer:** Requester/Product Lead uses it to prioritize/validate; Engineer/Tech Lead uses it to identify next technical questions; QA/design may use it to shape scenarios.

A single Jira issue, markdown document, or product board entry may hold this artifact; the name/location is not mandatory.

```markdown
# Discovery: <Working title>
Status: Draft | Validated for exploration | More discovery required | Deferred | Declined
Owner / decider:
Date / evidence sources:

## Problem & users
Who has what problem, in what context? Why does it matter?

## Current behavior & evidence
Observed examples, workarounds and frequency.
Evidence: Confirmed / Assumed / Unknown (cite source or named person).

## Desired outcome
Observable user/business change and candidate success signal.
Do not invent numeric targets.

## Needs & constraints
High-level capabilities, known privacy/data/security/operational constraints.

## Candidate scope
In / Out / To be tested (not implementation promises).

## Open questions & risks
Question | importance / impact | owner | next evidence step

## Decision & next step
Validated by whom, when? What is agreed, what is still uncertain?
Next action: explore technically | prototype/experiment | discover more | defer/decline
```

**Avoid** duplicating this information across multiple standalone artifacts without an identifiable consumer.

## Outcomes: routing, not a binary ready/not-ready gate

| Outcome | When to choose | Example next action |
| --- | --- | --- |
| **Explore technically** | Problem and desired outcome are understood enough to investigate feasibility, integration, data sources, and risks. | Technical discovery with evidence-linked questions |
| **Prototype / experiment** | Major uncertainty concerns value, usability, or behavior; rapid learning is cheaper than committing to a design. | User test using fake data or a limited prototype |
| **Discover more** | Missing or conflicting information could change scope, meaning, or feasibility. | Targeted stakeholder interview |
| **Defer / decline** | No credible problem, low relative value, unacceptable cost/risk, or higher-priority commitments. | Record rationale and owner |

**Do not conflate:** validated **for exploration** with ready **for implementation**. Implementation readiness is assessed later with acceptance criteria, technical impact, design/test evidence and ownership according to FDL.

### Minimal decision check

Before routing work, the responsible humans should be able to answer:
- What actual problem and users are we targeting?
- What evidence supports it? What is only a hypothesis?
- What outcome would make further work worthwhile?
- Which unknown is most likely to change the solution?
- Who can decide the next action and who owns it?

A low-risk, self-contained task can merge this into a short issue. High-risk finance, personal data, authorization, or regulated cases demand more explicit validation and safeguards.

## Roles at different scales

| Scale | Accountability | Coordination overhead |
| --- | --- | --- |
| **Solo** | One person names requester (possibly self), states assumptions and makes decision | Single short note, test with users when possible |
| **Team** | Product owner validates problem and priority; Tech Lead/Engineers identify feasibility unknowns; Design/QA contribute | Shared summary, brief playback, named next-step owner |
| **Product organization** | Product/Research leads discovery and validation; Engineering and other specialists assess risk; product governance decides investment | Segment evidence, research and opportunity links, compliance input where applicable |

A Tech Lead helps ensure testability and technical feasibility but is **not** automatically the sole owner of user research, product priority or approval.

## Fictional worked example — personal finance

**Raw request:** "I want to record income and expenses and see how much I have left."

**Evidence stated by the fictional user**
- Wants to identify high spending and potential reductions.
- Rarely records expenses; reviewing bank transactions manually is tiresome.
- Uses two bank accounts, a credit card and cash.
- Hesitates to connect banks directly for privacy reasons.
- Might import statements monthly and estimate cash spending periodically.
- "Left" could mean spendable money after expected obligations, rather than a bank balance.

**Not yet established**
- What financial decisions the user will actually make using the app.
- Which statement formats are accessible and importable, with acceptable security and consent.
- Whether monthly statements are timely enough for a pre-payday "available to spend" view.
- How incomplete cash data and duplicate transfers would affect trust and accuracy.
- Whether a prototype improves user understanding and continued use.

**Working problem (hypothesis, not validated):** The user lacks a low-effort, trustworthy view of spending patterns and near-term spending capacity.

**Possible next action:** Validate data availability and actual import behavior with synthetic samples while separately testing the meaning of available-to-spend on a mockup. Do not promise automatic categorization, bank sync, or exact cash balances yet.

## AI opportunities — no skill required in v0.1

Potential **assistive** tasks: draft open interview questions; highlight unstated assumptions; sort interview notes into evidence/hypotheses/unknowns; check summary for contradictions; propose small validation experiments.

Human-only accountability: confirm user statements, accept problem framing, decide priority/scope, assess privacy approvals, authorize publication. Treat AI-generated text as *draft*.

Build a new `puen-stack` Skill only after repeated pilots show a meaningful reduction in manual effort or missed questions, with tests and review evidence. Simple templates may be enough.

## How to evaluate this workflow

Run on one **synthetic or approved non-sensitive** request and record:
- Time/effort to obtain a reviewable summary.
- Number and impact of critical assumptions found before technical planning.
- Whether decision owners understand and agree with the problem.
- Whether downstream design requires material rework because a user need was misunderstood.
- Any unnecessary steps or documents that should be removed.

No automatic productivity claim from a single exercise.

## Refinements and pilot questions (post-approval)

1. Is the one-summary artifact lean enough for solo use yet useful for teams?
2. Should an explicit business-value/prioritization question always appear in the check?
3. What is the smallest credible evidence needed for **Validated for exploration**?
4. Are privacy/security prompts adequate for the personal-finance sample?
5. Which two pilot cases (tiny change and uncertain product request) best demonstrate appropriate scaling?

**Status:** Accepted baseline. These refinement questions remain open and may be improved through pilots without blocking use. The fictional case is not a validated requirement, and FDL v1.0 remains the parent foundation.
