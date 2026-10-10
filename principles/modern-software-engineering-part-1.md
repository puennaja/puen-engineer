# Modern Software Engineering — Engineering Foundations, Part I (Study Notes v0.1)

- **Status:** Draft / Research notes; no Principle is accepted by this document
- **Date reviewed:** 2026-10-10
- **Language:** English (canonical); [Thai companion](./modern-software-engineering-part-1.th.md)
- **Work:** David Farley, *Modern Software Engineering: Doing What Works to Build Better Software Faster*, 1st ed., Addison-Wesley (published 2021; © 2022)
- **Scope:** Part I, Chapters 1–3; this is an original synthesis, **not** a replacement for the book
- **Reading scope:** [Engineering Foundations Reading Scope](./engineering-foundations-reading-scope.md)

## 0. Source integrity — read this first

**Directly available and reviewed:** publisher-provided **Chapter 3** extract, pp. 31–38, and the publisher's full table of contents and overview.

**Not fully available/read in this pass:** complete prose of **Chapters 1 and 2**. Their sections are identified from the authoritative table of contents. Accordingly, their summaries below are **thematic previews**, not detailed claims about passages we have not inspected.

**Attribution codes**
- **[AUTHOR / TEXT]** Ideas supported by directly inspected Chapter 3 text.
- **[PUBLISHER / OUTLINE]** Topic coverage supported by Pearson overview or table of contents, without complete chapter-text verification.
- **[MY ENGINEER / SYNTHESIS]** Our interpretation or practical application; not the author's verbatim framework.
- **[OPEN]** Requires further primary-text reading or independent evidence.

References: [S1] Pearson product overview and contents; [S2] Pearson sample PDF including Chapter 3; [S3] Farley's published Chapter 3 extract on InformIT. Direct URLs at the end.

## 1. The central argument

**[AUTHOR / TEXT; PUBLISHER / OUTLINE]** Treating software development as engineering requires a disciplined way to make progress under uncertainty. Farley describes two fundamental competencies:

1. **Become good at learning:** exploration, discovery, experiments and empirical reasoning help determine what works in changing circumstances.
2. **Become good at managing complexity:** software can exceed an individual's capacity to understand, and teams introduce organizational as well as technical complexity.

**[MY ENGINEER / SYNTHESIS]** A useful test of an engineering process is not how many ceremonies or diagrams it produces, but whether it improves the quality and speed of learning while making changes safer and more comprehensible.

This is a summary of themes, not a claim that one measurement proves a system is engineered well.

## 2. Chapter-by-chapter reading notes

### Chapter 1 — Introduction (thematic preview)

**Evidence level: [PUBLISHER / OUTLINE].** Publisher-listed themes include the practical application of science, the meaning/history of software engineering and a shift in how the field thinks about making progress.

**[MY ENGINEER / SYNTHESIS]** Working question: When we call an activity “engineering,” what observable evidence shows that we are solving a problem rather than merely producing code?

**[OPEN]** Read the complete chapter before attributing specific definitions, historical arguments or examples to Farley.

### Chapter 2 — What Is Engineering? (thematic preview)

**Evidence level: [PUBLISHER / OUTLINE].** Listed topics include design versus production engineering; engineering versus coding; limitations of craft; precision, measurement, repeatability, scale, complexity, trade-offs and false impressions of progress.

**[MY ENGINEER / SYNTHESIS]** Working question: What work reduces the cost/risk of change beyond writing the next line of code? For example, understanding interfaces, checking an assumption and proving a recovery path can be engineering work even when they produce little code.

**[OPEN]** We have *not* independently verified Farley's detailed positions on craft, productivity or particular trade-offs from Chapter 2 text. Do not present the preview as a full chapter summary.

### Chapter 3 — Fundamentals of an Engineering Approach

**Evidence level: [AUTHOR / TEXT]; publisher Chapter 3 sample, book pp. 31–38.**

**A. Durable knowledge vs. rapidly changing techniques.** Farley questions treating every fashionable technology/practice as lasting progress. He argues for fundamentals that remain useful as tools evolve. **Implication [MY ENGINEER]:** evaluate a proposed practice by the problem it solves and the feedback it produces, not its popularity.

**B. Meaningful measurement supports reasoning.** Farley distinguishes the difficulty of measuring individual software productivity from the possibility of measuring important delivery-system properties. He discusses **stability** and **throughput** as two useful perspectives, with the point that they need to be considered together. **Caution [MY ENGINEER]:** neither metric alone establishes product value, individual merit or causal improvement.

**C. Learn empirically.** Farley advocates an exploratory, scientific style of engineering: observe a problem, test explanations and learn from results, instead of equating confident opinion with fact. **Implication [MY ENGINEER]:** separate verified behavior, hypothesis, test and decision in a discovery record.

**D. Manage complexity intentionally.** Code organization and team coordination matter because large systems cannot be understood in one person's head. **Implication [MY ENGINEER]:** make boundaries, ownership and feedback testable and understandable; do not assume that adding layers, interfaces or services automatically reduces complexity.

**E. Two capabilities, not one universal recipe.** The chapter frames expertise at learning and at managing complexity as foundations, rather than endorsing any single project-management methodology or architectural pattern.

## 3. Candidate Principles Registry — deliberately unapproved

### EF-C01 — Learn from evidence, not confidence

- **Type:** Candidate engineering principle, My Engineer naming and synthesis; based chiefly on Chapter 3's empirical learning theme.
- **Definition:** Where important uncertainties exist, use inspectable observations or bounded experiments to inform decisions rather than presenting assumptions as established facts.
- **Why/how:** A testable hypothesis creates a way to discover whether the current model is wrong.
- **Apply when:** Uncertain requirements, unexpected production behavior, unfamiliar codepaths, costly/reversible architectural decisions.
- **Do not over-apply:** An obvious low-risk code edit should not require an elaborate experiment.
- **Trade-offs:** Instrumentation and experiments consume time; poorly chosen metrics and biased test data can mislead.
- **Failure modes:** Cherry-picked evidence, fabricated benchmarks, overgeneralizing from one spike, post-hoc justifications.
- **Alternative:** For well-understood, low-risk work, use proven local conventions with quick verification.
- **Example [fictional]:** Before building CSV statement import, test synthetic variants for duplicate IDs and date formatting; record unsupported formats.
- **References:** [S2] Chapter 3 pp. 31–38; [S3] Chapter 3.
- **Confidence:** Moderate for the thematic link; applicability beyond that is our engineering judgment. Independent empirical corroboration pending.
- **Mapping hypothesis:** Requirement Discovery, Technical Discovery, Solution Design.
- **Status:** Candidate — not accepted.

### EF-C02 — Manage complexity to preserve changeability

- **Type:** Candidate engineering principle, My Engineer naming and synthesis; Chapter 3 complexity theme.
- **Definition:** Choose boundaries and responsibilities that make important system behavior understandable, testable and safe to change.
- **Why/how:** Reducing what must be simultaneously understood and coordinated lowers the cost of reasoning about changes.
- **Apply when:** Growing codebases, multi-team systems, unclear data ownership, many coupled changes.
- **Do not over-apply:** A small disposable proof-of-concept may need less structure; premature layers can increase cognitive burden.
- **Trade-offs:** Abstractions and boundaries add indirection, maintenance and coordination.
- **Failure modes:** Interface for every class, splitting services without evidence, duplicating code to force artificial separation.
- **Alternative:** Maintain a simple cohesive module with direct dependencies until a change boundary is demonstrated.
- **Example [fictional]:** Isolate bank-statement format decoding from the domain calculation of spending, but avoid a distributed import service without operational reasons.
- **References:** [S2] Chapter 3 pp. 36–38; Part III table of contents gives related topics, but detailed Part III claims are **not** verified here.
- **Confidence:** Moderate for the high-level theme; concrete modularity tactics require later chapter review and independent sources.
- **Mapping hypothesis:** Technical Discovery, Solution Design.
- **Status:** Candidate — not accepted.

## 4. Questions and objections to carry forward

1. **Measurement is contextual.** Stability/throughput may show delivery health; they do not alone prove that the team built the right thing. How will My Engineer assess user outcomes?
2. **Experiments can be misleading.** Who defines success, data quality and the decision threshold? How do we check counterexamples?
3. **Complexity is not eliminated by naming it.** Which boundary actually reduces change cost, and when does isolation introduce more coordination?
4. **Scientific reasoning is not a rigid ceremony.** What is the minimum evidence for low-risk changes?
5. **Book-age check:** This is a 2021 work, not a current benchmark report. Confirm contemporary applicability with independent research and our own pilot observations before treating recommendations as policy.

## 5. How this influences My Engineer (mapping proposals, not endorsements)

| Existing workflow | Candidate connection | Required verification |
| --- | --- | --- |
| Requirement Discovery v1.0 | Separate questions and evidence before selecting a feature (EF-C01) | Does this expose genuine misunderstandings? |
| Technical Discovery v1.0 | Back consequential as-is claims with inspected sources (EF-C01) | Are the citations reliable and proportionate? |
| Solution Design v1.0 | Compare viable options and complexity costs (EF-C01, EF-C02) | Does this avoid unnecessary abstractions? |
| Delivery Planning v0.1 — Draft | Learn from thin increments rather than pretending complete predictability | Further study of Part II, Chapters 4–8 required |

**No status of any workflow is changed** by these research notes.

## 6. Next reading / verification

- Read the full primary texts of Chapters 1–2 when legitimately available and update sections 2.1–2.2.
- Review Part II, Chapters 4–8 for precise treatment of iteration, feedback and incrementalism; do not attribute those specifics to Part I.
- Compare empirical-learning claims with current engineering/measurement literature and record counterarguments, not just supporting quotes.
- Review EF-C01 and EF-C02 with the owner; only explicitly approved, source-traceable entries can become **Accepted** Principles.

## Sources and access boundaries

- **[S1]** Pearson, official book overview and complete table of contents: https://www.pearson.com/en-us/subject-catalog/p/modern-software-engineering-doing-what-works-to-build-better-software-faster/P200000009466/9780137314867
- **[S2]** Pearson, official sample PDF (includes Chapter 3, pp. 31–38): https://ptgmedia.pearsoncmg.com/images/9780137314911/samplepages/9780137314911_Sample.pdf
- **[S3]** InformIT, David Farley, Chapter 3 excerpt, section “The Foundations of a Software Engineering Discipline”: https://www.informit.com/articles/article.aspx?p=3129276&seqNum=4

This is an original, non-exhaustive study summary. Do not circulate reproduced chapters or represent thematic previews as a full-book reading.
