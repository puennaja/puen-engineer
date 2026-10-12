# For Me — ชุด Skills ใช้ทำงานจริงระหว่างรอ puen-stack (ภาษาไทย)

> 2026-10-12 · Interim personal workflow, **ไม่ใช่ Corporate Policy หรือ Skill Pilot Pass** · Source: [pstack analysis](./pstack/README.th.md) | [Matt analysis](./matt-pocock/README.th.md) | [Accepted FDL](../workflows/feature-delivery-lifecycle.th.md)

## TL;DR

**เลือก Matt Pocock สำหรับ Requirement/Spec/Tickets/TDD/Standards+Spec Review; เลือก pstack สำหรับอ่านโค้ด/Impact/Architecture/หลักฐานว่า behavior ทำงานจริง; ใช้ Git/Tracker ตามนโยบายบริษัทเอง.** อย่าต่อ multiagent สองชุดเป็น pipeline ทุก Task. ถ้า Task เล็ก ใช้ขั้นต่ำที่พิสูจน์ผลได้.

**ก่อนใช้กับงานบริษัท:** ขออนุญาตใช้ AI/เครื่องมือเชื่อมต่อและตรวจ data handling; ห้ามใส่ secrets, customer data, private code หรือ internal design ใน public GitHub `puen-engineer` และเครื่องมือที่บริษัทยังไม่อนุญาต. นโยบายบริษัทมีอำนาจสูงกว่าเอกสารนี้.

## 1. ชุด Skill ที่แนะนำ

| ความสำคัญ | Provider | Skill | ใช้เมื่อ | สิ่งที่ต้องได้ |
|---|---|---|---|---|
| หลัก | Matt | `grill-with-docs` | Jira Story ไม่ชัด, grooming, decision ยังไม่ครบ | Problem, behavior, acceptance, unknowns, glossary/ADR เฉพาะที่ตัดสินแล้ว |
| หลัก | pstack | `how` | ก่อนแก้ legacy หรือไม่เข้าใจ flow | As-is call/data flow, files, contracts, evidence |
| หลัก | pstack | `blast-radius` | Change ดูเล็กแต่กระทบ consumers ได้ | Impacted callers, regression surface, proof |
| หลัก | pstack | `architect` | เพิ่ม behavior ข้าม function/service boundary | caller-first interface, types/data shape, options, trade-offs; **with checkpoint** |
| หลัก | Matt | `to-spec` | Requirement/design ตกลงแล้วและต้องส่งต่อหลาย sessions | Written spec (ไม่ใช่เครื่องมือถาม Requirement) |
| หลัก | Matt | `to-tickets` | งานใหญ่ต้องแบ่งพร้อม dependencies | Vertical slices + ready-to-implement tickets |
| หลัก | Matt | `tdd` | Business behavior มี input/output ทดสอบได้ | failing test → minimal passing code → refactor |
| หลัก | Matt | `code-review` | ต้อง Review against requirement และ repo standards | **แยก Standards verdict / Spec verdict**, explicit findings |
| หลัก | pstack | `create-verification-skill` | ยังไม่มี repeatable app/API verification | runnable verification harness + real evidence; พิสูจน์ harness ก่อนใช้ |
| หลัก | Matt | `pr` | หลัง Verify และเตรียม Draft MR | PR/MR body ที่มี Summary, Evidence, Merge Danger; **ไม่ได้เปิด MR** |
| เสริม | Matt | `diagnosing-bugs` | Bug ซับซ้อน/flake/perf regression | reproduce-minimise-hypothesis-instrument-fix-evidence |
| เสริม | pstack | `why` | ประวัติ/เจตนาการออกแบบสำคัญ | commit/issue/doc reasoning, uncertainty |
| เสริม | pstack | `interrogate` | Diff risk สูง, correctness/security/perf/edge cases | adversarial findings; ไม่ใช่ formal acceptance แทนคน |
| เสริม | Matt | `retro` | AI ทำผิดซ้ำ/หาไฟล์นาน/เสีย context | proposed checks/pointers/standards changes |
| เสริม | Matt | `codebase-design` | อยากพิจารณา module depth, seam, interfaces | Design vocabulary/heuristics; ไม่ execute งาน |
| เสริม | Matt | `wayfinder` | Scope ใหญ่และ route ยังไม่รู้/หลาย sessions | Decision map (ไม่ใช่ implementation tickets) |

Dependencies: `grill-with-docs` ต้องมี `grilling` และ `domain-modeling`; `tdd` ใช้ vocabulary ของ `codebase-design`. การติดตั้งจะต้องตรวจให้ครบตามเวอร์ชัน.

### ไม่ควรเปิดเป็น default

- Matt `implement`: โดยปกติ **commit เอง** และ review diff fixed-point ก่อน commit อาจว่าง. ใช้ได้เฉพาะเมื่อได้รับอนุญาตให้ commit ชัดเจนและตรวจ branch/ref ก่อน หรือใช้ coding agent ธรรมดาพร้อม `tdd` แล้ว review manual.
- Matt `implement-spec`: parallel worktrees, integrate branches และอาจจัดการ ticket/draft PR; ยังไม่คุ้มสำหรับเริ่มที่ทำงาน
- pstack `poteto-mode` full Feature / Opening PR / Shipping: orchestration อาจพาไปเปิด PR หรือ merge เกินสิทธิ์; ใช้ selective skills หรือระบุ stop gate พร้อม tool permissions
- `arena`, `swarm`: เมื่อ risk/coverage justify token จริง ๆ เท่านั้น; อย่ารันเพราะเท่
- pstack `setup-pstack`, `poteto-mode`, `create-verification-skill` มีพฤติกรรม Cursor-specific; **ยังไม่ยืนยันว่า Codex/Claude ใช้เหมือนกัน**
- Matt `pr` ไม่ใช่ create PR/MR; `wayfinder` map ไม่ใช่ build work items

## 2. Feature Workflow ที่ทำตามได้จริง

| ขั้น | ทำอะไร | เรียก Skill | Output / จุดตรวจ |
|---|---|---|---|
| 0 | เปิด Jira/Figma/Repository ที่ถูก, ตรวจ working tree, policies, tools | manual; setup ตาม agent ที่ได้รับอนุญาต | Requirements + branch + baseline |
| 1 Requirement | อ่าน Jira Story; ถาม PO เฉพาะประเด็นที่ยังไม่ชัด | Matt `grill-with-docs` | Problem/acceptance/negative cases/unknowns; **คนยืนยัน** |
| 2 Technical Discovery | Trace Web → BFF → Service แยก repo พร้อม references | pstack `how`, `why` เฉพาะจำเป็น | Current flow/actual contracts/constraints |
| 3 Impact & Design | หา consumers, type/API/data boundary, review alternatives | pstack `blast-radius`, `architect with checkpoint`; Matt `codebase-design` เฉพาะจำเป็น | Solution sketch, trade-offs, verification plan; **คน/ทีม review** |
| 4 Planning | ถ้างานหลาย sessions เขียน spec แล้วแตก Work Items | Matt `to-spec` → `to-tickets` | Spec + vertical slices, dependencies; **คนอนุมัติ** |
| 5 Acceptance first | ระบุ externally observable test seams | Matt `tdd` | Accepted behavior tests; red-green evidence |
| 6 Implement | ให้ Coding Agent แก้ทีละ approved slice, run lint/test/build | **Coding Agent ปกติ + Matt tdd** (ไม่จำเป็นต้องใช้ full implement) | Scoped diff, actual check outputs, no unauthorized commit |
| 7 Verify | รัน app/API/integration จาก user path; assess blast radius | pstack `create-verification-skill` (ทำครั้งแรก), `blast-radius` | Reproducible behavior evidence; Not run/Blocked ต้องแยก |
| 8 Independent Review | reviewer ใหม่ ตรวจ spec/standards; เพิ่ม adversarial เมื่อ risk สูง | Matt `code-review`; pstack `interrogate` optional | Findings/actioned/dismissed; human verification |
| 9 Deliver | ตามบริษัท: scoped commit/branch/push, Draft MR ไป target ที่ตรวจแล้ว | Matt `pr` **แค่เขียน body**; Git tooling ที่ได้รับอนุญาต | Reviewed MR body, evidence and known risk; ห้าม auto merge |
| 10 Outcome | หลัง QA/Deploy: verify behavior, observe, learn | Matt `retro` เมื่อมี friction | Work item updates, follow-up; ไม่อ้างว่า deploy healthy ถ้าไม่ได้ตรวจ |

**ถ้างานเล็กมาก:** Requirement/acceptance → `/how` เฉพาะถ้าไม่รู้ flow → coding agent + test → `/code-review` หรือ reviewer → MR. ไม่ต้องสร้าง spec/tickets/ADR แบบบังคับ. Gate ตามความเสี่ยงและ policy ยังคงมี.

## 3. Playbooks ตามชนิดงาน

### A. Jira Feature ทั่วไป
`grill-with-docs` → `how` → `blast-radius` → `architect with checkpoint` (ข้าม boundary) → `to-spec/to-tickets` (งานใหญ่) → coding agent + `tdd` → runtime verify → `code-review` → `pr` → GitLab Draft MR manual.

### B. Bug Fix
`how` (ถ้าไม่เข้าใจ flow) → Matt `diagnosing-bugs` (สำหรับยาก) → `tdd` regression test → fix → runtime proof → `code-review` → `pr`.
**ต้องมี repro ก่อนเดาสาเหตุ**; bug ธรรมดาที่ reproduce/fix ง่ายไม่ต้อง full diagnosis.

### C. Refactor
`how` → `blast-radius` → baseline tests/behavior → Matt `codebase-design` (ถ้าขยับ boundaries) → focused refactor → verify no behavior change → `code-review`.
`improve-codebase-architecture` เป็น survey หา candidate แยกจากงาน feature ไม่ใช้เป็น gate ทุกครั้ง.

### D. Performance
Measured baseline/trace ก่อน → pstack Perf playbook + `benchmark-checklist` เมื่อ Cursor supports → targeted fix → alternate repeated before/after evidence → review. ห้ามกล่าวอ้างเร็วขึ้นจาก single noisy run.

### E. Multi-repo Feature (Admin Web, Admin BFF, Merchant Service)
Read-only `how` **แยก repo** → consolidate contract/dependency/owners ใน artifact กลางที่ไม่เปิดเผยข้อมูลบริษัท → `blast-radius` ต่อ repo → `architect with checkpoint` ต่อ boundary → `to-tickets` slice ตาม integration order → implement/verify แต่ละ repo → cross-repo end-to-end tests → independent review → separate Draft MRs ตามบริษัท. เปิด VS Code ทีละ repo ไม่ทำให้ Agent อ่านอีกสอง repo อัตโนมัติ.

### F. Review-only
Matt `code-review <base ref>` ให้ Standards/Spec; ถ้าความเสี่ยงสูงใช้ pstack `interrogate` เพิ่ม. ต้องมี approved ticket/spec และตรวจว่าดิฟที่รีวิวครอบคลุม working-tree หรือ committed changes ตามจริง; Matt review ใช้ fixed-ref...HEAD จึงอาจ **มองไม่เห็น uncommitted diff**. ห้ามอ้างว่า reviewed แล้วถ้า diff ว่าง.

## 4. Prompt copy/paste (สำหรับเครื่องมือที่รองรับ skill)

```text
/grill-with-docs
นี่คือ Jira story: <link/text ที่ได้รับอนุญาต>
ช่วยแยก confirmed/assumed/unknown และถามเฉพาะคำถามที่มีผลต่อ behavior,
acceptance, edge cases และ impacted contracts. อย่าเดาคำตอบแทน PO.
หยุดก่อน implementation และอย่าโพสต์ข้อมูลบริษัทออกนอกระบบที่อนุญาต.
```

```text
/how Trace flow ของ <feature> ใน repo นี้แบบ read-only.
คืน call path, key files, request/response shapes, storage, error handling,
tests ที่มีอยู่ และสิ่งที่ยังพิสูจน์ไม่ได้. ห้ามแก้ไฟล์.
```

```text
/architect with checkpoint: Design <approved feature> จาก current code.
เสนอ caller-first types, contracts, edge cases, trade-offs และ test seams.
หยุดหลัง design proposal เพื่อ review กับทีม; ห้าม implement/commit.
```

```text
/to-tickets <approved spec>
เสนอ vertical implementation slices พร้อม dependencies, acceptance,
impacted repos และ verification for each. แสดง draft ก่อนเขียน issue tracker;
อย่าสร้าง/อัปเดต Jira ticket จนได้รับอนุญาต.
```

```text
Implement only <approved subtask> within <repo>.
Apply Matt's tdd discipline where an observable behavior seam exists.
Run focused tests and relevant checks; report commands and results.
Do not commit, push, open MR, merge, or deploy. Stop for my review.
```

```text
/code-review <verified-base-ref>
Review committed diff against <ticket/spec> on two independent axes:
1) standards, 2) spec correctness. Include severity, paths, evidence, tests missing.
Read-only. Explicitly say if uncommitted diff is not covered.
```

```text
/pr
Prepare a concise GitLab Draft MR body using our company template:
Summary, Evidence (exact checks plus runtime proof), Merge Danger,
affected contracts, roll-out/rollback risks, Jira link, known gaps.
Write description only; do not push, open MR, merge, or deploy.
```

## 5. Setup ที่ทำงานแบบปลอดภัย

1. ตรวจว่าองค์กรอนุญาต Claude/Codex/Cursor, cloud models, plugins/MCP ที่จะเข้าถึง private repo/Jira หรือไม่
2. ใช้ **AGENTS.md เป็นกลาง** ตามแนวเรา; setup Matt แบบ per repo ให้ใช้ AGENTS.md เดิม, ตรวจว่าไม่เพิ่ม CLAUDE.md ซ้ำ และตรวจ config `docs/agents/issue-tracker.md` ก่อนเปิด write
3. ใช้ installation จาก README upstream ที่เข้ากับ runtime จริง; Matt รองรับ Codex plugin installation ใน README และ pstack จาก Cursor plugin; **ตรวจ slash commands ที่ติดตั้งจริงก่อนใช้**
4. เริ่มด้วย read-only `how` หรือ `grill-with-docs`; อย่าให้ auto git writes ก่อน review permission
5. เก็บ docs งานบริษัทใน private company-approved destination เท่านั้น; `puen-engineer` public เก็บ generic methodology/fictional examples
6. เปิด unit test, lint, typecheck และ runtime evidence ตาม nature ของ repo; ไม่สมมติว่า test pass
7. หากจะใช้ใน puen-stack pilot ให้บันทึก worked/failed/not-verified และ costs; แต่ไม่บังคับ pilot recorder ในงานบริษัท

## 6. สรุปเป็นกฎเลือกเร็ว

- **Requirement ไม่ชัด** → Matt `grill-with-docs`
- **ไม่เข้าใจโค้ด** → pstack `how`
- **กลัวกระทบจุดอื่น** → pstack `blast-radius`
- **ข้าม service/boundary หรือดีไซน์ยาก** → pstack `architect with checkpoint`
- **ต้องแตก Jira subtasks** → Matt `to-tickets` (ผ่าน tracker permissions)
- **พฤติกรรมทดสอบได้** → Matt `tdd`
- **Bug ยาก** → Matt `diagnosing-bugs`
- **ต้องพิสูจน์ว่าแอปรันจริง** → pstack `create-verification-skill`
- **ต้อง Review ว่าโค้ดตรง requirement และ convention ไหม** → Matt `code-review`
- **High-risk adversarial review** → pstack `interrogate`
- **ต้องเขียน MR description** → Matt `pr`
- **ต้องแก้ recurring AI mistake** → Matt `retro`

**เกณฑ์ย้ายเข้า puen-stack:** ขณะใช้จริงจดว่าทำไม skill ไหนดี/ไม่ดี, token/cost, test/review outcomes และ friction. เขียน custom skills เฉพาะส่วนที่ซ้ำ/ขาด โดยไม่ผูก workflow กับ provider เดียว.

## 7. ตัวอย่างการใช้งานจริง (Private Finance)

ตัวอย่าง Prompt ภาษาไทยสั้น ๆ โดยสมมติ Product `private-finance` มี Architecture `FE → BFF → BE → Database`

- [เพิ่ม Feature วิเคราะห์การใช้จ่าย — ตั้งแต่ Requirement ถึง Draft MR](./for-me-private-finance-example.th.md)
- [Bug Fix — Dashboard สรุปรายจ่ายไม่ตรงรายการ](./for-me-private-finance-bugfix-example.th.md)
- [Refactoring — ลด Logic แปลง Transaction ที่ซ้ำโดยรักษา Behavior](./for-me-private-finance-refactoring-example.th.md)
- [Multi-Repo Feature — FE, BFF, BE และ Database](./for-me-private-finance-multi-repo-example.th.md)

> ทุกตัวอย่างเป็นสถานการณ์สมมติ ใช้งานทีละ Skill ตามความจำเป็น และต้องตรวจหลักฐาน/ขอบเขต Git/สิทธิ์ตามนโยบายของทีม

## 8. Optional — Ponytail สำหรับ Code ที่เล็กแต่ครบและปลอดภัย

**Ponytail ไม่ใช่ Lifecycle Runner**; ให้ใช้เฉพาะตอน Implement หรือ Review เพื่อหลีกเลี่ยง Overengineering โดยยังคง Validation, Security, Error Handling และ Test ที่จำเป็น

- **Implementation:** เริ่ม `ponytail full` หลัง Scope/Design อนุมัติแล้ว ให้ Coding Agent ใช้ของเดิมก่อน ไม่เพิ่ม Abstraction/Dependency โดยไม่มีเหตุผล และทดสอบ logic ที่เสี่ยง เช่น เงิน/Refund/Concurrency
- **Review:** `ponytail-review` ใช้ตรวจ bugs/security/load/callers นอก Diff และโค้ดเกินจำเป็น; **Matt `code-review` ยังเป็นตัวหลัก** สำหรับ Standards และ Spec Compliance. เพิ่ม Ponytail เฉพาะ High Risk/รีวิวก่อน Commit ที่ต้องอ่าน Uncommitted Diff
- **Repo Audit:** `ponytail-audit` ใช้ตรวจ Legacy เฉพาะพื้นที่เสี่ยงเมื่อจำเป็น ไม่ต้องรันทุก Ticket
- **ไม่ต้องใช้ทุกอัน:** `lite/full/ultra` ต่างกัน; `ultra` อาจย้อนถาม Requirement ที่ทีมอนุมัติแล้ว จึงไม่ใช้เป็น Default; `ponytail-gain` เป็น benchmark ของผู้พัฒนา ไม่ใช่ Cost Measurement ของเรา

Prompt ตัวอย่าง: `/ponytail-review ดู diff ที่ยังไม่ commit นี้หน่อย เน้นยอดเงิน timezone refund และ caller ที่อาจพัง ขอแค่ findings ไม่ต้องแก้`

**Codex:** upstream ระบุว่า command อาจอยู่ใต้ namespace `$ponytail:ponytail-review`; ต้องตรวจการติดตั้งและ Hooks จริง และเช็กนโยบายบริษัทก่อนเปิด always-on mode

[อ่าน Analysis + Private Finance Prompt ภาษาไทยของ Ponytail](./ponytail/README.th.md)
