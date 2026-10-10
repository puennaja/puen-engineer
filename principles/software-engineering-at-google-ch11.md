# Software Engineering at Google — Chapter 11: Testing Overview

- **Status:** Draft study notes v0.1 — candidates **not approved**
- **Evidence:** Full publicly published chapter reviewed including examples and conclusion; original analytical paraphrase, not a translation
- **Author(s):** Adam Bender
- **Primary source:** https://abseil.io/resources/swe-book/html/ch11.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch11.th.md)
- **Reviewed:** 2026-10-10

**Attribution:** Chapter analysis paraphrases the author's arguments and examples. Candidate IDs and workflow mappings are **My Engineer synthesis**, not formal Google standards. Historical quantitative figures are not current benchmarks.

## Full-chapter analytical notes

### 1. Tests make change sustainable

Developer-driven automation reduces repeat debugging, creates confidence, gives executable examples and reveals difficult APIs. The Google Web Server story illustrates testing-culture change, not a universal effect size.

**Engineering judgment / limitations:** A pile of brittle tests can make change harder than before.

### 2. Write, run, react

The value chain includes writing checks, running them frequently and responding quickly to failures. Ignored red tests destroy trust.

**Engineering judgment / limitations:** Tests are not evidence if not actually executed or if failures are ignored.

### 3. Size differs from scope

Google calls tests small/medium/large by resource/process constraints, separately from narrow/broad code scope. Small tests are usually faster; medium can use local processes; large can involve remote systems.

**Engineering judgment / limitations:** Their exact constraints are organization-specific, not universal definitions of unit/integration tests.

### 4. Balance levels and test real behavior

The testing pyramid is a heuristic; chapter illustrates roughly 80% narrow, 15% integration, 5% E2E as a Google guideline, not a target to mandate. Real dependencies can be better than mock-heavy tests when feasible.

**Engineering judgment / limitations:** Coverage counts executed lines, not correctness or useful assertion quality.

### 5. Hermetic tests and flakiness

Isolation, determinism and fast feedback preserve trust. Sleeps, shared state and networks add nondeterminism; reruns can mask the problem. Beware large brittle suites and overspecified mocks.

**Engineering judgment / limitations:** Do not assume zero flakiness is attainable in all realistic end-to-end environments.

### 6. Culture and failure testing

Orientation, Test Certified and Testing on the Toilet made tests part of team habits. The Beyoncé Rule asks teams to test behaviors they need preserved, including errors and failures.

**Engineering judgment / limitations:** Don't copy Google's maturity badges or infrastructure at small scale.

## Candidate Principles (proposed, not approved)

### EF-G19 — Use Tests to Protect Changeability

- **English name:** Use Tests to Protect Changeability
- **Primary chapter anchors:** Why Do We Write Tests?; Benefits of Testing Code
- **Layer A — Meaning and mechanism:** Preserve business invariants and compatibility during refactoring.
- **Layer B — Trade-offs / failure modes:** Brittle tests overconstrain implementation and inhibit improvement.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G20 — Keep Test Feedback Fast, Reliable, and Actionable

- **English name:** Keep Test Feedback Fast, Reliable, and Actionable
- **Primary chapter anchors:** Write, Run, React; Test Sizes; Flaky Tests Are Expensive
- **Layer A — Meaning and mechanism:** Favor isolated deterministic checks and react to failures promptly.
- **Layer B — Trade-offs / failure modes:** Excessive mocking hides integration faults; large tests still needed.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G21 — Test Important Behavior, Not Coverage Targets

- **English name:** Test Important Behavior, Not Coverage Targets
- **Primary chapter anchors:** The Beyoncé Rule; A Note on Code Coverage; Test Scope
- **Layer A — Meaning and mechanism:** Choose meaningful failure, contract and business-rule scenarios.
- **Layer B — Trade-offs / failure modes:** No single ratio or percent guarantees adequate assurance.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

## Proposed My Engineer mappings

Implementation & Verification future workflow: tests as evidence; Technical Discovery: locate missing coverage; Solution Design: decide invariants, test seams; Delivery Planning: test work included in slices.

## Cautions and next step

Google's 2020 context differs from current local/team practices. Before making any candidate a policy, corroborate with newer/independent evidence, identify counterexamples and perform a safe non-sensitive pilot. Do not alter existing accepted workflows, AGENTS.md or create AI skills as a side effect.

**Full primary text:** https://abseil.io/resources/swe-book/html/ch11.html
