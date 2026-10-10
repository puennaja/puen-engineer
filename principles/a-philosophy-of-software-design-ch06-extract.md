# A Philosophy of Software Design (2nd ed.) — Chapter 6: General-Purpose Modules are Deeper

- **Status:** 🟡 Draft v0.1 — **partial original-book text + author-lecture study**, not a full-chapter summary
- **Author:** John Ousterhout
- **Primary original extract:** https://web.stanford.edu/~ouster/cgi-bin/aposd2ndEdExtract.pdf
- **Author lecture (read in full):** https://web.stanford.edu/~ouster/cgi-bin/cs190-winter18/lecture.php?topic=modularDesign
- **Edition check:** https://web.stanford.edu/~ouster/cgi-bin/book.php
- **Thai companion:** [Chapter 6 — Thai](./a-philosophy-of-software-design-ch06-extract.th.md)
- **Evidence boundary:** Chapter 6 opening and §6.1 text surfaced in the official PDF search index; further section titles are identifiable, but **the PDF cannot be opened in full in this session**. The author lecture is readable and explicitly addresses generic classes, editor example, information hiding and specialization. Do not label later sections full original-text reading.

## The author's core idea

**[BOOK EXTRACT — opening/§6.1]** Ousterhout argues that narrowly specialized methods frequently complicate code because they expose application-specific decisions through lower-level module interfaces. Generality can yield *simpler* interfaces and better information hiding. His target is **somewhat general-purpose**, not “implement all conceivable features.” Current requirements constrain functionality; an interface can still be reusable beyond a single current call site.

**[AUTHOR LECTURE]** The author frames the test as: What is the simplest interface satisfying current needs? Could it work in more than one situation? Is it convenient for today's callers? An abstraction should not require callers to learn details it could have hidden.

## Concrete example: text editor

**[AUTHOR LECTURE; discussed in §6.2–6.4 by heading, original section text not fully verified]** Consider editing stored text. A specialized storage interface such as `backspace()`, `deleteKey()` or `cutSelection()` embeds knowledge of UI actions in the text-storage module. A more reusable storage operation such as `deleteRange(start,end)` enables the UI layer to decide which range to remove, while the storage layer knows how to maintain text representation and invariants. This reduces dependencies when adding new editor controls.

**Interpretation:** Separate *policy* (what the user action means) from *mechanism* (how text changes safely). However, `deleteRange` must still specify offsets, invalid-range behavior and undo/transaction guarantees that real users need.

## Where to draw the line

**[AUTHOR LECTURE]** Favor sufficient generality in the **interface**, not unused future functionality. Special-purpose business orchestration may legitimately belong in a higher layer, while reusable mechanisms sit behind lower-level APIs. Prefer revising a problematic abstraction over adding many pass-through wrappers. Too many thin layers create knowledge overhead without hiding any difficult decisions.

**[MY ENGINEER]** For a Merchant Approval feature across Admin Web, BFF and Merchant Service, a generic `transitionRequest(id, targetStatus, actor)` might be reasonable **only if** the service can express the permitted transitions, audit obligations and error semantics correctly. Otherwise, explicit domain operations like `approveRequest` may communicate invariants better. “General-purpose” is not synonymous with generic CRUD or replacing the domain model with `execute(action, payload)`.

## Candidate Principles (not approved)

### AP-C07 — Prefer Somewhat General-Purpose Interfaces, Not Speculative Features

- **Understanding:** Design a small interface that supports current valid use cases and hides details likely to vary.
- **Engineering judgment:** Apply when each new business action demands another low-level method with duplicate logic. Don't add unused plugin points, knobs, framework layers or highly generic `any` payloads just in case.
- **Evidence:** Official second-edition extract opening and §6.1; Stanford lecture “Generic Classes are Deeper.”
- **Overlaps:** AP-C02 Deep Modules, AP-C05 important contracts, EF-G02 preserve changeability. Consider consolidation before promotion.
- **Review trigger:** The interface grows special methods, forces callers to coordinate internal implementation details, or becomes too ambiguous to use safely.

### AP-C08 — Keep Specialized Policy Separate from Reusable Mechanism Where Valuable

- **Understanding:** UI/workflow rules should not automatically leak into a lower-level data mechanism.
- **Engineering judgment:** Useful for editors, parsing, domain orchestration and shared libraries; not an excuse to split everything into multiple services. Avoid pass-through tiers and hidden transaction boundaries.
- **Evidence:** Stanford lecture, editor example and “New Layer, New Abstraction”; official chapter title/section structure only for the later editor sections.
- **Overlaps:** AP-C06 hide incidental detail, EF-G03 compatibility risk.
- **Review trigger:** Changes to UI policy repeatedly force low-level storage edits or new layers add no independent abstraction.

## Decision checklist for Solution Design / Code Review

1. What is the **current set** of valid use cases and contracts?
2. Which details do callers need to know for correctness, error handling and safety?
3. Which decisions are genuinely reusable mechanisms, and which are domain-specific policies?
4. Does a new method simplify more callers than it burdens? Would a concrete operation be clearer?
5. Does this create a deeper module or just another forwarding layer?
6. What evidence would justify a future refactor instead of speculative extensibility now?

## Evidence limitations and future work

Only indexed portions of official Chapter 6 were available; **not a full reading of §§6.2–6.9**. The entire 2018 Stanford modular-design lecture was read, but it cannot substitute for the revised 2021 chapter wording. Independent comparisons and a local prototype remain pending. Both AP-C07 and AP-C08 are **Draft/Candidate**, with no changes to approved EF-G01–39 or workflows.

Sources: [official extract](https://web.stanford.edu/~ouster/cgi-bin/aposd2ndEdExtract.pdf), [author lecture](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter18/lecture.php?topic=modularDesign), [edition notes](https://web.stanford.edu/~ouster/cgi-bin/book.php).
