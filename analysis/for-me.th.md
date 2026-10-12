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

## 7. ตัวอย่างพิมพ์จริง: เพิ่ม Feature วิเคราะห์การใช้จ่ายใน private-finance

**สมมติ:** มี Product `private-finance` ใช้งานอยู่แล้ว; Architecture คือ `FE → BFF → BE → Database`. ตัวอย่างนี้เป็นข้อมูลสมมติ ไม่ได้อ้างว่าเคยอ่านโค้ดจริง หรือว่ามี API/Table ใดอยู่แล้ว

**วิธีใช้:** พิมพ์ทีละช่วง ไม่ต้องเอาทุกคำสั่งไปรวมเป็น Prompt เดียว; ตรวจผลลัพธ์และคุยกับทีมก่อนขยับขั้นเมื่อมี Design/Contract ที่ต้องตัดสินใจ. ชื่อ Skill ตามต้นฉบับ; ถ้า agent ที่ใช้ไม่รองรับคำสั่ง ให้ใช้ Skill ที่ติดตั้งและยืนยันการทำงานได้จริง

### 1) Requirement ยังไม่ชัด — Matt `/grill-with-docs`

```text
/grill-with-docs มี private-finance อยู่แล้ว อยากเพิ่มหน้าวิเคราะห์การใช้จ่าย
เช่น แยกตามหมวดหมู่และรายเดือน ช่วยถามสิ่งที่ต้องรู้ก่อนเริ่มทำหน่อย
```

**คาดหวัง:** คำถามเรื่องเป้าหมาย, ขอบเขต, การจัดหมวด, ช่วงเวลา, ข้อมูลที่ขาด, Acceptance; อาจมี Glossary/ADR เมื่อเราตกลงกันแล้ว. **เรา/PO ตัดสินใจ** ไม่ให้ AI สมมติแทน

### 2) เข้าใจของเดิม — pstack `/how` (ทำแยกตาม Repo ที่เปิด)

```text
/how ช่วยไล่ flow รายการใช้จ่ายใน repo นี้ให้หน่อย ว่ารับข้อมูลจากไหน
เรียกอะไรต่อ แล้วมีไฟล์ไหนเกี่ยวข้องบ้าง ขอแค่อ่าน ยังไม่แก้โค้ด
```

**คาดหวัง:** ใน FE เห็นหน้าจอ/การเรียก BFF, ใน BFF เห็นการ map/request BE, ใน BE เห็น business logic กับ query DB; แต่ละ repo ต้องตรวจเอง ไม่สมมติว่า Agent มองอีก repo ได้

### 3) หาจุดกระทบ — pstack `/blast-radius`

```text
/blast-radius ถ้าเพิ่ม API สรุปรายจ่ายรายเดือน จะกระทบ flow เดิม
หรือ consumer ไหนบ้าง ช่วยเช็กจากโค้ดก่อน ยังไม่ต้องแก้
```

**คาดหวัง:** caller, shared types, query, tests, risk และสิ่งที่ต้องทดสอบซ้ำ

### 4) Design — pstack `/architect` แบบหยุดก่อนลงมือ

```text
/architect with checkpoint ช่วยออกแบบ feature วิเคราะห์รายจ่าย
บน FE → BFF → BE → DB ให้หน่อย เน้น API contract, data shape,
filter เดือน/หมวดหมู่ และ edge cases ขอรีวิวแบบก่อน ยังไม่ต้องเขียนโค้ด
```

**คาดหวัง:** options, API request/response, data boundary, aggregation strategy, timezone/currency/refund/empty state, trade-offs และ test approach. **คน/ทีม review** ก่อน implement; ไม่ถือว่า design ผ่านเพียงเพราะ AI เขียนออกมา

### 5) สรุป Spec (ถ้างานใหญ่) — Matt `/to-spec`

```text
/to-spec จากที่คุยและตกลงกันแล้ว ช่วยสรุป spec ของ feature วิเคราะห์รายจ่าย
แยก acceptance, ข้อจำกัด และ contract ที่ต้องรักษาไว้ด้วย
```

**คาดหวัง:** Spec จาก decision ที่ตกลงแล้ว ไม่ใช่เดาคำตอบใหม่; skill อาจเผยแพร่ไป tracker จึงต้องตรวจ config/permission ก่อน

### 6) แตกงาน — Matt `/to-tickets`

```text
/to-tickets ช่วยแตก feature นี้เป็นงานย่อยที่ทำและทดสอบจบทีละชิ้น
บอก dependency ของ FE, BFF, BE, DB ด้วย เสนอรายการก่อน ยังไม่สร้าง Jira
```

**คาดหวัง:** Work items ขนาดพอทำได้, acceptance และ dependencies; อย่าบังคับแยกหนึ่ง ticket ต่อ layer ถ้าจะแบ่งเป็น vertical slice ที่ทดสอบได้ดีกว่า. ตรวจชื่อ/สิทธิ์ tracker ก่อนสร้างจริง

### 7) Implement ทีละชิ้น — Coding Agent ปกติ + Matt `/tdd`

```text
/tdd เริ่มจาก test สำหรับสรุปยอดใช้จ่ายรายเดือนตามหมวดหมู่ก่อน
ช่วยเลือกจุดทดสอบที่เหมาะ แล้วค่อยเขียนโค้ดให้ผ่าน test
```

จากนั้นใน Repo และ Work Item ที่อนุมัติ:

```text
ทำ subtask นี้ตาม spec ที่ตกลงไว้ ช่วยแก้เฉพาะ repo นี้
รัน test กับ build แล้วสรุป diff ให้ดู ยังไม่ต้อง commit หรือ push
```

**คาดหวัง:** Test ที่เห็น Red → Green, Code Diff เฉพาะ Scope และคำสั่ง/ผลการทดสอบจริง; `/tdd` เป็น discipline ไม่ใช่ guarantee ว่าจะ implement ทั้ง feature ให้จบ. อย่าเรียก Matt `/implement` ตรง ๆ ถ้ายังไม่อนุญาต Commit เพราะต้นฉบับอาจ Commit เอง

### 8) Verify behavior จริง — pstack `/create-verification-skill` (เฉพาะถ้ายังไม่มี harness)

```text
/create-verification-skill ช่วยทำวิธีทดสอบ flow วิเคราะห์รายจ่าย
จากหน้าเว็บจนได้ยอดสรุปจริง พร้อมหลักฐานว่าผลตรงกับข้อมูลทดสอบ
```

**คาดหวัง:** วิธี launch/drive/evidence/cleanup ที่รันได้จริง; ถ้า app มี E2E/integration suite ดีอยู่แล้ว ใช้ของเดิม ไม่ต้อง generate ใหม่. เปรียบเทียบ known fixture เช่น ยอดรายเดือนและหมวดหมู่, zero transactions และข้อมูลหลายเดือน; **ห้ามใช้ข้อมูลการเงินจริงในเครื่องมือที่ไม่ได้รับอนุญาต**

### 9) Review — Matt `/code-review` (เพิ่ม pstack `/interrogate` เมื่อเสี่ยงสูง)

```text
/code-review <base-commit>
ช่วยรีวิว branch นี้เทียบกับ spec วิเคราะห์รายจ่ายด้วย
ดูทั้ง coding standards และความถูกต้องของ requirement
```

ถ้าเสี่ยงเรื่องยอดเงิน, การคำนวณ หรือ Data Integrity มาก:

```text
/interrogate ช่วยจับผิดเรื่องการรวมยอด, timezone, refund
และผลกระทบต่อข้อมูลเดิม เน้นปัญหาจริง ไม่ต้องจับ nitpick
```

**คาดหวัง:** Findings ที่ชี้ไฟล์/เหตุผล/ความรุนแรง พร้อมผลทดสอบประกอบ; Matt `/code-review` อาจไม่เห็น uncommitted changes (`base...HEAD`) ให้ตรวจ diff scope ก่อนอ้างว่า review ครบ

### 10) เตรียม Draft MR — Matt `/pr` + GitLab ตามสิทธิ์จริง

```text
/pr ช่วยเขียน description สำหรับ Draft MR ของ feature วิเคราะห์รายจ่าย
ใส่ Jira, สิ่งที่เปลี่ยน, หลักฐาน test และจุดเสี่ยง ขอแค่ข้อความ ไม่ต้อง push หรือเปิด MR
```

**คาดหวัง:** MR Body ที่สั้นและตรวจได้. `/pr` **ไม่สร้าง MR**; commit, push, draft MR, merge, deploy แยกตามสิทธิ์และขั้นตอนของทีม

### ทางลัดให้จำ

```text
Requirement ไม่ชัด   → Matt /grill-with-docs
อ่านโค้ดของเดิม       → pstack /how
เช็กผลกระทบ          → pstack /blast-radius
ออกแบบก่อนแก้         → pstack /architect with checkpoint
ต้องมี spec/tickets   → Matt /to-spec → /to-tickets
ลงมือเขียน             → Coding Agent + Matt /tdd
พิสูจน์ระบบจริง        → App/API tests หรือ pstack /create-verification-skill
ตรวจ diff             → Matt /code-review (+ pstack /interrogate ถ้าเสี่ยง)
เขียน Draft MR        → Matt /pr (ข้อความเท่านั้น)
```

**หมายเหตุ:** ตัวอย่างนี้ตั้งใจให้สั้นและเหมือนพิมพ์คุยกับ AI จริง; ไม่ต้องเรียกทุกขั้นสำหรับงานเล็ก และอย่าข้าม approval/verification ที่บริษัทกำหนด
