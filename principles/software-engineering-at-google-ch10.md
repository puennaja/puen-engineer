# Software Engineering at Google — Chapter 10: Documentation

- **Approval:** 🟢 Accepted — summary and principles approved as contextual guidance, not mandatory policy; independent corroboration still pending.
- **Status:** Accepted — reference knowledge; non-mandatory guidance
- **Evidence:** Full publicly published chapter reviewed including examples and conclusion; original analytical paraphrase, not a translation
- **Author(s):** Tom Manshreck
- **Primary source:** https://abseil.io/resources/swe-book/html/ch10.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch10.th.md)
- **Reviewed:** 2026-10-10

**Attribution:** Chapter analysis paraphrases the author's arguments and examples. Candidate IDs and workflow mappings are **My Engineer synthesis**, not formal Google standards. Historical quantitative figures are not current benchmarks.

## Full-chapter analytical notes

### 1. Documentation is engineering work

Docs pay off mainly to future readers; writing can expose a confused API. Engineers usually own creation and maintenance.

**Engineering judgment / limitations:** Not every change needs a large standalone document; use a risk- and consumer-based threshold.

### 2. Documentation as code and the wiki failure

The GooWiki case had stale duplicates and unclear ownership. Moving canonical documents into revision control, reviews, bug tracking and code-adjacent Markdown improved maintenance.

**Engineering judgment / limitations:** Source control does not guarantee truth; a review process can add friction.

### 3. Audience and discoverability

Identify the primary audience; distinguish seekers who need scannable reference from stumblers who need orientation. Do not mix API consumers with maintainers without reason.

**Engineering judgment / limitations:** Over-segmentation can make discovery harder. Keep navigation concise.

### 4. Document types have different jobs

Reference/API comments, design docs, tutorials, conceptual guides and landing pages serve distinct purposes. Tutorials show user actions and prerequisites; design docs preserve goals, alternatives and trade-offs.

**Engineering judgment / limitations:** A concise cross-link is preferable to duplicating all details.

### 5. Review, freshness and writing judgment

Technical review tests accuracy; audience review tests comprehension; editorial review tests consistency. The author contrasts completeness, correctness and clarity; deprecate stale docs and record review ownership.

**Engineering judgment / limitations:** The writing-philosophy section is explicitly partly the author's opinion, not an enforced Google standard.

## Candidate Principles (proposed, not approved)

### EF-G13 — Make Documentation Owned and Changeable

- **English name:** Make Documentation Owned and Changeable
- **Primary chapter anchors:** Documentation Is Like Code; Google Wiki; Documentation Reviews
- **Layer A — Meaning and mechanism:** Useful for long-lived docs, code-adjacent references and decision records.
- **Layer B — Trade-offs / failure modes:** Governance overhead for tiny or transient notes; stale Git docs remain possible.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G14 — Write for One Primary Reader and Purpose

- **English name:** Write for One Primary Reader and Purpose
- **Primary chapter anchors:** Know Your Audience; Documentation Types; Documentation Philosophy
- **Layer A — Meaning and mechanism:** Choose reference vs tutorial vs design record by consumer needs.
- **Layer B — Trade-offs / failure modes:** Splitting excessively creates navigation and ownership costs.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G15 — Deprecate Stale or Competing Sources

- **English name:** Deprecate Stale or Competing Sources
- **Primary chapter anchors:** Documentation Is Like Code; Deprecating Documents
- **Layer A — Meaning and mechanism:** Mark canonical references and point obsolete docs to replacements.
- **Layer B — Trade-offs / failure modes:** Deleting history can hurt archaeology; archive thoughtfully.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

## Proposed My Engineer mappings

Solution Design: decision records; Technical Discovery: canonical as-is evidence; Delivery Planning: docs as necessary delivery work; AGENTS.md: concise scoped instructions.

## Cautions and next step

Google's 2020 context differs from current local/team practices. Before making any candidate a policy, corroborate with newer/independent evidence, identify counterexamples and perform a safe non-sensitive pilot. Do not alter existing accepted workflows, AGENTS.md or create AI skills as a side effect.

**Full primary text:** https://abseil.io/resources/swe-book/html/ch10.html
