# A Philosophy of Software Design, 2nd ed. — Chapter 21: Decide What Matters (Extract Study)

- **Status:** 🟡 Draft v0.1; not a full-chapter review or approved principle
- **Author:** John Ousterhout (second edition, 2021)
- **Source:** [Official author-hosted extract PDF](https://web.stanford.edu/~ouster/cgi-bin/aposd2ndEdExtract.pdf)
- **Secondary metadata:** [Author's book page](https://web.stanford.edu/~ouster/cgi-bin/book.php)
- **Thai companion:** [Thai](./a-philosophy-of-software-design-ch21-extract.th.md)
- **Evidence boundary:** Original PDF search-index excerpts for Chapter 21 opening and §§21.3–21.4 were inspected. The 13.9 MB PDF was too large for the web reader; §21.1 (beyond an indexed excerpt), §21.2 and §21.5 have **not** been fully inspected. This is therefore a **partial extract study**, not a full chapter summary.

## What the author actually argues (verified portions only)

### The opening: design is a hierarchy of importance

Good design chooses which pieces of information, constraints and capabilities matter to users or maintainers, and emphasizes those. Less-important details should be localized and hidden. Abstraction, naming and even performance architecture all involve selecting what users must understand.

### §21.1 — Recognize leverage (indexed excerpt only)

High-leverage concepts solve multiple related problems or let one piece of knowledge explain many others. In the text-editor example, a range insertion/deletion API is more general and reusable than distinct APIs tied to individual keystrokes. The original excerpt also introduces invariants as leverage points.

**Caution:** This is an extract from an indexed snippet, not a full verification of the section.

### §21.3 — Emphasize what matters

The author describes three ways to make important design choices visible: **prominence** (put them where readers will look, such as interfaces and names), **repetition** (reinforce key concepts across usages), and **centrality** (let foundational contracts shape surrounding design). An operating-system driver interface illustrates centrality.

The inverse matters too: incidental details should not be repeated everywhere or determine the architecture.

### §21.4 — Two mistakes

1. **Overexposure:** Treating too many details as important creates parameters and configuration options almost nobody needs, raising cognitive load. The buffered/unbuffered I/O example makes an incidental choice visible to ordinary users.
2. **Underexposure:** Hiding genuinely important information or lacking commonly necessary capabilities forces repeated rediscovery or reinvention and produces unknown unknowns.

A good API therefore neither exposes every implementation choice nor hides everything indiscriminately.

## Engineering example — My Engineer synthesis, not a book example

Suppose three independent repositories implement a payment approval operation. A caller may need **merchant identifier, request identifier, intended transition and authorization context**. It should not need to know which database table stores the audit log, which internal transport serializes the event or what retry implementation the service uses. Conversely, silently hiding the fact that a transition is not idempotent would conceal something important.

**Decision test:** Can a new engineer identify core invariants, outcomes and failure behavior from the contract without learning its infrastructure details?

## Candidate Principles — not approved

### AP-C05 — Make high-leverage invariants and contracts visible

- **Understanding:** Design around stable, high-value concepts; expose properties consumers need to make correct decisions.
- **Judgment:** Use with APIs, domain boundaries, contracts and technical documentation. Beware making an unproven abstraction central prematurely.
- **Evidence:** Official extract, Chapter 21 opening, indexed §21.1, §21.3.
- **Overlap:** EF-G03 (observable behavior), AP-C02 (deep modules); should be considered for consolidation.

### AP-C06 — Hide incidental detail without hiding obligations

- **Understanding:** Separate necessary information from incidental configuration or implementation.
- **Judgment:** Good defaults reduce cognitive load, but risks, limits, side effects and business invariants must remain discoverable.
- **Evidence:** Official extract, Chapter 21 opening, §21.4.
- **Overlap:** AP-C01 (complexity), AP-C02 (module design), EF-G08 (reader focus).

## Open evidence and next step

- Fetch/read the PDF in a session that supports its full size (or via an authorized user-provided copy); inspect full §21.1, §21.2 and §21.5.
- Read revised Chapter 6 and both *Clean Code* comparison excerpts before claiming second-edition coverage.
- Compare with independent research on information hiding and abstraction; do not auto-promote AP-C05/06 or change AGENTS.md/workflows.

**Integrity note:** The source is author-hosted and legitimate; coverage is **partial** due tooling constraints. These notes do not reproduce book pages.
