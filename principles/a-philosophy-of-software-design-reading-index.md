# A Philosophy of Software Design — Reading & Source Index

- **Status:** 🟡 Draft v0.1 — source inventory; no APOSD principles have been approved
- **Author:** John Ousterhout
- **Edition of interest:** Second Edition (2021)
- **Created:** 2026-10-10
- **Thai companion:** [Thai reading index](./a-philosophy-of-software-design-reading-index.th.md)
- **Scope:** Prioritize architecture/system design and code quality/maintainability in My Engineer.


## New second-edition source study (partial Chapter 21)

- **[Decide What Matters — English](./a-philosophy-of-software-design-ch21-extract.md)** · **[Thai](./a-philosophy-of-software-design-ch21-extract.th.md)**
- **Status:** 🟡 Draft — official extract was partially text-indexed, **not downloaded/read in full**.
- **Confirmed coverage:** Chapter 21 opening, partial §21.1, §§21.3–21.4; §21.2 and §21.5 pending.
- **New candidates:** AP-C05–AP-C06 (unapproved), with overlaps flagged for consolidation.
- **Correction to earlier blocker:** While a full PDF reader still failed (13.9 MB), the web search index returned author-hosted original-text excerpts, enough for a **partial source study only**.

## First study completed — Author Talk + Stanford Lecture Notes

- **Study:** [Author Talk & Lecture Evidence Notes (English)](./a-philosophy-of-software-design-author-talk.md) · [Thai](./a-philosophy-of-software-design-author-talk.th.md)
- **Status:** 🟡 Draft — original source-backed thematic analysis, **not** a full-video, full-chapter or second-edition book summary
- **Coverage:** Accessible excerpts from the 2018 video transcript + author-authored CS190 2018/2021 notes + later author clarification.
- **Candidate IDs:** AP-C01–AP-C04 (all pending review). No existing approved principles changed.
- **PDF blocker:** The official extract URL is verified by the author's website, but online PDF fetch (13.9 MB) and independent download failed. Do not mark AP-S01 as read until inspected.

## Scope and evidence policy

This is a **source inventory and reading plan**, not a chapter summary or a claim that the entire book has been read.

| Evidence type | Appropriate claim | Must not claim |
| --- | --- | --- |
| Second-edition original book extract | Accurate summary of the *specific extract pages or chapters actually inspected* | Full access to the whole second edition |
| 2018 Talks at Google by Ousterhout | Author's talk/perspective around the first edition | Verbatim second-edition text or whole-book coverage |
| Stanford CS190 materials by Ousterhout | Author's lectures, demonstrations, and design vocabulary | A full chapter-by-chapter reproduction of the book |
| Author's book page | Edition history, what changed, scope of released extract | The body of unread chapters |
| Third-party notes or transcript mirrors | Finding topics, timestamps and counterarguments; check against primary material | Primary authority or verified exact wording without checking |

Every future note should label claims **[BOOK EXTRACT]**, **[AUTHOR TALK]**, **[AUTHOR LECTURE]**, **[AUTHOR WEBSITE]** or **[MY ENGINEER SYNTHESIS]**. Summaries must cite concrete headings/pages or video timestamps when reviewed. Explicitly label unavailable original chapters rather than invent coverage.

## Source library

| ID | Source | Type / edition | Access verified | What it contributes | Status |
| --- | --- | --- | --- | --- | --- |
| AP-S01 | [Official second-edition extract](https://web.stanford.edu/~ouster/cgi-bin/aposd2ndEdExtract.pdf) | Author-hosted book extract, second edition | Direct PDF link confirmed by author's website; **PDF contents not yet fully inspected** | Changed/new material including *Decide What Matters*, revised Ch. 6, and comparisons with *Clean Code* | 🟡 To read and index pages |
| AP-S02 | [Talks at Google — A Philosophy of Software Design](https://www.youtube.com/watch?v=bmSAYlu0NcY&t=233s) | Author video talk, Aug 1, 2018 (first-edition era) | Official video's title, presenter and description confirmed; **no claim of full first-hand video review here** | Complexity, modularity, abstraction and design philosophy explained by author | 🟡 Watch/transcript-review |
| AP-S03 | [Stanford CS190, Winter 2024 course](https://web.stanford.edu/~ouster/cs190-winter24/) | Author's teaching material | Official course page verified | Author-taught class meetings, design discussions, feedback and exercises | 🟡 Review selected lecture slides |
| AP-S04 | [Stanford CS190, Winter 2021 intro notes](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter21/lecture.php?topic=intro) | Author's lecture notes | Content verified | Complexity framing; design awareness; iteration and code review | 🟡 Study/reference |
| AP-S05 | [Official book page](https://web.stanford.edu/~ouster/cgi-bin/book.php) | Edition metadata | Verified | Second edition published July 2021; new *Decide What Matters*; expanded Ch. 6; *Clean Code* discussion | 🟢 Source metadata verified |

**Why editions matter:** The 2018 talk is excellent primary evidence about the author's design philosophy, but the **2021 second edition includes updated and added material**. The two sources must not be conflated.

**PDF access caveat:** The extract endpoint is a large PDF (~14 MB). The web reader refused to fetch its contents in the current check; the author's site confirms the extract exists. Its internal headings and pages therefore require separate inspection before generating detailed extract summaries.

## Select a Topic (source-driven, not fictional chapters)

| Topic | Starting sources | Expected value | Status |
| --- | --- | --- | --- |
| What complexity means | AP-S02, AP-S04 | Understand change difficulty, cognitive load, dependencies | 🟡 Draft reading |
| Deep modules and information hiding | AP-S02, AP-S03 | Evaluate module boundaries and abstraction cost | 🟡 Draft reading |
| Strategic vs tactical programming | AP-S02, AP-S03 | Balance short-term shipping and long-term changeability | 🟡 Draft reading |
| General-purpose modules and scope | AP-S01, AP-S03 | Read **revised second-edition** arguments on generality | 🟡 Draft reading |
| Decide what matters | AP-S01, AP-S05 | Read the **new second-edition chapter** before summarizing | 🟡 Draft reading |
| Comments, method length and *Clean Code* disagreements | AP-S01, AP-S03 | Compare conflicting design heuristics without assuming one is universally right | 🟡 Draft reading |
| Design review and refactoring judgment | AP-S03, AP-S04 | Test design claims on concrete code and review cycles | 🟡 Draft reading |

## Study workflow for My Engineer

1. Inspect the complete official extract and inventory its **actual pages/sections**. If local user-provided licensed book pages later become available, mark that access separately.
2. Watch or review a trustworthy complete transcript of AP-S02, with timestamps. Keep author speech separate from summaries by other people.
3. Study matching CS190 slides/lecture notes and compare perspectives across 2018/2021/2024.
4. Write topic-focused original notes in English `.md` + Thai `.th.md`, with source anchors and known coverage limits. Do **not** label them full-chapter summaries without chapter text.
5. Extract **Candidate** principles with three layers: Understanding, Engineering Judgment, Evidence & Evolution; include counterexamples and overlap with EF-G01–39.
6. After owner review, accept as **contextual guidance** (never automatically mandate in `AGENTS.md`, CI or workflows).

## Boundary with accepted My Engineer knowledge

The eleven *Software Engineering at Google* chapter studies and EF-G01–39 are an **approved knowledge baseline / contextual guidance**, not universally compulsory policy. All future APOSD notes/principles start **Draft/Candidate**. Compare deep-module decisions with existing changeability, consistency and code review principles before introducing new IDs.

**No new `AGENTS.md` rules, AI skills, or workflow status changes are authorized by this index.**
