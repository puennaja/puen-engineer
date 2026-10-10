# Software Engineering at Google — Chapter Reading Index (v0.3)

- **Status:** 🟢 Accepted knowledge baseline (owner approved, 2026-10-10). 39 synthesized principles accepted as **contextual engineering guidance**, not mandatory repo-wide rules. Independent corroboration and pilots remain pending.
- **Updated:** 2026-10-10
- **Original book table of contents:** https://abseil.io/resources/swe-book/html/toc.html
- **Coverage:** 11 selected chapters with English canonical notes and Thai companions; 39 candidate principles (EF-G01–39), **approved as contextual guidance, not mandatory rules**
- **Selection rule:** This index lists **all chapters currently summarized by My Engineer**, not all 25 chapters of the book.

## Project Approval Status

**Color legend:** 🟢 **Approved / Accepted** = expressly approved as a My Engineer baseline; 🟡 **Draft / Candidate** = written for review, **not approved**. A completed chapter summary is still Draft until the owner approves it; an extracted principle requires **separate** approval.

| Area | Document / scope | Status |
| --- | --- | --- |
| Workflow | [Feature Delivery Lifecycle v1.0](../workflows/feature-delivery-lifecycle.md) | 🟢 **Approved** |
| Workflow | [Requirement Discovery v1.0](../workflows/requirement-discovery.md) | 🟢 **Approved** |
| Workflow | [Technical Discovery v1.0](../workflows/technical-discovery.md) | 🟢 **Approved** |
| Workflow | [Solution Design v1.0](../workflows/solution-design.md) | 🟢 **Approved** |
| Workflow | [Delivery Planning v0.1](../workflows/delivery-planning.md) | 🟢 **Approved (study)** |
| Study | 11 bilingual Google SWE chapter summaries (see below) | 🟢 **Approved as knowledge references** |
| Registry | EF-G01–39 extracted principles | 🟢 **Accepted as contextual guidance** (not mandatory rules; evidence gaps remain) |

**Important:** The owner approved the chapter notes and the 39 synthesized principle candidates as **contextual engineering guidance**. This does **not** waive their listed uncertainties, imply independently proven effectiveness, or mandate CI/AGENTS.md policies. The four Approved workflows remain unchanged.

## Select a Chapter — All Summarized Chapters

| Chapter | Topic / original chapter | English summary | Thai summary | Candidate IDs | Status | Focus |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | [What Is Software Engineering?](https://abseil.io/resources/swe-book/html/ch01.html) | [English](./software-engineering-at-google-ch01.md) | [ภาษาไทย](./software-engineering-at-google-ch01.th.md) | EF-G01–06 | 🟢 **Approved (study)** | Engineering foundations |
| 7 | [Measuring Engineering Productivity](https://abseil.io/resources/swe-book/html/ch07.html) | [English](./software-engineering-at-google-ch07.md) | [ภาษาไทย](./software-engineering-at-google-ch07.th.md) | EF-G16–18 | 🟢 **Approved (study)** | Measurement |
| 8 | [Style Guides and Rules](https://abseil.io/resources/swe-book/html/ch08.html) | [English](./software-engineering-at-google-ch08.md) | [ภาษาไทย](./software-engineering-at-google-ch08.th.md) | EF-G07–12 | 🟢 **Approved (study)** | Rules and standards |
| 9 | [Code Review](https://abseil.io/resources/swe-book/html/ch09.html) | [English](./software-engineering-at-google-ch09.md) | [ภาษาไทย](./software-engineering-at-google-ch09.th.md) | EF-G22–24 | 🟢 **Approved (study)** | Review and collaboration |
| 10 | [Documentation](https://abseil.io/resources/swe-book/html/ch10.html) | [English](./software-engineering-at-google-ch10.md) | [ภาษาไทย](./software-engineering-at-google-ch10.th.md) | EF-G13–15 | 🟢 **Approved (study)** | Engineering documentation |
| 11 | [Testing Overview](https://abseil.io/resources/swe-book/html/ch11.html) | [English](./software-engineering-at-google-ch11.md) | [ภาษาไทย](./software-engineering-at-google-ch11.th.md) | EF-G19–21 | 🟢 **Approved (study)** | Testing foundations |
| 14 | [Larger Testing](https://abseil.io/resources/swe-book/html/ch14.html) | [English](./software-engineering-at-google-ch14.md) | [ภาษาไทย](./software-engineering-at-google-ch14.th.md) | EF-G25–27 | 🟢 **Approved (study)** | Integration and larger-scale verification |
| 15 | [Deprecation](https://abseil.io/resources/swe-book/html/ch15.html) | [English](./software-engineering-at-google-ch15.md) | [ภาษาไทย](./software-engineering-at-google-ch15.th.md) | EF-G28–30 | 🟢 **Approved (study)** | System retirement and migration |
| 16 | [Version Control and Branch Management](https://abseil.io/resources/swe-book/html/ch16.html) | [English](./software-engineering-at-google-ch16.md) | [ภาษาไทย](./software-engineering-at-google-ch16.th.md) | EF-G31–33 | 🟢 **Approved (study)** | Integration and branching |
| 21 | [Dependency Management](https://abseil.io/resources/swe-book/html/ch21.html) | [English](./software-engineering-at-google-ch21.md) | [ภาษาไทย](./software-engineering-at-google-ch21.th.md) | EF-G34–36 | 🟢 **Approved (study)** | Dependency and compatibility |
| 24 | [Continuous Delivery](https://abseil.io/resources/swe-book/html/ch24.html) | [English](./software-engineering-at-google-ch24.md) | [ภาษาไทย](./software-engineering-at-google-ch24.th.md) | EF-G37–39 | 🟢 **Approved (study)** | Safe release and delivery |

All links in the English/Thai columns point to notes in this repository. Chapter title links point to the freely available original source.

## Recommended reading path (optional)

For building My Engineer's principles, a useful sequence is **1 → 8 → 10 → 7 → 11 → 9 → 14 → 15 → 16 → 21 → 24**. The table above intentionally remains in **chapter-number order** so every completed chapter is easy to find. Read only areas relevant to the current engineering decision.

## Consolidation notes — candidates, not rules

- **EF-G01–06 (Chapter 1):** expected software lifetime, changeability, observable compatibility, scalable recurring work, automated expectations, full-cost trade-offs.
- **EF-G07–12 (Chapter 8):** rules worth their cost, reader focus, contextual consistency, exceptions, rule revision, objective enforcement.
- **EF-G13–15 (Chapter 10):** documentation ownership, audience and purpose, eliminating stale competing sources.
- **EF-G16–18 (Chapter 7):** actionable measures, Goals–Signals–Metrics, balanced qualitative/quantitative evidence.
- **EF-G19–21 (Chapter 11):** tests support change, reliable feedback, meaningful behavior over coverage targets.
- **EF-G22–24 (Chapter 9):** review comprehensibility, reviewable changes, respectful knowledge sharing.
- **EF-G25–27 (Chapter 14):** test fidelity proportional to risk, smallest sufficient system under test, owned integration tests.
- **EF-G28–30 (Chapter 15):** retirement work, resourced migration, no new deprecated dependencies.
- **EF-G31–33 (Chapter 16):** integration source of truth, early verified integration, version-skew management.
- **EF-G34–36 (Chapter 21):** dependency graphs, version compatibility evidence, dependency ownership.
- **EF-G37–39 (Chapter 24):** deploy-on-demand capability, progressive exposure, release health safeguards.

## Research-to-registry boundary

These IDs and labels are **My Engineer syntheses**, not formally named Google standards. Check current independent evidence, counterexamples, context and overlap before accepting a candidate. Do **not** change accepted workflows, AGENTS.md, CI gates or the still-Draft Delivery Planning workflow merely because a book recommends an approach.

**Reading guidance:** Start with the Thai note to learn the concepts, refer to the English note and original chapter for attribution, then assess applicability to solo/team/multi-repo contexts. The reading notes are original summaries, not full reproductions of the book.
