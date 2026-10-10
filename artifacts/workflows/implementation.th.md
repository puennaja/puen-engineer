# Implementation / Developer Handoff — Artifact Template (ภาษาไทย)

> **สำหรับ Jira Work Item ที่รับผิดชอบจริง** คัดลอกส่วนนี้ลง Jira/MR ได้ **ไม่บังคับสร้างไฟล์ใหม่** Verification-first เตรียม Acceptance/Test Intent ก่อน Code ได้ แต่ Formal Verification ต้องตามหลัง Developer Checks ที่ผ่านจริงและ Human Implementation Gate ห้าม Mark Approved เอง
>
> Workflow: [Implementation v1.0 (Accepted)](../../workflows/implementation.th.md) · [English](./implementation.md)

- **Work Item (Jira Story / Sub-task / Bug / Task เดิม):** <Link>
- **Parent Feature / Approved Design + Plan:** <Links และ Human Gate Records>
- **สถานะ:** Planned | Implementing | Developer checks passed | Implementation Gate pending | Ready for formal verification | Blocked
- **Assigned Engineer / Authorized Repos:** <Owner, Repos, Exclusions>
- **Risk / Reason:** <Low/Medium/High พร้อม Data/Security/Contract Impact>
- **Branch / Fixed Diff Base:** <Branch/Base Commit จริง อย่าเดา Target Branch>

## 1. Goal, Acceptance และ Boundaries

- **Work Item Outcome / Source-backed Acceptance:** <Scenarios และ Expected Behavior จาก Requirement>
- **In Scope / Out of Scope:** <Services ที่ทำจริง ไม่แตะงาน FE/BE/BFF ของคนอื่น>
- **API/Event/Data Contracts & Migrations ที่กระทบ:** <Changes, Owner, Compatibility>
- **Negative Cases / Domain Invariants:** <Permissions, Idempotency, Concurrency, Validation ตามจริง>
- **Verification-first Input:** <Acceptance Examples/Test Plan ที่เตรียมก่อน Code; Early Feedback ยังไม่ใช่ Formal Verification>

## 2. Working-tree และ Edit Safety

- **ก่อนแก้:** <git status, branch, Dirty/Untracked Files ที่มีอยู่, Allowed Paths>
- **Existing Changes ที่ต้องรักษา:** <ห้ามทับ ถ้าซ้อนกันต้องถาม Human>
- **Editing Boundaries:** <Scoped Edits ไม่มี Unrelated Refactor/Destructive Commands>
- **หลังแก้:** <ตรวจ Diff/Status แยก Changes ของตนกับของเดิม>
- **Files / Commits ที่แก้:** <Paths และ Commit SHAs จริง หรือ N/A>

## 3. Implementation Changes และเหตุผล

| Change / File | ทำไมต้องแก้เพื่อ Acceptance | Contract / Data / Risk Effect | Evidence |
| --- | --- | --- | --- |
| <File/Diff> | <Reason> | <Effect หรือ None ที่ตรวจจริง> | <Link> |

**New Assumptions / Material Scope Changes:** <หาก Design/Planning Decision เดิมใช้ไม่ได้ ให้ย้อน Human Gate ที่กระทบ>

## 4. Developer Checks — บันทึกผลจริงเท่านั้น

| Required? / เหตุผล | Command / Manual Check | Safe Environment / Fixture | Expected vs Observed | Pass / Fail / Not run / Blocked / Inconclusive | Evidence |
| --- | --- | --- | --- | --- | --- |
| <Required/Optional> | <Command จริง> | <Env> | <Result> | <Status> | <CI/Log> |

- **Unit / Regression Tests ที่เกี่ยวข้อง:** <รันอะไร ได้ผลอะไร>
- **Build / Lint / Typecheck / CI ตาม Repo Policy:** <รันอะไรจริง>
- **No Unit-test Seam (ข้อยกเว้น):** <เหตุผล, Alternative Evidence, Human เห็นชอบที่ Implementation Gate; ห้ามอ้าง Pass ปลอม>
- **Checks ที่ไม่ได้รัน/ไม่ผ่าน พร้อม Owner:** <Gap; ถ้า Required ไม่ผ่าน ยังเริ่ม Formal Verification ไม่ได้>

## 5. Human Implementation Gate — ต่อ Work Item

- **Proposed Disposition:** Ready for verification | More Implementation | Blocked (Discovery/Design) | Stop/Defer
- **Review Packet:** <Scope, Diff/Base, Real Checks, Contract Risks, Gaps>
- **Human Gate Owner / Decision / วันที่ / Link:** <หลักฐาน Approve/Reject/Pending จริง>
- **Publish Authorization แยก:** <Work Branch/Push Scope ที่อนุมัติ หรือ None/Pending>
- **Formal Independent Verification Entry:** <Developer Checks ที่เกี่ยวข้องต้องผ่านจริง + Human Implementation Gate>

## 6. MR Publication และ Verification Handoff

- **Draft MR Proposal (ถ้าต้องเปิด):** <Repo, Source, Proposed Target, เหตุผล Target, Jira, MR Dependencies>
- **Human Confirm ทุก Draft MR แยก:** <ผู้อนุมัติ/วันที่/Link หรือ Pending — ไม่เปิดเองก่อน>
- **MR / Commit / Pipeline Links ที่เกิดขึ้นจริง:** <Links>
- **ส่งต่อ Verification:** <Fixed Diff Base, Acceptance Oracle, Real Tests, Negative Cases, Gaps, Dependencies, Release Risks>
- **Rework:** <Finding → Scoped Fix → Rerun Tests ที่กระทบ → Independent Reverification>

**Authority Boundary:** **Human เท่านั้น** Mark Draft MR Ready หรือ Merge; Code/Test ผ่านไม่ได้อนุญาต Deploy ห้ามข้าม GitLab Branch/CI/Reviewer/Security Policies ของ Repo จริง
