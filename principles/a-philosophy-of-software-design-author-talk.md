# A Philosophy of Software Design — Author Talk & Stanford Lectures (v0.1)

- **Status:** 🟡 Draft / Candidate — no newly accepted principles
- **Source type:** Author-talk + author-lecture analytical study, **not a full-book or full-chapter summary**
- **Thai companion:** [Thai](./a-philosophy-of-software-design-author-talk.th.md)
- **Reviewed:** 2026-10-10
- **Actual coverage:** Official talk metadata, accessible transcript excerpts with timestamps, 2018/2021 Stanford lecture notes, and a later author interview.
- **Unverified:** The large official second-edition PDF could not be retrieved; the complete video was not played directly. Do **not** present these notes as a full talk or second-edition reading.

## Source anchors

- [A1 — Official 2018 Talks at Google video](https://www.youtube.com/watch?v=bmSAYlu0NcY)
- [A2 — Third-party timestamped transcript](https://lilys.ai/cn/notes/808736) — verify any verbatim quotation against the video
- [A3 — CS190 Winter 2018 Modular Design](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter18/lecture.php?topic=modularDesign)
- [A4 — CS190 Winter 2021 Introduction](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter21/lecture.php?topic=intro)
- [A5 — Author's second-edition change note](https://web.stanford.edu/~ouster/cgi-bin/book.php)
- [A6 — Later author interview transcript](https://podscripts.co/podcasts/the-pragmatic-engineer/the-philosophy-of-software-design-with-john-ousterhout) — supporting caution on error handling

## Evidence-based synthesis

### 1. Complexity grows from dependencies, obscurity and accumulated small compromises

**Author argument (paraphrased):** In CS190, Ousterhout describes complexity as the structural difficulty of understanding and changing code. Dependencies force readers to understand distant modules; obscurity makes required information hard to discover. Complexity increases incrementally, so a large refactor may become prohibitively expensive.

**Evidence:** CS190 Winter 2021, Introduction: Complexity; 2018 talk, strategic/tactical discussion around 34–38 min

**Trade-off / limits:** Not all dependencies are harmful; necessary contracts, shared invariants and cross-service coordination remain.

### 2. A deep module hides substantial implementation behind a comparatively simple interface

**Author argument (paraphrased):** Ousterhout measures abstraction leverage by the benefit of functionality relative to interface complexity, including usage rules and side effects, not just the function signature. UNIX file operations serve as an example: a small common interface masks large implementation details. Thin pass-through layers may add rather than remove complexity.

**Evidence:** CS190 Winter 2018, Modular Design: Classes Should Be Deep, Information Hiding, New Layer New Abstraction; 2018 talk around 12:34–16:15

**Trade-off / limits:** Module depth is not line count; do not turn example class sizes into a hard rule. Abstractions sometimes deliberately have low functionality for safety or composition.

### 3. Reduce unnecessary error cases through API design, but never ignore real failures

**Author argument (paraphrased):** The talk introduces 'define errors out of existence': change a contract so a previously exceptional but harmless state becomes an ordinary valid outcome. This is different from catching-and-discarding failures. In a later author interview, Ousterhout explicitly warns against students failing to handle network crashes while claiming to have designed the error away.

**Evidence:** 2018 Talks at Google: discussion of exceptions and error complexity; Pragmatic Engineer interview 2025 28:03–30:36 (corroboration of author's warning)

**Trade-off / limits:** Security failures, financial integrity violations and transient distributed failures cannot be 'defined away' merely by suppressing signals.

### 4. Strategic design invests in future changeability rather than merely shipping the next feature

**Author argument (paraphrased):** The talk contrasts tactical patches with sustainable design investment. Strategic programming can feel slower initially, but the objective is to reduce the cumulative cost of changes. CS190 advocates an iterative loop: design some, implement, evaluate, review, revise. The talk's illustrative percentage claims should not be treated as measured universal ROI.

**Evidence:** Talk approx. 34–44 min; CS190 Winter 2021 Introduction: Role of Design in the Software Development Process

**Trade-off / limits:** Short-lived throwaway code and crisis patches may legitimately favor immediate delivery; strategic design must be proportional to lifetime and risk.

## Candidate Principles (not approved)

| ID | Candidate | Overlap check |
| --- | --- | --- |
| AP-C01 | Reduce Apparent Complexity in Changed Areas | Related to EF-G02; specifically dependency and obscurity |
| AP-C02 | Favor Deep Modules and Information Hiding | Adds module-level boundary evaluation, not line-count mandates |
| AP-C03 | Reduce Avoidable Error Cases Through Explicit Semantics | API/error-design extension; never hide genuine failures |
| AP-C04 | Invest Proportionally in Strategic Design | Related to EF-G01 and EF-G06; consider consolidation |

### Three-layer assessment

**AP-C02 — Deep Modules:** Understanding: high-value capability behind a simple, narrow knowledge contract. Judgment: look for pass-through layers and information leakage; avoid replacing them with god modules and uncontrolled responsibilities. Evidence: A2/A3 directly support attribution; independent effectiveness tests and local pilots remain pending.

**AP-C03 — Error Contract Design:** Understanding: carefully chosen semantics can remove the need for exceptional handling of harmless states without swallowing real errors. Judgment: useful with some idempotent operations and input normalization; dangerous when hiding contractual breaches, financial invariants or transient network failures. Evidence: A2 and A6; validate actual failure modes before approval.

## Illustrative My Engineer example (not from the author)

A CSV ingestion interface like `ParseAndValidateStatement(file)` could conceal encoding, date parsing, schema mapping and invariant validation. Five pass-through wrappers that simply forward the same parameters may increase apparent complexity. Conversely, merging all ingestion concerns into an oversized god service would violate sensible responsibility boundaries. Assess actual change patterns instead of imposing a fixed number of layers.

## Open research tasks

The official second-edition extract was **not read** due retrieval failure. Obtain a readable copy or user-provided licensed extract before creating second-edition book notes. A verified complete video/transcript pass is still required before calling this a full-talk review. Compare Ousterhout's advice with independent design evidence (e.g. Parnas on information hiding).

**No approved workflows, AGENTS.md mandates or EF-G01–39 statuses changed.**
