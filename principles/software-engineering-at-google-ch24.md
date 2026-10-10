# Software Engineering at Google — Chapter 24: Continuous Delivery

- **Approval:** 🟢 Accepted — summary and principles approved as contextual guidance, not mandatory policy; independent corroboration still pending.
- **Status:** Draft v0.1 — candidate principles are **not accepted**
- **Chapter author(s):** Radha Narayan, Bobbi Jones, Sheri Shipe, David Owens
- **Primary full chapter:** https://abseil.io/resources/swe-book/html/ch24.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch24.th.md)
- **Reviewed:** 2026-10-10
- **Method:** Analytical summary of publicly published full chapter; original paraphrase; not reproduction. Author's positions in section notes; IDs/decisions below are **My Engineer syntheses**.

**Caution:** Google's 2020 tooling and organizational scale are context, not today's universal standard. Reassess contemporary applicability.

## Chapter analysis

### 1. Code value arrives with users

Keeping completed code unreleased delays feedback and accumulates risk. Small, frequent release batches make it easier to identify regressions and reduce stale work.

**Limit / trade-off:** Low frequency can still be intentional for client/app-store constraints or regulation.

### 2. Readiness before frequency

The chapter calls out agility, automation, isolation, reliability, data-driven decisions and phased rollout. Teams can achieve deploy-on-demand capability without forcing every commit live immediately.

**Limit / trade-off:** Frequent releases are not safe without CI, testing, rollback and monitoring.

### 3. Keep release trains predictable

Massive batches and late cherry-picks make release engineers a bottleneck. Predictable trains and sensible cutoffs protect operations and make missing a train less costly.

**Limit / trade-off:** Hard deadlines must not trump high-impact user-facing correctness issues.

### 4. Progressive exposure with guardrails

Use flags to decouple deploy from feature exposure; stage rollouts and compare health metrics, use representative device/customer coverage when exhaustive qualification is impossible. Rare accessibility or regional issues still matter.

**Limit / trade-off:** A/B needs valid sample size; flags add lifecycle debt; production probes cannot undo all impact.

### 5. Release only worthwhile functionality

Features cost users bandwidth, storage and cognitive load. Dynamic delivery and experiments can balance cost against actual usage. Operational metrics and a coordinated human decision determine go/no-go.

**Limit / trade-off:** Usage is not the sole measure of social or accessibility value.

## Candidate Principles — Not Yet Approved

### EF-G37 — Build the Capability to Release Safely on Demand

- **English name:** Build the Capability to Release Safely on Demand
- **Source anchors:** Idioms of Continuous Delivery; Conclusion
- **Layer A — Understanding / application:** Invest in CI, repeatable deploy, rollback/rollforward and visibility.
- **Layer B — Judgment / failure modes:** Do not mandate deployment frequency without customer and regulatory context.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G38 — Release Small, Isolated Changes with Progressive Exposure

- **English name:** Release Small, Isolated Changes with Progressive Exposure
- **Source anchors:** Velocity Is a Team Sport; Shifting Left; Staged Rollout
- **Layer A — Understanding / application:** Use small changes, flags and monitored canaries when risks justify them.
- **Layer B — Judgment / failure modes:** Flag proliferation and unsafe automated promotion can worsen operations.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G39 — Use User Impact and Health Guardrails for Release Decisions

- **English name:** Use User Impact and Health Guardrails for Release Decisions
- **Source anchors:** Quality and User-Focus; Meet Your Release Deadline; Ship Only What Gets Used
- **Layer A — Understanding / application:** Define relevant error/latency/functionality indicators and named go/no-go owner.
- **Layer B — Judgment / failure modes:** A numeric target alone can hide accessibility or rare-user failures.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

## Proposed mapping to My Engineer

FDL: outcome feedback loop; Solution Design: feature isolation; Delivery Planning: small release increments; Release & Operations: deployment, rollback and progressive rollout.

## Review before accepting

1. Check fit for solo/team/multi-repo systems instead of importing Google's policy literally.
2. Cross-check against newer evidence and credible counterexamples.
3. Identify overlaps with EF-G01–24 before growing the Registry.
4. Pilot on a non-sensitive representative change and measure actual burden and benefit.

**No approved workflows, Draft Delivery Planning, AGENTS.md, or Skills changed.**
