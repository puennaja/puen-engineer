# Software Engineering at Google — Chapter 09: Code Review

- **Status:** Draft study notes v0.1 — candidates **not approved**
- **Evidence:** Full publicly published chapter reviewed including examples and conclusion; original analytical paraphrase, not a translation
- **Author(s):** Tom Manshreck and Caitlin Sadowski
- **Primary source:** https://abseil.io/resources/swe-book/html/ch09.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch09.th.md)
- **Reviewed:** 2026-10-10

**Attribution:** Chapter analysis paraphrases the author's arguments and examples. Candidate IDs and workflow mappings are **My Engineer synthesis**, not formal Google standards. Historical quantitative figures are not current benchmarks.

## Full-chapter analytical notes

### 1. Review is more than bug finding

Google reviews correctness, comprehensibility, ownership and language conventions; default approval roles can overlap. New code is a maintenance liability, so reusing proven patterns matters.

**Engineering judgment / limitations:** Google's three approval bits are not a mandatory org chart for small projects.

### 2. Different review goals require different evidence

Greenfield needs API/design alignment; behavior changes need tests/benchmarks; bug fixes should stay focused with regression evidence; generated refactors need tool/process evidence and local impact review.

**Engineering judgment / limitations:** Do not reopen accepted architecture in every line comment; escalate design questions separately.

### 3. Respect, knowledge and ownership

Review is a social/knowledge-sharing practice, not just a gate. Questions can expose confusing APIs, and an author may choose among equally valid solutions. Review history helps archaeology.

**Engineering judgment / limitations:** Unkind feedback, style wars and gatekeeper bottlenecks can erase benefits.

### 4. Small, reviewable changes and meaningful descriptions

Small atomic changes speed comprehension, integration and rollback; first line and body of change description explain what and why. Google mentions ~200 lines and 24-working-hour response as local defaults.

**Engineering judgment / limitations:** Don't enforce fixed line counts; generated sweeping changes or cohesive features may be larger.

### 5. Automation and responsive review

Presubmits handle formatting/lint/test checks, leaving reviewers to judge design and intent. Minimize unnecessary reviewers while involving owners for real boundary risks.

**Engineering judgment / limitations:** AI suggestions are a separate aid; no model output is a substitute for verified tests and accountable human approval.

## Candidate Principles (proposed, not approved)

### EF-G22 — Review for Correctness, Comprehensibility, and Maintainability

- **English name:** Review for Correctness, Comprehensibility, and Maintainability
- **Primary chapter anchors:** How Code Review Works at Google; Code Consistency; Code Comprehension
- **Layer A — Meaning and mechanism:** Apply differentiated review expectations to source changes.
- **Layer B — Trade-offs / failure modes:** Review cannot fix upstream product/design ambiguity alone.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G23 — Make Change Sets Reviewable and Reversible

- **English name:** Make Change Sets Reviewable and Reversible
- **Primary chapter anchors:** Write Small Changes; Bug Fixes and Rollbacks; Change Descriptions
- **Layer A — Meaning and mechanism:** Keep cohesive diffs, written rationale and tests that support rollback.
- **Layer B — Trade-offs / failure modes:** Blind line-count gates and oversplitting obscure larger intent.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G24 — Treat Review as Respectful Knowledge Exchange

- **English name:** Treat Review as Respectful Knowledge Exchange
- **Primary chapter anchors:** Psychological and Cultural Benefits; Knowledge Sharing; Be Polite and Professional
- **Layer A — Meaning and mechanism:** Ask before assuming; deference when alternatives are equivalently valid; timely constructive feedback.
- **Layer B — Trade-offs / failure modes:** Delays, humiliating comments, and reviewer dominance harm team learning.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

## Proposed My Engineer mappings

Implementation & Verification: reviewer evidence and approvals; Solution Design: avoid reopening approved choices absent new evidence; Delivery Planning: review time and ownership are real capacity.

## Cautions and next step

Google's 2020 context differs from current local/team practices. Before making any candidate a policy, corroborate with newer/independent evidence, identify counterexamples and perform a safe non-sensitive pilot. Do not alter existing accepted workflows, AGENTS.md or create AI skills as a side effect.

**Full primary text:** https://abseil.io/resources/swe-book/html/ch09.html
