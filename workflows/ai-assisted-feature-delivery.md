# AI-assisted feature delivery

- Status: Draft — pending review
- Created: 2026-10-09
- Scope: personal engineering workflow (generic example; no company-confidential material)

## Goal
Build features efficiently with AI assistance while retaining developer ownership and correctness.

## Proposed stages
1. **Intake** — read requirement, identify ambiguity, establish acceptance criteria and use cases.
2. **Explore** — identify impacted repositories, architecture, dependencies, and existing patterns.
3. **Plan** — split tasks, compare feasible approaches, enumerate edge cases, create implementation/test plan.
4. **Implement** — apply focused changes and track assumptions.
5. **Verify** — unit/integration tests as applicable, independent checks, and relevant manual validation.
6. **Review** — separate diff review; developer independently validates findings and final behavior.
7. **Deliver** — agreed commit format, draft merge/pull request, and traceability to the work item.

## Multi-repository consideration
When a feature spans multiple repositories, maintain one shared feature design but identify per-repository changes, version/contract dependencies, and test responsibilities. Do not assume a tool can automatically see repositories outside its workspace.

## Agent instructions
Prefer a short project-level `AGENTS.md` describing local conventions and entry points. Keep procedural skills outside the file, loaded on demand from `puen-stack`.

## Open decisions
- Which actions should be automated versus require approval?
- Which skills should be implemented first?
- How will skills be installed or synchronized into individual workspaces?
- What commit conventions and PR/MR template should be used?
