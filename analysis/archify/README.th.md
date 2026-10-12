# วิเคราะห์ Archify สำหรับ My Engineer (ภาษาไทย)

> 12 ตุลาคม 2026 · Source: [tt-a1i/archify](https://github.com/tt-a1i/archify) / [SKILL.md](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md) / [repository authoring](https://github.com/tt-a1i/archify/blob/main/archify/references/repository-authoring.md) · เป็นการวิเคราะห์เอกสาร ไม่ใช่ผลทดสอบ CLI จริง

## 1. Archify คืออะไร — และไม่ใช่อะไร

Archify เป็น **Skill เดียวเพื่อสร้าง Interactive Diagrams** จากคำบรรยาย, Mermaid หรือ Repository ที่ Agent อ่านจริง โดยแปลงเป็น typed JSON แล้วสร้าง standalone HTML + SVG ที่สำรวจเส้นทาง เปิดรายละเอียดและส่งต่อได้ ไม่ใช่ code-writing skill, requirement interviewer, architect ที่ตัดสิน design แทนทีม, code reviewer, หรือ runtime verifier

รูปแบบหลัก 5 อย่าง:
- **architecture:** components, services, storage, trust boundaries
- **sequence:** request/response, order, async calls
- **dataflow:** sources, transformations, consumers, data lineage
- **workflow:** development steps, approvals, error branches
- **lifecycle:** states, events, retries, terminal outcomes

Pipeline จาก SKILL.md: scope + repository evidence (เมื่อใช้ codebase จริง) → typed `candidate.json` → `finalize` → validate/deliver/check/browser-check → HTML + machine receipt. การ validate schema และ browser rendering **ไม่ใช่การยืนยัน semantic correctness หรือ live behavior**. ต้องประเมิน source evidence และให้ human review.

## 2. Mapping กับ My Engineer Workflows

| Workflow | Archify ช่วยอะไร | รูปแบบ + Output | ขอบเขต |
|---|---|---|---|
| Requirement Discovery | ทำภาพ User Journey/Approval ให้ PO เห็นภาพ | workflow/lifecycle, illustrative HTML | ไม่ถาม Requirement/อนุมัติ Acceptance แทน Matt `grill-with-docs` |
| Technical Discovery | ภาพ As-Is Architecture และ Sequence ที่อ้างอิงโค้ด | architecture + sequence พร้อม source links | **เหมาะมาก** แต่ต้องอ่าน Repo จริงก่อน; ไม่แทน pstack `how` |
| Solution Design | ภาพ To-Be / Alternatives สำหรับ Design Review | architecture/dataflow/sequence | **เหมาะมาก** เมื่อสรุป design จาก pstack `architect`; diagram เป็น proposal ไม่ใช่ approved design |
| Delivery Planning | Visualize dependencies และ Gate | workflow | ใช้เมื่ออธิบายแผนยาก; ไม่แทน Matt `to-tickets` หรือ Jira |
| Implementation | เปิดดู contract/path ระหว่างแก้ไข | sequence/dataflow | ไม่ใช่ implementation tool |
| Verification | ใช้ภาพประกอบ expected behavior และ coverage | sequence/lifecycle | ไม่รัน Product E2E tests; checks ของ Archify ตรวจ artifact เท่านั้น |
| Review & Integration | เปรียบเทียบ Before/After authored architecture ด้วย Architecture Delta | architecture comparison | ไม่อนุมาน blast radius/merge safety; ต้องใช้ code review และ tests จริง |
| Release & Operational Readiness | วาด deploy path, rollback, failure state | workflow/lifecycle | ไม่ทำ deploy/release approval |
| Outcome & Learning | Document As-Built หลังการเปลี่ยนแปลง | architecture/dataflow | อัปเดตตาม source revision จริง; ไม่ใช่ observed business outcome |

## 3. ใช้ร่วมกับ Matt และ pstack ยังไง

- **Matt** รับ Requirement/Spec/Tickets, TDD, Code Review, PR text
- **pstack** อ่านโค้ด (`how`, `why`), วิเคราะห์ผลกระทบ (`blast-radius`), เสนอ design (`architect with checkpoint`), ตรวจ diff (`interrogate` high-risk)
- **Archify** ทำให้หลักฐานและ Design **สื่อสารเป็น Interactive Diagram** เข้าใจ/รีวิวร่วมกันได้
- **My Engineer / บริษัท** ยังเป็นเจ้าของ Approval, Jira/Plane, Commit/Push, MR, Deploy, Outcome

ให้ใช้ Archify หลัง `how` เพื่อทำ As-Is; หลัง `architect` เพื่อทำ To-Be; หรือหลังการพัฒนาเพื่อเปรียบเทียบ As-Built. ไม่ต้องเรียกทุก Task

## 4. ตัวอย่าง Prompt สั้น ๆ — Private Finance (FE → BFF → BE → Database)

> สมมติว่า FE/BFF/BE เป็น 3 Repos; แยกอ่านตาม Repo ที่เปิดอยู่จริง. ข้อมูลด้านล่างเป็นตัวอย่าง ไม่ได้ยืนยัน Endpoint, DB Schema หรือ Internal Infrastructure ใด ๆ

### A. As-Is Architecture หลังใช้ pstack /how

```text
ใช้ archify ทำ Architecture Diagram จากโค้ดใน repo นี้หน่อย
เน้นส่วนที่เกี่ยวกับรายการใช้จ่าย ขอลิงก์ไฟล์ที่เป็นหลักฐานด้วย
ยังไม่ต้องแก้โค้ด
```

**ได้:** Interactive HTML ของส่วนที่ Agent ตรวจสอบจริง; ถ้าเปิดเฉพาะ BFF ก็อย่าแต่งส่วน FE/BE ที่มองไม่เห็น. แยก unknowns ให้ชัด

### B. Request Sequence ของ Transaction List

```text
ใช้ archify วาด Sequence ของการเปิดหน้ารายการใช้จ่าย
ไล่ FE → BFF → BE → DB เอาเฉพาะ flow ที่พิสูจน์จากโค้ดได้
```

**ได้:** Diagram ลำดับคำขอ/การตอบกลับและ error paths ที่มีหลักฐาน; Repo อื่นที่ไม่ได้อ่านต้องแสดงเป็น assumption หรือ unknown

### C. To-Be Analysis Feature หลัง pstack /architect

```text
ใช้ archify วาด To-Be ของหน้า Dashboard วิเคราะห์รายจ่าย
จาก design ที่เราตกลงกัน ช่วยแยกส่วนใหม่กับส่วนเดิมให้เห็นชัด
ยังไม่ต้อง implement
```

**ได้:** Design proposal ที่ทีมเปิดดูได้; design ต้องได้รับอนุมัติแยก

### D. Data Flow คำนวณยอด

```text
ใช้ archify วาด Data Flow ของยอดรายจ่ายรายเดือน
แสดงจุด filter, aggregate, currency และ refund ตาม spec ที่มี
อะไรไม่รู้ให้เขียนว่า unknown
```

**ได้:** ภาพแสดงการเคลื่อน/แปลงข้อมูล; ไม่ใช่หลักฐานว่าคำนวณยอดถูก ต้อง verify fixtures จริง

### E. วาดขั้นตอนทีม / Gate

```text
ใช้ archify วาด Workflow ของ feature นี้จาก Jira → Design Review
→ Implement → QA → Draft MR พร้อมจุดที่ต้องให้คนอนุมัติ
```

**ได้:** Workflow visualization ไม่ใช่ระบบ automate gates

### F. PR Design Delta

```text
ใช้ archify เปรียบเทียบ Architecture ก่อนและหลัง feature นี้
เอาเฉพาะข้อเท็จจริงที่มี source evidence และบอกส่วนที่ยัง verify ไม่ได้
```

**ได้:** Diagram/Delta ของ authored facts; อย่าใช้เป็นการรับรอง impact และ safety

## 5. ขั้นตอนติดตั้งและใช้งาน

ตาม README ต้นฉบับ:

```bash
npx skills add tt-a1i/archify -g
```

ถ้าจะทดลองกับ Codex แบบไม่ติดตั้งถาวร README เสนอ:

```bash
npx skills use tt-a1i/archify@archify --agent codex
```

การเรียกเป็น Prompt ภาษาธรรมดาโดยระบุ **Use archify** (หรือ `archify` เมื่อ Agent โหลด Skill ไว้จริง). คู่มือบอกว่าทำงานกับ Cursor, Claude Code, Codex และ OpenCode แต่ **เรายังไม่ได้ทดสอบ runtime จริงในเครื่องผู้ใช้**. ตรวจ policies ของบริษัทและ permissions ก่อนติดตั้ง global

ผลลัพธ์ทั่วไปอยู่ใต้ `.archify/<type>-<slug>-<timestamp>/` พร้อม `candidate.json`, HTML และหลักฐาน validation; filename อาจต่างกันตาม request. จาก SKILL.md โหมด repository-backed ต้อง pin Git revision และ source line ที่อ่านจริง; dirty working tree ไม่ถือเป็น committed source ณ HEAD

## 6. สิ่งที่ต้องระวัง

1. Diagram ที่อ่านจากคำบรรยายไม่ใช่ source-backed fact. ถ้าคำบรรยายมี assumptions, output ก็เป็น proposal
2. การตรวจ `finalize` ผ่านเป็น **artifact integrity** (validate/render/browser check) ไม่ใช่ architecture design approval หรือ runtime behavior proof
3. Repository-backed claims ต้องมี file references และ pin revision; แยก Unknowns/Assumptions ไม่แต่ง service/call/DB
4. `Upstream/Downstream` ของ viewer เป็น **authored graph reachability**, ไม่ใช่ pstack `blast-radius` หรือ production impact analysis
5. Architecture Delta เปรียบเทียบ facts ที่ผู้เขียนระบุ ไม่ตีความ merge safety และไม่รับรอง migration risks
6. **Multi-repo:** อ่านหลักฐานแยกทุก repo/commit ก่อนรวม cross-service view; Git revision ของ repo เดียวไม่รับรองอีก repo
7. **Company security:** local HTML/JSON อาจบรรจุชื่อระบบ ภาพ topology, source paths, source links, secrets ที่คัดลอกผิด. เก็บใน private approved location เท่านั้น; ไม่ commit ลง public `puen-engineer`
8. README ระบุ optional update checker เรียก stable manifest เป็นบางครั้ง; ตั้ง `ARCHIFY_UPDATE_CHECK_DISABLED=1` เพื่อปิด optional networking หาก policy ต้องการ และประเมิน dependency/supply-chain ก่อนรัน NPM package
9. Output diagram เก็บเป็น source-backed snapshot ที่อาจล้าสมัย; หลังแก้ code ต้อง re-check source revision ก่อนเผยแพร่
10. ไม่จำเป็นสำหรับ Task เล็กที่ text/call-tree สั้น ๆ ก็สื่อสารได้. เพิ่ม diagram เฉพาะที่ลด rework / ทำ review ง่ายขึ้น

## 7. Recommendation

**เพิ่ม Archify เป็น Optional Skill สำหรับ Technical Discovery + Solution Design**, และ Documentation เฉพาะที่ต้องการแผนภาพ. ไม่ต้องแทรกใน core happy path ทุก Task, ไม่ใช้แทน pstack /how, /architect, Matt /to-spec, /code-review, test และ approval.

ก่อนย้ายเข้าสู่ `puen-stack` ทดลองกับ private-finance สมมติหรือ Repo ทดสอบจริง: วัดเวลาทำความเข้าใจ flow, ความถูกต้องของ source references, false claims, ความง่ายในการ review, artifacts ที่สร้าง, runtime/cost และ permission compliance. เก็บผลใน `pilot-projects/docs` ไม่อ้างว่าผ่านถ้ายังไม่ได้รัน

## References
- [Archify README](https://github.com/tt-a1i/archify/blob/main/README.md)
- [Archify SKILL.md](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)
- [Repository authoring](https://github.com/tt-a1i/archify/blob/main/archify/references/repository-authoring.md)
- [Architecture Delta](https://github.com/tt-a1i/archify/blob/main/README.md#choose-the-right-diagram)
- [For Me interim skills](../for-me.th.md)
