# Software Engineering at Google — Chapter 15: Deprecation

- **Approval:** 🟢 Accepted — summary and principles approved as contextual guidance, not mandatory policy; independent corroboration still pending.
- **Status:** Accepted — reference knowledge; non-mandatory guidance
- **Chapter author(s):** Hyrum Wright
- **Primary full chapter:** https://abseil.io/resources/swe-book/html/ch15.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch15.th.md)
- **Reviewed:** 2026-10-10
- **Method:** Analytical summary of publicly published full chapter; original paraphrase; not reproduction. Author's positions in section notes; IDs/decisions below are **My Engineer syntheses**.

**Caution:** Google's 2020 tooling and organizational scale are context, not today's universal standard. Reassess contemporary applicability.

## Chapter analysis

### 1. Why removal matters

Software's value is in useful capabilities, not lines of code. An obsolete service keeps accruing upgrade, operational and coordination costs and can constrain its replacement. Age alone is not proof of obsolescence.

**Limit / trade-off:** A bad deprecation can cost more than continuing support; measure real usage and replacement readiness.

### 2. Advisory versus compulsory

Advisory deprecation warns and offers alternatives but rarely finishes migrations by itself. Compulsory deprecation sets a real removal deadline, authority to enforce it and staffed migration help.

**Limit / trade-off:** Deadlines with no migration funding are unfair and can imperil critical clients.

### 3. Reveal hidden dependencies and plan gradual exit

Static code search, dynamic logs and staged shutdown exercises reveal unexpected consumers. Reduce users incrementally, plan milestones, measure migration instead of waiting until the switch-off moment.

**Limit / trade-off:** Deliberately breaking production to discover clients needs approval, guardrails and recovery.

### 4. Govern deprecation as an owned project

Named owners, communications, migration guidance, tools and support create incentives to finish. Inventory and verify consumers. Stop backsliding: prevent new calls to an already deprecated API via warnings or lint rules.

**Limit / trade-off:** Do not ban new uses before a viable, authorized replacement exists.

### 5. Plan removal at design time

Systems should expose ownership and dependencies well enough to be turned down later. Removing old code is also feature delivery: resource and cognitive load is reclaimed.

**Limit / trade-off:** Some regulated retention or client obligations can restrict deletion.

## Candidate Principles — Not Yet Approved

### EF-G28 — Treat Retirement as Engineering Work

- **English name:** Treat Retirement as Engineering Work
- **Source anchors:** Why Deprecate?; Managing the Deprecation Process
- **Layer A — Understanding / application:** Plan deprecation, usage discovery, support and incremental milestones.
- **Layer B — Judgment / failure modes:** Deprecation is not justified by software age alone.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G29 — Match Deprecation Deadlines with Migration Support

- **English name:** Match Deprecation Deadlines with Migration Support
- **Source anchors:** Advisory Deprecation; Compulsory Deprecation
- **Layer A — Understanding / application:** Fund migrations, name an owner and communicate a genuine end-of-life date.
- **Layer B — Judgment / failure modes:** Forced cutoff without authority, observability or rollback can harm clients.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

### EF-G30 — Prevent New Dependence During Retirement

- **English name:** Prevent New Dependence During Retirement
- **Source anchors:** Deprecation Tooling; Preventing backsliding
- **Layer A — Understanding / application:** Annotate/warn/lint against new use while systematically reducing existing consumers.
- **Layer B — Judgment / failure modes:** Premature bans may block legitimate essential work.
- **Layer C — Evidence / evolution:** Primary full chapter supports thematic attribution; independent corroboration and real-work pilot remain pending. Revalidate upon new evidence or contrary operational findings.
- **Status:** Candidate

## Proposed mapping to My Engineer

Technical Discovery: locate consumers; Solution Design: compatibility and exit plans; Delivery Planning: migration staffing and milestones; Release & Operations: controlled retirement and monitoring.

## Review before accepting

1. Check fit for solo/team/multi-repo systems instead of importing Google's policy literally.
2. Cross-check against newer evidence and credible counterexamples.
3. Identify overlaps with EF-G01–24 before growing the Registry.
4. Pilot on a non-sensitive representative change and measure actual burden and benefit.

**No approved workflows, Draft Delivery Planning, AGENTS.md, or Skills changed.**
