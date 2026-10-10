# Software Engineering at Google — Chapter 16: Version Control and Branch Management

- **Approval:** 🟢 Accepted — summary and principles approved as contextual guidance, not mandatory policy; independent corroboration still pending.
- **Status:** Draft v0.1 — candidate principles are **not accepted**
- **Chapter author(s):** Titus Winters
- **Primary full chapter:** https://abseil.io/resources/swe-book/html/ch16.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch16.th.md)
- **Reviewed:** 2026-10-10
- **Method:** Analytical summary of publicly published full chapter; original paraphrase; not reproduction. Author's positions in section notes; IDs/decisions below are **My Engineer syntheses**.

**Caution:** Google's 2020 tooling and organizational scale are context, not today's universal standard. Reassess contemporary applicability.

## Chapter analysis

### 1. VCS establishes history and source of truth

Atomic commits, provenance, collaboration and a common known latest version reduce coordination errors. The chapter separates VCS mechanisms from policies applied to branches and repositories.

**Limit / trade-off:** Distributed copies are fine as workspaces; shared integration still needs a recognized source of truth.

### 2. Trunk-based versus long-lived development branches

Google argues large isolated development branches defer integration, increase merge conflicts and obscure culprit finding. It advocates small changes on a shared trunk guarded by CI, review and feature isolation; release branches can still make sense for maintained shipped versions.

**Limit / trade-off:** Trunk-based work without reliable tests/rollback is not automatically safer.

### 3. One-Version Rule and scaling

When teams can depend on arbitrarily divergent versions of the same component, integration and migration costs multiply. Google's One-Version Rule and monorepo simplify organization-wide compatibility but require significant tooling and coordination.

**Limit / trade-off:** One-version policy is not feasible for independent vendors, separated access controls or every multi-repo environment.

### 4. Repository choice is contextual

The chapter considers centralized versus distributed VCS, monorepo versus manyrepo, and virtual-monorepo possibilities. Multi-repo supports independent access and release needs; monorepo simplifies cross-code search and refactoring.

**Limit / trade-off:** Do not reorganize repositories just to imitate Google; evidence should show specific coordination costs.

### 5. Current practice boundaries

Google's choices are historical and assume unusually integrated build/testing infrastructure; suggested future VCS directions in the chapter are predictions, not verified current facts.

**Limit / trade-off:** Use actual GitLab CI, multi-repo interfaces and release constraints before choosing branch conventions.

## Candidate Principles — Not Yet Approved

### EF-G31 — Maintain an Explicit Integration Source of Truth

- **English name:** Maintain an Explicit Integration Source of Truth
- **Source anchors:** Source of Truth; Why Version Control Matters
- **Layer A — Understanding / application:** Agree which revision is integration base and record compatible component revisions.
- **Layer B — Judgment / failure modes:** Single trunk across independent systems may be infeasible; define scoped truth.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G32 — Integrate Small Changes Early with Verification

- **English name:** Integrate Small Changes Early with Verification
- **Source anchors:** Dev Branches; Trunk-based development; Release Branches
- **Layer A — Understanding / application:** Favor short-lived branches, CI and tests, with incomplete features isolated.
- **Layer B — Judgment / failure modes:** Without safety infrastructure, fast merges can spread regressions.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G33 — Constrain Version Divergence Where Coordination Pays Off

- **English name:** Constrain Version Divergence Where Coordination Pays Off
- **Source anchors:** One-Version Rule; Monorepo/Manyrepo trade-offs
- **Layer A — Understanding / application:** Specify version/contract compatibility and upgrade responsibility across related repos.
- **Layer B — Judgment / failure modes:** Avoid literal global one-version mandates on independently released systems.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

## Proposed mapping to My Engineer

Delivery Planning: dependency sequencing and merge cadence; Technical Discovery: compatible service versions; Implementation/Verification: CI on small changes; Release: release branch and deployment constraints.

## Review before accepting

1. Check fit for solo/team/multi-repo systems instead of importing Google's policy literally.
2. Cross-check against newer evidence and credible counterexamples.
3. Identify overlaps with EF-G01–24 before growing the Registry.
4. Pilot on a non-sensitive representative change and measure actual burden and benefit.

**No approved workflows, Draft Delivery Planning, AGENTS.md, or Skills changed.**
