# Release & Operations Workflow — v1.0 Design Draft

- **Status:** Design Draft — **not Accepted**; only Q1 below is agreed
- **Date:** 2026-10-10
- **Track:** A — My Engineer, vendor/tool-neutral engineering practices
- **Upstream:** [Implementation (Accepted)](./implementation.md), [Verification (Accepted)](./verification.md), [Feature Delivery Lifecycle](./feature-delivery-lifecycle.md)
- **Thai companion:** [Release & Operations (TH)](./release-operations.th.md)
- **Design principle:** Work Item verification, merge, deployment, feature enablement and business outcome are distinct events and decisions.

## Q1 — Fixed Human Release Gate (agreed 2026-10-10)

**Owner decision: A — Fixed Human Release Gate for Release & Operations v1.0.**

Every **production release** must receive **explicit approval by an authorized human release decision owner** before execution. After approval, an authorized, policy-compliant deployment pipeline can automate the approved release operations **within its specifically approved scope**. This is **release-level approval**, separate from Feature-level Design/Planning gates, per-Work-Item Implementation/Verification gates, and human-controlled MR Ready/Merge decisions.

**Boundaries:**
- A Work Item marked `Verified for Release Review` is **not** authorization to merge, deploy or enable user-facing changes. One Release may contain multiple Work Items/repos.
- AI may **prepare, inspect and recommend** release readiness, evidence, compatibility, rollout, monitoring and recovery plans where access is authorized. AI may not infer approval from green CI or exercise production privileges without separate authorization.
- An approved pipeline is not the same thing as unrestricted agent action. Actual production credentials, deployment actions, release approver identity, protected-environment permissions, incident ownership and company policies remain project-specific and are **not granted by approving this design decision**.
- This decision does **not** require a human to click every pipeline step. It requires explicit human authorization before each production release, with automation allowed inside the approved bounds.
- Production deployment, feature flag enablement or other exposure-affecting changes can have different risk and approval consequences; their exact **release authorization envelope** is **open for Q2**.
- No deployment, production access, repo policy change, release automation or new AI Skill is authorized by this draft.

## Candidate responsibilities (not yet approved as a workflow)

1. **Release candidate and readiness:** identify immutable build/artifact, included changes, dependencies, CI evidence, migration and rollback/roll-forward limitations.
2. **Coordination and Go/No-Go:** identify release owner, affected services/environments, rollout plan, risk/stop criteria, communications and authorization.
3. **Controlled deployment and validation:** execute only through authorized pipeline/people, validate deployment completion and critical behavior with safe observability.
4. **Operations, recovery and learning:** monitor agreed signals, identify regressions, mitigate/rollback/roll-forward as safe, involve incident owners and record actual outcomes.

**Adaptive depth:** Low-risk and reversible releases should remain lightweight; high-risk, multi-service, data-changing or hard-to-reverse releases require stronger compatibility, recovery, monitoring and qualified human ownership. These are proposed practices, **not additional auto-approved gates**.

## Q2 — Proposed next decision (not yet agreed)

**What exactly does one human Production Release approval authorize?**

- **A. Bounded Release Authorization (recommended):** bind human approval to a specific artifact/version or immutable release manifest, production target(s), rollout/exposure scope and meaningful validity constraints. Routine pipeline continuation within that envelope can be automatic; material changes/retries that alter risk or scope require renewed approval. Incident recovery remains subject to its separately authorized runbook/policy.
- **B. Broad release-window authorization:** approve a general deployment window or batch, with pipeline selecting eligible builds within that window subject to policy.

**Pending:** decide Q2, then refine release-readiness, recovery, observation, exception handling and the human release decision record before a v1.0 approval review.
