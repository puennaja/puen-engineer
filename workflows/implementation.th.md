# Implementation Workflow — v1.0 Release Candidate (ภาษาไทย)

- **สถานะ:** Review Ready / Release Candidate — รอ Owner Approve; ยังไม่ Accepted
- **Decision:** แยก Implementation และ Verification เป็นสอง Workflow โดยใช้ Execution Loop ร่วมกัน (2026-10-10)
- **วันที่:** 2026-10-10
- **Track:** A — Build My Engineer
- **ก่อนหน้า:** [Delivery Planning v1.0](./delivery-planning.th.md), [Solution Design v1.0](./solution-design.th.md)
- **Workflow คู่กัน:** [Verification](./verification.th.md)
- **ต้นฉบับอังกฤษ:** [Implementation](./implementation.md)
- **รองรับ:** Solo ถึง Cross-team; AI เป็นตัวเลือก ไม่ใช่ข้อบังคับ

## เป้าหมายและขอบเขต

นำ Increment ที่ตกลงและเข้าใจพอแล้วมาสร้างเป็น **โค้ดที่ Review ได้** พร้อม Developer Checks และหลักฐานส่งต่อให้ Verification

**Implementation กับ Verification แยกความรับผิดชอบ แต่ไม่ใช่ Waterfall**: ทำวนสั้น ๆ ได้ เขียน Test หรือขอ Review ได้ตั้งแต่ระหว่าง Implement ไม่ต้องรอเขียนโค้ดทั้ง Feature จบ

**Inputs:** Acceptance Scenarios, Increment ใน Delivery Plan, Design Decisions, Repo/Contract ที่สำรวจแล้ว, Risks, Coding Conventions, สิทธิ์ที่ได้รับอนุญาต

**หยุดและกลับไป Discovery/Design/Planning** ถ้าข้อสมมติที่ยังไม่พิสูจน์อาจทำให้เกิด Data Loss, Authorization ผิด, Contract เข้ากันไม่ได้หรือ Business Behavior ผิด การทำ Spike แบบจำกัดขอบเขตยังนับเป็นงานที่เหมาะสม

## Execution Loop ร่วมกัน — ข้อตกลงการส่งต่องาน (เสนอสำหรับ v1.0)

Workflow สองตัว **แยกหน้าที่** แต่ใช้ **วงจรทำงานร่วมกันเดียว** ต่อ Increment:

```text
Delivery Planning (Increment ที่ตกลง)
   ↓
Implementation: สำรวจ → แก้โค้ด → Developer Checks
   ⇄ Verification: Acceptance / Tests / Contracts / Independent Review
   ↳ Findings → แก้โค้ด → ตรวจส่วนที่ได้รับผลกระทบซ้ำ
   ↓
Verification Decision → Release Review (Workflow อื่น)
```

**เริ่ม Verify ได้ทันทีที่มีสิ่งให้ตรวจ:** Acceptance Example, Test Strategy, API/Contract หรือ Partial Diff ก็เริ่มได้แล้ว ไม่ต้องรอ Code ทั้ง Feature จบ หรือรอป้าย Ready for Verification แบบบังคับ Developer Tests ยังคงอยู่ใน Implementation ส่วนการประเมินอย่างอิสระและการตัดสินคุณภาพหลักฐานเป็นหน้าที่ Verification

**ส่งต่อผ่าน Jira/MR เดิมเพียงชุดเดียว** ไม่บังคับสร้างเอกสารใหม่:
- **Identity / Scope:** Increment, Acceptance IDs, Repo/Diff Links, Exclusions
- **Changes / Risks:** Boundary ที่เปลี่ยน, Contracts/Data/Migration และ Failure Cases
- **Evidence:** Checks ที่รันจริงพร้อมผล/Environment, สิ่งที่ **Not run / Blocked**
- **Feedback:** Findings พร้อม Severity/หลักฐาน/Owner, ผลแก้และตรวจซ้ำ
- **Decision:** ทำต่อ / พร้อมตรวจเพิ่ม / Fix & Reverify / Blocked / Verified for Release Review โดยมนุษย์รับผิดชอบ Residual Risk

**กติกาวนซ้ำ:** แก้จุดไหนให้ Reverify จุดนั้นและ Dependencies ที่ได้รับผลกระทบ แต่ถ้าผลกระทบขยายต้องตรวจเพิ่ม ถ้าเจอ Unknown สำคัญย้อนกลับ Discovery/Design/Planning ทั้งสอง Workflow **ไม่มีอำนาจ Deploy** หรือแก้ Acceptance เงียบ ๆ

**Roles:** งาน Solo ทำได้ทั้งสองหน้าที่ แต่ควรแยก Self-check กับ Review Pass งาน Medium/High-risk ควรมี Reviewer ที่เป็นอิสระจริงเมื่อทำได้หรือ Local Policy กำหนด AI สองตัวเห็นตรงกันไม่ได้แปลว่า Test ผ่านหรือ Human อนุมัติ

**Scale:** งานเล็ก Reversible บันทึกใน MR สั้น ๆ ได้ งาน Cross-repo/Finance/Security ต้องมีหลักฐาน Contract, Tests, Review และ Release Risks มากขึ้น โดยไม่ต้องสร้าง Ticket ซ้ำ

## Execution Loop ร่วมกับ Verification

Implementation กับ [Verification](./verification.th.md) มี **หน้าที่แยกกัน แต่ทำงานวนซ้ำร่วมกัน** ไม่ใช่ Waterfall ที่ต้องเขียนโค้ดทั้งหมดให้เสร็จก่อนตรวจ

```text
Delivery Planning → Implementation (Code + Developer Checks)
                     ↕
                 Verification (Acceptance + Independent Review)
                     ↓ Findings → แก้ไข → ตรวจซ้ำ
                     ↓ Evidence เพียงพอ → Release Review (แยก)
```

Verification เริ่มได้ตั้งแต่มี Acceptance Cases, API Contract, Test Strategy หรือ Partial Diff ไม่จำเป็นต้องรอ Feature เสร็จ ใช้ **Jira/MR เดิมเป็น Shared Handoff** โดยระบุ:

- Increment, Acceptance, Repo และ Diff Links
- Contracts/Data Changes, Critical Invariants, Failure Risks
- Commands ที่รันจริง, Environment, ผลลัพธ์ และรายการ Not run
- Findings, Severity, หลักฐาน, Owner, การแก้ไขและ Retest
- Decision โดยมนุษย์: ทำต่อ / Fix & Reverify / Blocked / Ready for Release Review

ทำ Verification ซ้ำเฉพาะส่วนที่ได้รับผลกระทบและ Dependencies ที่เกี่ยวข้อง แต่ขยายเมื่อ Impact มากขึ้น สำหรับงาน Solo สามารถแยก Self-check กับ Review Pass ได้ งานเสี่ยงสูงต้องพิจารณา Reviewer อิสระตาม Policy; AI เห็นตรงกันไม่เท่ากับมนุษย์อนุมัติ ทั้งสอง Workflow ไม่มีอำนาจ Deploy

## กิจกรรม (ทำซ้ำได้)

| กิจกรรม | ทำอะไร | หลักฐานขั้นต่ำ |
| --- | --- | --- |
| 1. Orient | อ่าน Codebase, AGENTS.md (ถ้ามี), Tests, Requirement/Design และ Git Working Tree | เข้าใจพฤติกรรมเดิมและ Files ที่มีหลักฐานว่ากระทบ |
| 2. Bound | ระบุ Increment, Acceptance, Non-goals และ Integration Boundary | Task/Plan Link และ Change Intent |
| 3. Plan local edits | เลือกการแก้ที่เล็กและสอดคล้อง Architecture ระบุ Test/Migration/Error | แนวทางแก้และ Unknown สำคัญ |
| 4. Implement | แก้โค้ดอย่างมีจุดประสงค์ แยก Refactor ที่ไม่เกี่ยวข้อง และรักษา Security/Compatibility | Diff ที่อ่าน Review ได้ |
| 5. Developer checks | รัน Test/Lint/Typecheck/Build ที่เกี่ยวข้องและเพิ่ม Regression Tests | คำสั่งที่รันจริง ผลและสิ่งที่ไม่ได้รัน |
| 6. Handoff / Iterate | แก้ Findings ภายใน สรุป Contract, Risks, Unknowns ให้ Verification | Diff/Acceptance/Test Evidence/Remaining Gaps |

**ไม่ต้องรอถึงขั้นท้ายค่อย Test** Developer Checks เป็น Feedback ภายใน Implementation แต่ Independent Verification อยู่ใน Workflow แยก และสามารถเรียกตรวจระหว่างทำได้

## ปรับแนวทาง Implementation จาก pstack + grill-me (เสนอสำหรับ v1.0)

ส่วนนี้ **ยืมหลักคิด ไม่ได้บังคับให้ใช้ Plugin หรือ Slash Command** อ้างอิง [pstack Guide](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/README.md), [Understand](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/03-understand.md), [Design](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/04-design.md), [Build](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/05-build-and-clean.md) และ [grilling ของ mattpocock](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md) โดยถือว่า **Workflow เป็นหลัก ส่วน Skill เป็นเครื่องมือช่วยทำงาน**

### 1. ก่อน AI แก้โค้ด ต้องมี Goal Contract

ข้อมูลที่ต้องมีหรืออ้างจาก Jira/Plan เดิมคือ **Goal**, **Done Check ที่ผ่าน/ไม่ผ่านได้**, **Proof ที่ต้องเห็น**, **Known Facts/Code References** และ **Constraints** เช่น Read-only หรือหยุดรอ Decision Owner ถ้าข้อมูลอยู่ใน Issue แล้วไม่ต้องเขียน Spec ซ้ำทุก Prompt ถ้า Requirement คลุมเครือให้ AI สรุปความเข้าใจก่อนแก้โค้ด

### 2. เข้าใจโค้ดจาก Evidence และถามเฉพาะ Decision สำคัญ

เริ่มจาก Read-only Investigation ว่าโค้ดทำงานอย่างไร (**how**) แล้วดูเหตุผลที่ออกแบบไว้ (**why**) เมื่อกำลังเปลี่ยน Boundary, Ownership หรือ Contract แยก Fact, Inference, Assumption และ Unknown ห้ามถือว่าทฤษฎีเรื่อง Root Cause เป็นข้อเท็จจริงโดยไม่มี Reproduction

นำแนวคิด `grill-me` มาใช้เฉพาะ **การตัดสินใจที่คลุมเครือหรือเปลี่ยนยาก**: ไล่ Decision Tree ตาม Dependencies, เสนอตัวเลือกพร้อมเหตุผลให้ Human ตัดสิน แต่ข้อเท็จจริงที่หาได้จาก Codebase ให้ AI ไปอ่านเอง ไม่ถาม Engineer เกินจำเป็น **Solution Design ที่ Approved อยู่แล้วเป็นเจ้าของ Design Decision**; การ Grill ไม่ใช่การ Approve Design โดยอัตโนมัติ

### 3. เลือก Execution Path ให้เหมาะกับชนิดงาน

| ประเภท | สิ่งที่ควรทำ | เมื่อไรต้องกลับไปค้นหาเพิ่ม |
| --- | --- | --- |
| **Bug** | Reproduce ด้วย Surface ที่ใกล้ผู้ใช้จริงที่สุด → หา Root Cause → แก้เล็กเท่าที่ Evidence รองรับ → รัน Repro ซ้ำ | ถ้ายัง Reproduce ไม่ได้ให้แจ้ง **Inconclusive** ไม่เดา Fix |
| **Feature** | เริ่มจาก Acceptance/Contracts/Data Shape → Implement ทีละ Vertical Slice ที่ Verify ได้ | ถ้า Boundary หรือ Data Shape เสี่ยง ให้กลับ Solution Design และพิจารณา Options/Prototypes |
| **Refactor** | เก็บผล Behavior เดิม → เปลี่ยนโครงสร้าง → พิสูจน์ Behavior เท่าเดิม | ถ้าผลลัพธ์หรือ Contract เปลี่ยนโดยไม่ตั้งใจ ให้หยุดทบทวน |
| **Performance** | วัด Baseline และ Bottleneck ก่อนแก้ แล้ว Compare โดยเงื่อนไขใกล้กัน | ถ้าตัวเลขวัดไม่น่าเชื่อถือ ให้ตรวจ Harness ก่อนอ้างว่าเร็วขึ้น |

ทำ **Change → Developer Check → Inspect → Adjust** แบบสั้น ๆ จะเลือก Test-first, Test-after, Real CLI/API/UI หรือ Experiment ก็ได้ตาม Risk ไม่บังคับ TDD ทุกกรณี

### 4. เตรียม Diff ให้ Review ได้

ตัด Unrelated Edits, Dead Compatibility Code และ Defensive Code ที่ไม่มีหลักฐานรองรับเมื่อปลอดภัย แต่ **ไม่เอากฎลบ Comments ทั้งหมดของ pstack มาใช้ตรง ๆ**: Comments ที่อธิบาย Invariant, External Constraints และ API Contract ที่จำเป็นยังมีคุณค่า เก็บ Decisions/Evidence ใน Jira/MR เดิม

**Candidate Skills ในอนาคต (ยังไม่สร้าง):** Task Router (Goal/Done/Evidence → Procedure), Codebase Grounding, Implement by Slice และ Decision Interview ไม่ Copy `/poteto-mode`, `/architect`, `/grill-me` ไปเป็น Chain บังคับใน `AGENTS.md` ต้อง Pilot เทียบ No-skill Baseline ทั้งด้าน Quality, Overhead และ Token Cost ก่อน

## แนวทางตาม Risk

- รักษาพฤติกรรมนอก Scope; ระวัง Compatibility และ Side Effects
- ลด Complexity ไม่ใช่เพียงลดจำนวนบรรทัด ใช้ Pattern เดิมถ้าไม่มีหลักฐานให้เปลี่ยน
- เลือก Test-first / Test-after / แบบผสมตาม Risk ไม่บังคับ TDD หรือ Coverage %
- พิจารณา Migration, Retry, Idempotency, Permission, Concurrency, Observability เมื่อเกี่ยวข้อง
- บอกว่า Test ผ่านต่อเมื่อรันเห็นผลจริง ถ้ารันไม่ได้ให้ระบุ **Not run / Blocked**
- ห้ามเผย Secrets และข้อมูลลับบริษัทใน Public Repo หรือ AI Tool ที่ไม่ได้รับอนุญาต
- ใช้ Jira/MR เป็น Source of Truth โดยไม่สร้าง Document ซ้ำถ้าไม่จำเป็น

## การทำงานหลาย Repos

หนึ่ง Feature-level Work Item เชื่อม Task แยก Repo กำหนด Contract Owner และ Integration Checkpoint จากข้อมูลจริง **Agent ที่เปิด Repo เดียวไม่ได้แปลว่าเห็น Repo อื่น** ต้องยืนยัน Producer/Consumer Contract และลำดับ Deploy ตาม Compatibility ไม่เดา

## AI ช่วยได้อย่างไร (ยังเป็น Candidate)

AI เสนอ Code, Tests, Explanation ได้ภายใต้สิทธิ์ที่อนุญาต Engineer รับผิดชอบการเปลี่ยน Architecture, Destructive Operations, การ Run Command ที่มีสิทธิ์พิเศษ, Commit/Push/MR ตาม Policy ท้องถิ่น ยังไม่ได้บังคับว่าต้องใช้ Claude หรือ Codex คู่กัน และไม่สร้าง Skill ก่อน Pilot

## GitLab Draft MR — Human Confirmation ทุกครั้ง (ข้อตกลงสำหรับ Review v1.0)

หลังผ่าน **Implementation Gate** แล้ว AI ต้อง **ตรวจ Git Flow ของ Repo และงานนั้นก่อน** ทั้ง Contribution/Branching Rules, Jira/Work Item, Source Branch, Proposed Target Branch, Release/Integration Flow และ Dependency ของ MR ที่เกี่ยวข้อง ห้ามสมมติว่าต้อง Target `master` เสมอ

**ก่อนสร้าง Draft MR ทุกครั้ง AI ต้องแสดง Repo + Source → Target Branch + เหตุผล + Jira Link แล้วถาม Human เพื่อยืนยัน MR นั้นโดยเฉพาะ** แม้เคยอนุมัติ Implementation Gate หรือมี Scoped Git Permission แล้วก็ตาม ห้ามเปิดก่อนมีคำตอบ การแก้ Target Branch ภายหลังก็ต้องยืนยันใหม่

Commit/Push Permissions ยังเป็นคำถามใน Grill-me รอบถัดไป และ **การอนุมัติให้เปิด MR ไม่ใช่สิทธิ์ Merge/Deploy** อ้างอิง [AI-assisted feature delivery — Draft](./ai-assisted-feature-delivery.md)

## ส่งอะไรให้ Verification

1. Acceptance Cases และ Repo/Boundary ที่เปลี่ยน
2. Diff และ Contract/Schema Changes
3. Tests ที่รันจริง ผลและ Environment รวมทั้ง Tests ที่ไม่ได้รัน
4. Failure/Permission Scenarios
5. Remaining Unknowns และ Release/Rollback Concerns

**ผลลัพธ์:** Ready for Verification / Needs More Implementation / Blocked — Discovery/Design / Stop-Defer

**Ready for Verification ไม่ใช่สิทธิ์ Deploy**

## Checklist ก่อนอนุมัติ — v1.0 Release Candidate

1. Implementation กับ Verification แยก Responsibility ชัด และไม่สร้าง Waterfall หรือบังคับ Handoff ทุกครั้งหรือไม่?
2. Shared Loop และหลักฐาน Jira/MR เพียงชุดเดียวเหมาะกับ Solo และ Multi-repo หรือไม่?
3. เกณฑ์ Independent Review และผล Verify เพียงพอตาม Risk หรือไม่?
4. ขอบเขต AI Permissions ยังรอออกแบบแยกจาก Workflow อย่างเหมาะสมหรือไม่?

## Checklist ก่อนอนุมัติ v1.0

1. แยกความรับผิดชอบชัด โดยไม่ต้อง Handoff ทุกครั้งที่แก้ Code
2. เก็บ Evidence/Findings ไว้ใน Jira/MR ชุดเดียว
3. Review และ Test Evidence เข้มตาม Risk
4. AI Permissions และ Skills รอแยกออกแบบต่อ

## ทดลองและคำถามที่ยังเปิด

ลอง Tiny Regression, Medium Feature และ Simulated Cross-repo Contract Change; วัด First Feedback, Rework, ความตรงไปตรงมาของ Test Evidence และ Overhead

**รอ Review:** AI แก้/Commit ได้ภายใต้ขอบเขต Permission แบบไหน? Handoff ขั้นต่ำใน Jira/GitLab เท่าไร? งานใหญ่ควรเริ่ม Independent Review เมื่อไร?

**Release Candidate รอ Owner Approve** ยังไม่เปลี่ยน AGENTS.md, Skills, Automation หรือ Permissions
