# Software Engineering at Google — Chapter 21: Dependency Management

- **Approval:** 🟢 Accepted — summary and principles approved as contextual guidance, not mandatory policy; independent corroboration still pending.
- **Status:** Draft v0.1 — candidate principles are **not accepted**
- **Chapter author(s):** Titus Winters
- **Primary full chapter:** https://abseil.io/resources/swe-book/html/ch21.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch21.th.md)
- **Reviewed:** 2026-10-10
- **Method:** Analytical summary of publicly published full chapter; original paraphrase; not reproduction. Author's positions in section notes; IDs/decisions below are **My Engineer syntheses**.

**Caution:** Google's 2020 tooling and organizational scale are context, not today's universal standard. Reassess contemporary applicability.

## Chapter analysis

### 1. Dependencies form a changing graph

Transitive dependencies and independent version changes create network constraints; a choice that works for one direct package may fail across the diamond dependency graph. Other organizations control upstream changes.

**Limit / trade-off:** A dependency solver finding a combination does not establish security or runtime compatibility.

### 2. Versioning is a model, not a guarantee

Semantic Versioning and minimum version selection attempt to encode compatibility with varying success; consumers may depend on unexpected behavior (Hyrum's Law). Time, version pinning, diamond dependencies and maintenance all interact.

**Limit / trade-off:** Neither pin-everything nor unpin-everything is universally correct.

### 3. Bundled versions versus Live at Head

Bundled distribution gives responsibility for a tested compatible set to a distributor. Google's Live at Head favors one current version, downstream CI and provider responsibility for migration, while explicitly acknowledging costs and impracticality across OSS organizations.

**Limit / trade-off:** Do not interpret Google's preferred model as an achievable rule for our external libraries.

### 4. Compatibility tests and update cadence

An update strategy must preserve a coherent reproducible build, handle security fixes, run suitable tests and detect downstream breakage. The chapter explores how broad CI coverage is costly; risk-based selected dependent tests are a possible compromise.

**Limit / trade-off:** CI coverage cannot prove all behavior; supply-chain and license requirements require independent modern evidence.

### 5. Exporting dependencies and OSS sustainability

Publishing a library creates maintenance, reputation, legal and community obligations. The gflags history shows how an internal/exported fork can drift when changes cannot flow both directions.

**Limit / trade-off:** Forking is sometimes required for security, urgent fixes or independent governance.

## Candidate Principles — Not Yet Approved

### EF-G34 — Assess Dependency Graph Risk, Not Just Direct Imports

- **English name:** Assess Dependency Graph Risk, Not Just Direct Imports
- **Source anchors:** Why Is Dependency Management So Difficult?; Diamond Dependency
- **Layer A — Understanding / application:** Inspect transitive versions, compatibility and ownership for material dependencies.
- **Layer B — Judgment / failure modes:** Exhaustive graph analysis can be costly; prioritize critical dependencies.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G35 — Treat Version Promises as Hypotheses Requiring Tests

- **English name:** Treat Version Promises as Hypotheses Requiring Tests
- **Source anchors:** Semantic Versioning; Live at Head; Compatibility
- **Layer A — Understanding / application:** Use lockfiles/reproducible builds plus upgrade and contract tests.
- **Layer B — Judgment / failure modes:** Over-pinning causes stale security patches; unpinning can break reproducibility.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G36 — Make Dependency Ownership and Update Paths Explicit

- **English name:** Make Dependency Ownership and Update Paths Explicit
- **Source anchors:** Live at Head; Exporting Dependencies; gflags
- **Layer A — Understanding / application:** Know who maintains a critical dependency and how updates or forks get merged.
- **Layer B — Judgment / failure modes:** Strong provider responsibility is hard across organizations without contracts.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

## Proposed mapping to My Engineer

Technical Discovery: dependency evidence; Solution Design: vendor/build-vs-buy risk; Delivery Planning: upgrade work and compatibility tests; Implementation/Operations: SBOM/security updates and lockfile governance (modern cross-check needed).

## Review before accepting

1. Check fit for solo/team/multi-repo systems instead of importing Google's policy literally.
2. Cross-check against newer evidence and credible counterexamples.
3. Identify overlaps with EF-G01–24 before growing the Registry.
4. Pilot on a non-sensitive representative change and measure actual burden and benefit.

**No approved workflows, Draft Delivery Planning, AGENTS.md, or Skills changed.**
