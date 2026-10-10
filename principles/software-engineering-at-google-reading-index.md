# Software Engineering at Google — Selected Chapter Reading Index (v0.1)

- **Status:** Draft study library — candidate principles not approved
- **Reviewed:** 2026-10-10
- **Primary source:** https://abseil.io/resources/swe-book/html/toc.html
- **How to read:** Each chapter has canonical English notes and a Thai companion. The notes are original paraphrases based on chapter text, not full reproductions.

| Priority | Chapter | English | Thai | Candidates |
| --- | --- | --- | --- | --- |
| 1 | [10 — Documentation](https://abseil.io/resources/swe-book/html/ch10.html) | [EN](./software-engineering-at-google-ch10.md) | [TH](./software-engineering-at-google-ch10.th.md) | EF-G13–15 |
| 2 | [7 — Measuring Engineering Productivity](https://abseil.io/resources/swe-book/html/ch07.html) | [EN](./software-engineering-at-google-ch07.md) | [TH](./software-engineering-at-google-ch07.th.md) | EF-G16–18 |
| 3 | [11 — Testing Overview](https://abseil.io/resources/swe-book/html/ch11.html) | [EN](./software-engineering-at-google-ch11.md) | [TH](./software-engineering-at-google-ch11.th.md) | EF-G19–21 |
| 4 | [9 — Code Review](https://abseil.io/resources/swe-book/html/ch09.html) | [EN](./software-engineering-at-google-ch09.md) | [TH](./software-engineering-at-google-ch09.th.md) | EF-G22–24 |

Previous:
- [Chapter 1 — EN](./software-engineering-at-google-ch01.md) · [TH](./software-engineering-at-google-ch01.th.md) — EF-G01–06
- [Chapter 8 — EN](./software-engineering-at-google-ch08.md) · [TH](./software-engineering-at-google-ch08.th.md) — EF-G07–12

## Additional full-chapter study set — Chapters 14, 15, 16, 21, 24

| Suggested order | Chapter | English | Thai | Candidate IDs |
| --- | --- | --- | --- | --- |
| 1 | [14 — Larger Testing](https://abseil.io/resources/swe-book/html/ch14.html) | [EN](./software-engineering-at-google-ch14.md) | [TH](./software-engineering-at-google-ch14.th.md) | EF-G25–27 |
| 2 | [15 — Deprecation](https://abseil.io/resources/swe-book/html/ch15.html) | [EN](./software-engineering-at-google-ch15.md) | [TH](./software-engineering-at-google-ch15.th.md) | EF-G28–30 |
| 3 | [16 — Version Control and Branch Management](https://abseil.io/resources/swe-book/html/ch16.html) | [EN](./software-engineering-at-google-ch16.md) | [TH](./software-engineering-at-google-ch16.th.md) | EF-G31–33 |
| 4 | [21 — Dependency Management](https://abseil.io/resources/swe-book/html/ch21.html) | [EN](./software-engineering-at-google-ch21.md) | [TH](./software-engineering-at-google-ch21.th.md) | EF-G34–36 |
| 5 | [24 — Continuous Delivery](https://abseil.io/resources/swe-book/html/ch24.html) | [EN](./software-engineering-at-google-ch24.md) | [TH](./software-engineering-at-google-ch24.th.md) | EF-G37–39 |

### New candidate inventory

- **Larger Testing:** EF-G25 Test Fidelity by Risk; EF-G26 Smallest Sufficient SUT; EF-G27 Owned, Diagnosable Integration Tests.
- **Deprecation:** EF-G28 Retirement Is Engineering; EF-G29 Staffed Migration Deadlines; EF-G30 Prevent New Uses During Retirement.
- **Version Control:** EF-G31 Integration Source of Truth; EF-G32 Early Integration with Verification; EF-G33 Contain Version Divergence.
- **Dependency Management:** EF-G34 Dependency Graph Risk; EF-G35 Version Promises Need Tests; EF-G36 Update/Provider Ownership.
- **Continuous Delivery:** EF-G37 Safe On-demand Release Capability; EF-G38 Small Isolated Progressive Releases; EF-G39 User Impact and Health Guardrails.

**Current study count:** 11 chapters with English and Thai notes; 39 **unreviewed candidate** principles. These are candidates to deduplicate and validate, **not 39 accepted rules**.

## Candidate inventory from the four chapters

- **Documentation:** EF-G13 Owned and Changeable Documentation; EF-G14 Reader/Purpose; EF-G15 Deprecate Stale Sources
- **Measurement:** EF-G16 Actionable Measurement; EF-G17 Goals–Signals–Metrics; EF-G18 Balanced & Triangulated Productivity
- **Testing:** EF-G19 Tests Preserve Changeability; EF-G20 Reliable Actionable Feedback; EF-G21 Behavior over Coverage Targets
- **Code Review:** EF-G22 Correctness/Comprehension/Maintainability; EF-G23 Reviewable/Reversible Changes; EF-G24 Respectful Knowledge Exchange

## Research-to-registry boundary

These labels are **My Engineer synthesis**, not named standards established by Google. Each candidate needs independent corroboration, counterexamples, applicability and risk review before owner approval. Do not change accepted workflows, AGENTS.md, CI gates or the still-Draft Delivery Planning workflow automatically.

### Reading guidance
1. Read the Thai summary to establish vocabulary, then use the English primary notes and linked original chapter for disputed claims.
2. Identify what changes for solo versus team/multi-team.
3. Record whether the principle is actionable without adding bureaucracy.
4. Review cross-chapter overlap (e.g. Chapter 8 deterministic enforcement and Chapter 9/11 CI) before accepting duplicate entries.
