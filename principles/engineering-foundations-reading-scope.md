# Engineering Foundations — Reading Scope v0.1

- **Status:** Proposed / Draft (not an accepted Principles Registry)
- **Date:** 2026-10-10
- **Track:** A — My Engineer
- **First reference:** David Farley, *Modern Software Engineering: Doing What Works to Build Better Software Faster*, Addison-Wesley Professional, publisher date December 10, 2021 (copyright 2022), 1st edition
- **Primary bibliographic / contents sources:**
  - https://www.pearson.com/en-us/subject-catalog/p/Farley-Modern-Software-Engineering-Doing-What-Works-to-Build-Better-Software-Faster/P200000009466?view=educator
  - https://ptgmedia.pearsoncmg.com/images/9780137314911/samplepages/9780137314911_Sample.pdf

## Decision in scope

Start **Engineering Foundations** research with *Modern Software Engineering*, not by automatically adopting its advice. The purpose is to develop inspectable candidate principles and evaluate relevance to My Engineer's already accepted workflows. This document defines a research scope, **not** a claim that all book chapters have been studied or that principles have been approved.

## Why this reference

The publisher describes two core concerns: **learning/exploration** and **managing complexity**. This fits a universal engineering system better than starting with a single coding style or architectural template. We should still cross-check each claim with additional contemporary sources, competing positions and contextual evidence.

## Three knowledge layers for each candidate principle

1. **Understanding:** definition, problem addressed, mechanism, clear example.
2. **Engineering Judgment:** when to apply, when not to apply, trade-offs, alternatives, failure modes.
3. **Evidence & Evolution:** original source and *specific chapter/section*, corroborating/contradicting references, applicability and limits, confidence, last review, triggers for revalidation.

Never call a table-of-contents entry a verified full-book argument. Distinguish direct author claims, synthesis by My Engineer and local framework decisions.

## Research scope mapped to chapters

| Research group | Book anchors visible in publisher contents | Candidate questions (not yet accepted claims) |
| --- | --- | --- |
| **Engineering mindset** | Part I, chapters 1–3: Introduction; What Is Engineering?; Fundamentals of an Engineering Approach | What distinguishes engineering discipline from coding? What should be measured and why? How to reason about change and trade-offs? |
| **Optimize for Learning** | Part II, chapter 4: Working Iteratively; chapter 5: Feedback; chapter 6: Incrementalism; chapter 7: Empiricism; chapter 8: Being Experimental | What is the difference between iterative and incremental? What constitutes actionable feedback? How can we test a hypothesis without false certainty? |
| **Optimize for Managing Complexity** | Part III, chapter 9: Modularity; chapter 10: Cohesion; subsequent Part III chapters to verify before mapping | What makes boundaries meaningful? How do modularity and cohesion affect change, tests and coupling? What costs do abstractions introduce? |

The remaining chapter-to-principle mapping will be completed from publisher materials or legitimately accessed book text, **not** from memory alone.

## Candidate queue (provisional labels, not claimed as Farley's exact principle names)

- EF-C01 — Engineering as hypothesis-driven learning
- EF-C02 — Work iteratively
- EF-C03 — Shorten actionable feedback loops
- EF-C04 — Build and integrate incrementally
- EF-C05 — Prefer observation to untested assumptions
- EF-C06 — Design for modularity and useful cohesion

Each is **Unreviewed** until specific passages and independent evidence have been checked. Additional candidates may emerge from reading. Avoid forcing exactly six.

## Registry entry contract (future; not yet approved)

```yaml
id: EF-C01
title: Engineering as hypothesis-driven learning
domain: engineering-foundations
status: candidate
source_attribution: synthesized-label # author-claim | synthesized-label | local-decision
source_chapter: pending
source_evidence: pending
independent_evidence: pending
confidence: unassessed
last_reviewed: null
review_trigger: new counterevidence or implementation feedback
```

Narrative fields: Definition; Problem/Why; Mechanism; Examples; When to Apply; When Not to Apply; Trade-offs; Alternatives; Failure Modes; Sources and Contradictions; Workflow Mapping; Open Questions.

## Relationship to accepted workflows

- Requirement Discovery: observations and explicit uncertainty (candidate link; to verify).
- Technical Discovery: evidence-backed claims and inspection limits (candidate link; to verify).
- Solution Design: complexity, modularity and comparative trade-offs (candidate link; to verify).
- Delivery Planning v0.1 **Draft**: iteration, incremental delivery, feedback (candidate link; do not silently approve).

These are traceability **hypotheses**, not statements that Farley authored our workflows.

## Research / review protocol

1. Verify exact edition, chapter and section before attributing claims.
2. Note whether source is publisher summary, sample chapter, direct book reading or independent research.
3. Check a second meaningful source and any credible counterexample for consequential claims.
4. Explain context and failure cases; do not turn a heuristic into a universal rule.
5. Keep candidate → reviewed → accepted status explicit; only user approval advances principles to Accepted.
6. Revisit when runtime results, engineering practice or new research change applicability.
7. Keep quotations short, attribute them, and do not redistribute copyrighted chapters.

## Next smallest step

Research **Part I, chapters 1–3**, draft no more than 1–2 evidence-traced candidate principles and review their value with the owner before expanding. The rest is a reading backlog, not an endorsement.

**No new AI skill, automation, policy or previously approved workflow changes are authorized by this Draft.**
