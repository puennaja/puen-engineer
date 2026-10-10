# Software Engineering at Google — Chapter 07: Measuring Engineering Productivity

- **Approval:** 🟢 Accepted — summary and principles approved as contextual guidance, not mandatory policy; independent corroboration still pending.
- **Status:** Accepted as study reference — principles are contextual guidance, not mandatory rules
- **Evidence:** Full publicly published chapter reviewed including examples and conclusion; original analytical paraphrase, not a translation
- **Author(s):** Ciera Jaspan
- **Primary source:** https://abseil.io/resources/swe-book/html/ch07.html
- **Thai companion:** [Thai](./software-engineering-at-google-ch07.th.md)
- **Reviewed:** 2026-10-10

**Attribution:** Chapter analysis paraphrases the author's arguments and examples. Candidate IDs and workflow mappings are **My Engineer synthesis**, not formal Google standards. Historical quantitative figures are not current benchmarks.

## Full-chapter analytical notes

### 1. Measurement must change a decision

Research work itself costs money and can change behavior. The readability case starts with a concrete question and asks what action follows either a positive or negative result.

**Engineering judgment / limitations:** If both outcomes yield the same action, reconsider collecting the metric.

### 2. Goals–Signals–Metrics (GSM)

Set a goal without defining it as a number, then a credible observable signal, then an imperfect measurable proxy. Trace each metric back to the original goal.

**Engineering judgment / limitations:** Avoid selecting dashboards first and inventing a goal later.

### 3. QUANTS balances dimensions

The chapter's QUANTS dimensions are Quality, Attention, Intellectual complexity, Tempo/velocity, Satisfaction. Optimizing review speed alone could erase the benefit of review.

**Engineering judgment / limitations:** Not every project needs five numerical indicators; discuss relevant trade-offs.

### 4. Validate quantitative proxies with qualitative evidence

A median build-time metric counted automated builds that engineers were not waiting for. Experience sampling showed the proxy was misleading. Surveys, interviews and logs reveal different aspects.

**Engineering judgment / limitations:** Surveys have recall, recency and selection bias; metric agreement does not prove causation.

### 5. Act and reevaluate

The readability research found learning and satisfaction benefits alongside process friction; language teams refined tools and made policies clearer. The goal is improvement, not scorekeeping.

**Engineering judgment / limitations:** Organizational context differs; a dedicated productivity research team is not a solo-team prerequisite.

## Candidate Principles (proposed, not approved)

### EF-G16 — Measure Only for Actionable Decisions

- **English name:** Measure Only for Actionable Decisions
- **Primary chapter anchors:** Triage: Is It Even Worth Measuring?
- **Layer A — Meaning and mechanism:** Before tracking a metric, name the decision owner and both positive/negative actions.
- **Layer B — Trade-offs / failure modes:** Skipping measurement can hide slow risks; high-consequence monitoring may be obligatory.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G17 — Trace Metrics Through Goals and Signals

- **English name:** Trace Metrics Through Goals and Signals
- **Primary chapter anchors:** Goals; Signals; Metrics
- **Layer A — Meaning and mechanism:** Start from observable outcomes; treat a number as an imperfect proxy.
- **Layer B — Trade-offs / failure modes:** Vanity metrics and Goodhart-like gaming; avoid unearned numeric precision.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

### EF-G18 — Balance Productivity and Triangulate Evidence

- **English name:** Balance Productivity and Triangulate Evidence
- **Primary chapter anchors:** QUANTS; Using Data to Validate Metrics
- **Layer A — Meaning and mechanism:** Pair flow/quality signals with engineer experience and complexity data.
- **Layer B — Trade-offs / failure modes:** Survey bias, privacy and interpretation costs; never rank individuals by crude metrics.
- **Layer C — Evidence and evolution:** Primary chapter anchors above; independent corroboration and local pilot **pending**. Attribution confidence high, local policy applicability not yet assessed. Revisit when new evidence or pilot results conflict.
- **Status:** Candidate

## Proposed My Engineer mappings

FDL and Delivery Planning: evaluate workflow cost/benefit; AI-assisted delivery: measure verified quality plus time rather than token count; leadership: avoid individual ticket/LOC scoring.

## Cautions and next step

Google's 2020 context differs from current local/team practices. Before making any candidate a policy, corroborate with newer/independent evidence, identify counterexamples and perform a safe non-sensitive pilot. Do not alter existing accepted workflows, AGENTS.md or create AI skills as a side effect.

**Full primary text:** https://abseil.io/resources/swe-book/html/ch07.html
