# ADR-0001: Universal Engineering System with adaptive delivery

- **Date:** 2026-10-09
- **Status:** Accepted (direction only)
- **Scope:** My Engineer system design
- **Decider:** Repository owner

## Context

A personal AI-assisted coding workflow must remain useful when work expands to multiple engineers or full product delivery. Basing the full lifecycle on Scrum ceremonies would conflate engineering practices with planning cadence; a rigid phase pipeline would discourage iterative learning. A single heavyweight standard would burden small changes.

## Decision

Adopt a **Universal Engineering System** that can scale from solo work to teams and products, with distinct composable concerns:

1. Engineering practices and evidence.
2. Agile-inspired adaptive delivery, with Scrum/Kanban as optional operating models.
3. Product, people, organization and operations management when scale requires them.
4. AI skills and automation as optional enablers with human accountability.

Use risk-proportional documentation, decision ownership and quality controls. Start with a small pilot. The detailed architecture is **proposed**, not yet approved.

## Alternatives considered

- **Scrum-first framework:** familiar ceremonies, but does not define architecture, test strategy or operational governance.
- **Linear SDLC:** easy to communicate, but encourages large hand-offs if applied rigidly.
- **Full multi-agent platform first:** attractive automation, but high overhead without validated workflow.

## Consequences

- Reusable engineering activities are independent of team process.
- Extra roles, artifacts and governance are introduced when risk/scale warrant them.
- More upfront thought is needed to specify profile selection and decision ownership.
- Need pilot evidence to avoid architecture becoming documentation-only.

## Follow-up

Review [Universal Engineering System](../architecture/universal-engineering-system.md), pilot the feature lifecycle, then determine the minimum set of templates and skills.
