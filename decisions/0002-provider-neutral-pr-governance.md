# ADR-0002: Provider-neutral AI contribution identity and PR governance

- **Date:** 2026-10-18
- **Status:** Accepted direction; technical implementation proposed / unverified
- **Scope:** My Engineer repositories and AI-assisted delivery
- **Decider:** Human repository owner
- **Related:** [ADR-0001](0001-universal-engineering-system.md), [feature delivery](../workflows/ai-assisted-feature-delivery.md)

## Context

When an AI implementation tool, AI reviewer and human owner all act through one GitHub identity, commit, push, review and approval attribution becomes ambiguous. A PR metadata preflight is not an AI code review. The target system should automatically request independent AI analysis on PR opening/updates while preserving human accountability and minimizing incremental API cost.

Provider neutrality is essential: the implementation role may be fulfilled by Codex, Claude Code, or another supported agent. **Never hardcode an implementation provider as the developer role.**

## Decision — role and authority boundaries

| Logical role | Responsibilities | Permitted authority | Required attribution |
| --- | --- | --- | --- |
| **Implementation Agent** | Work item implementation, tests/evidence, commits/pushes, Draft PR and responses to findings | Repository writing only within approved scope | Actual authenticated actor, underlying tool/provider/model where known, work item, commit SHA |
| **AI Reviewer** | Independent diff/spec/test/security review; findings and machine-readable outcome when supported | Read repository, comment/status as specifically granted; no merge or human approval | Actual authenticated reviewer actor, reviewer system/provider/model where known, reviewed head SHA |
| **Human Gate Owner** | Authorize privileged steps, assess evidence, make final approval/merge decisions | Final merge authorization subject to repository policies | Actual human GitHub actor and timestamp |

Separate **role**, **tool/provider/model**, **Git commit author**, **authenticated GitHub actor**, **PR author**, **review author**, **approval** and **merge actor**. They are not interchangeable. Commit author strings, PR text and labels are informational and not cryptographic proof of identity.

Prefer scoped machine identities (for example, distinct GitHub Apps) when a verified integration supports them. Exact application names and credentials are **not yet selected**. The ChatGPT GitHub connector cannot be assumed to act as a new GitHub App identity simply because one exists. Never impersonate the human or share credentials to simulate a different actor.

## Target PR lifecycle

1. Human assigns/authorizes a work item and its boundaries.
2. Implementation Agent works on a feature branch, records actual tests and trade-offs, opens Draft PR against protected base.
3. **PR preflight** verifies metadata/contract only; it does not validate code, architecture or safety.
4. **Independent AI review** is requested on initial PR and on new head commits. Reviewer compares actual diff against relevant requirements; posts concrete BLOCKER/MAJOR/MINOR findings, evidence and reviewed head SHA. A stale review must not qualify a newer commit.
5. Findings lead to scoped rework and renewed review. No actionable blockers -> PR can move to human-review state, subject to checks.
6. **Human Gate Owner alone decides final approval and merge.** AI may recommend readiness but must not submit a human approval or auto-merge. Permissions and branch rules should technically enforce these boundaries where platform capabilities permit.

A completed metadata check, passing tests, AI review without findings and human approval are **four distinct states**. Failure/missing evidence should not become a success by default. A draft-to-ready transition does not mean a PR is approved.

## Cost, automation and security constraints

- **Subscription-first** is a preference, not a claim that a ChatGPT Plus or Claude Pro plan covers unattended CI. Validate available product entitlements/terms and runtime behavior before implementing automation.
- No separately billed model API, new credential, self-hosted runner or paid service without explicit owner authorization.
- The existing GitHub Actions `ai-pr-review.yml` in `puen-harness` is a **deterministic preflight only**, despite its filename. Rename to `pr-preflight.yml` in a separate scoped change when ready.
- Automatic semantic AI reviews remain a **target design**, not an implemented feature. In-chat or scheduled reviews do not magically run as a GitHub workflow.
- Do not execute untrusted PR code with privileged bot tokens or personal subscription credentials. Use least privilege, source pinning, environment isolation, narrowly scoped GitHub App tokens, and verify PR head SHA before reporting current results.
- Human approval enforcement requires validating actual branch protection/rulesets and account permissions. An author generally cannot supply their own separate required approving review.

## Implementation plan — proposed, not yet authorized/executed

- **GOV-001 Identity & permissions:** identify authenticated actors and audit metadata, verify safe GitHub App integration for independent implementation/review identities; no new bots or tokens yet.
- **GOV-002 PR governance:** align preflight naming, review policy, head-SHA freshness, check statuses and enforceable human merge gate where supported.
- **GOV-003 Automated AI review:** prototype event-driven independent review with explicitly approved provider/auth/cost path, isolated execution and evidence; test initial PR, update, blocker, retry/failure and stale-review cases.

## Alternatives and trade-offs

- **One human account plus role labels:** easy and free but actor attribution is weak; useful as interim disclosure only.
- **Separate personal GitHub accounts for AI tools:** extra account/credential overhead and does not automatically integrate connected tools.
- **Distinct GitHub Apps:** potentially better actor attribution and least-privilege separation, but requires integration and security work.
- **Paid API-backed hosted CI:** convenient automation, but incurs new inference billing and is not currently authorized.
- **Subscription-backed self-hosted review:** possible research direction only; needs entitlement, security and reliability verification.

## Consequences and validation

The roles must remain provider-neutral. Actual review actor and push/merge actor must be independently auditable; a reviewer recommendation cannot be confused with human sign-off. When a new provider is introduced, adapt tool-specific transports without rewriting governance roles.

Do not mark GOV-001/002/003 complete until demonstrated with actual GitHub actor identities, permission tests, PR lifecycle evidence, and owner-confirmed merge restrictions. This ADR records agreed **governance direction**, not a verified working bot pipeline.
