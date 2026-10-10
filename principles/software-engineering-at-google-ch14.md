# Software Engineering at Google — Chapter 14: Larger Testing

- **Approval:** 🟢 Accepted — summary and principles approved as contextual guidance, not mandatory policy; independent corroboration still pending.
- **Status:** Draft v0.1 — candidate principles are **not accepted**
- **Chapter author(s):** Joseph Graves
- **Primary full chapter:** https://abseil.io/resources/swe-book/html/ch14.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch14.th.md)
- **Reviewed:** 2026-10-10
- **Method:** Analytical summary of publicly published full chapter; original paraphrase; not reproduction. Author's positions in section notes; IDs/decisions below are **My Engineer syntheses**.

**Caution:** Google's 2020 tooling and organizational scale are context, not today's universal standard. Reassess contemporary applicability.

## Chapter analysis

### 1. Why large tests exist and what they miss

Larger tests increase fidelity to production behavior and catch gaps in unit tests: stale mocks, deployment configuration, load, unexpected inputs and emergent interactions. Size (resource/process constraints) is not the same as scope (breadth of behavior).

**Limit / trade-off:** Higher fidelity costs more, takes longer and tends to be less deterministic.

### 2. Why not make everything E2E

Large tests can be slow, flaky, non-hermetic, costly and ownership-ambiguous. Testing every full path through growing service graphs does not scale. Prefer the smallest realistic system under test and compose focused integration tests at boundaries.

**Limit / trade-off:** Over-reduction loses the very integration signals that justify these tests.

### 3. Construct useful systems and data

A large test obtains a system under test, seeds data, runs actions, then verifies outcomes. Evaluate hermeticity against fidelity. Use realistic and authorized synthetic/sampled data; avoid bypassing production validation by directly writing database state when it matters.

**Limit / trade-off:** Production copies introduce privacy and safety risk; third-party calls may be costly or destructive.

### 4. Choose risk-specific verification

The chapter surveys functional/binary, UI/device, performance/load/stress, configuration, exploratory, A/B differential, UAT, production probers/canaries, disaster recovery/chaos and user evaluation. Assertions are not the only useful oracle.

**Limit / trade-off:** In-production tests can already affect users; chaos needs safety boundaries.

### 5. Fit large tests into daily workflow

A trustworthy large test must be fast enough for its intended cadence, have diagnostic failure output, ownership and a clear escalation path. Reduce sleeps with state-based polling/events; record/replay or consumer contracts can reduce dependence on live services.

**Limit / trade-off:** Recordings can become stale; excessive retry hides flakiness; not every large test belongs in presubmit.

## Candidate Principles — Not Yet Approved

### EF-G25 — Choose Test Fidelity by Risk

- **English name:** Choose Test Fidelity by Risk
- **Source anchors:** Fidelity; Common Gaps in Unit Tests; Types of Larger Tests
- **Layer A — Understanding / application:** Use realistic integration/configuration/load evidence when unit tests cannot represent the failure mode.
- **Layer B — Judgment / failure modes:** Do not buy maximum fidelity by default; production and shared-environment tests can be expensive or unsafe.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G26 — Prefer the Smallest Sufficient System Under Test

- **English name:** Prefer the Smallest Sufficient System Under Test
- **Source anchors:** Larger Tests at Google Scale; Reducing the size of your SUT
- **Layer A — Understanding / application:** Split tests at stable boundaries with explicit contracts and focused interactions.
- **Layer B — Judgment / failure modes:** Too many mocks or seams remove the real interactions at risk.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G27 — Make Integration Tests Diagnosable and Owned

- **English name:** Make Integration Tests Diagnosable and Owned
- **Source anchors:** Large Tests and the Developer Workflow; Owning Large Tests
- **Layer A — Understanding / application:** Maintain named owners, useful failure diagnostics and an appropriate run cadence.
- **Layer B — Judgment / failure modes:** Ownership bureaucracy or flaky tests that the team simply ignores.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

## Proposed mapping to My Engineer

Technical Discovery: discover actual boundaries/configuration risks; Solution Design: plan failure evidence; Delivery Planning: budget integration test work; Implementation & Verification / Release: choose testing levels and production-safe checks.

## Review before accepting

1. Check fit for solo/team/multi-repo systems instead of importing Google's policy literally.
2. Cross-check against newer evidence and credible counterexamples.
3. Identify overlaps with EF-G01–24 before growing the Registry.
4. Pilot on a non-sensitive representative change and measure actual burden and benefit.

**No approved workflows, Draft Delivery Planning, AGENTS.md, or Skills changed.**
