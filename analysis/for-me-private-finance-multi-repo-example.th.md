# Example: Private Finance — Feature วิเคราะห์รายจ่ายข้ามหลาย Repos

> ตัวอย่างสมมติใน Product `private-finance` (FE → BFF → BE → Database) ไม่ใช่ข้อเท็จจริงจาก Codebase จริง ใช้เฉพาะ Agent/เครื่องมือที่บริษัทอนุญาต

**วิธีใช้:** พิมพ์ทีละ Prompt ใน Repo ที่เกี่ยวข้อง ตรวจ Output ก่อนขั้นถัดไป และเก็บข้อมูลบริษัทไว้เฉพาะระบบที่ได้รับอนุญาต อย่า Commit/Push/Merge/Deploy อัตโนมัติ

**โจทย์:** เพิ่มหน้าดูรายจ่ายรายเดือนแยก Category ใน Product เดิม โดยสมมติแยก Repos เป็น `private-finance-fe`, `private-finance-bff`, `private-finance-be`; Database อยู่หลัง BE. **ชื่อ Repos เป็นตัวอย่าง ไม่ใช่ชื่อจริง**

### 1) เคลียร์ Requirement — Matt `/grill-with-docs`
```text
/grill-with-docs อยากเพิ่มสรุปรายจ่ายรายเดือนแยกหมวดหมู่
ช่วยถาม requirement และ edge cases ที่ต้องตกลงก่อนทำหน่อย
```
**คาดหวัง:** Outcome, Acceptance, filter/date/timezone, refund, currency, empty/permission states, non-goals; ยืนยันกับ PO

### 2) สำรวจแต่ละ Repo — pstack `/how`
เปิด Agent ในแต่ละ Repo ที่ได้รับอนุญาต **ทีละ Repo**:

**FE**
```text
/how หน้า transaction เดิมเรียก BFF อย่างไร มี component และ state ไหนเกี่ยวบ้าง
อ่านอย่างเดียว
```
**BFF**
```text
/how route transaction เดิมรับ request/map response ไป BE อย่างไร
อ่านอย่างเดียว
```
**BE/DB**
```text
/how BE ดึง transaction จาก Database ยังไง ตอนนี้มี aggregation หรือ index ไหน
อ่านอย่างเดียว
```
**คาดหวัง:** หลักฐานแยก Repo (file path, flow, contract, unknown); นำผลมารวมเอง ห้ามอ้างว่า Agent ใน Repo เดียวเห็นอีกสอง Repo

### 3) กำหนด Contract และ Impact — pstack `/blast-radius` + `/architect`
```text
/blast-radius ถ้าเพิ่ม endpoint สรุปรายจ่าย จะกระทบ consumer หรือ query เดิมไหนบ้าง
```
```text
/architect with checkpoint ช่วยออกแบบ contract FE-BFF-BE สำหรับสรุปรายจ่าย
เลือก data shape, filter และ error cases ให้ชัด ขอ design ก่อน ยังไม่ implement
```
**คาดหวัง:** API shape, ownership, data semantics, migration/index risks, dependency order; **คน/ทีม review contract** ก่อนแก้

### 4) Spec + แบ่ง Slice — Matt `/to-spec` → `/to-tickets`
```text
/to-spec ช่วยสรุป requirement และ contract ที่ทีมตกลงแล้วเป็น spec
```
```text
/to-tickets ช่วยแบ่ง feature เป็น slices ที่ demo ได้ พร้อม dependency
และระบุ repo ที่ต้องแก้ เสนอ tickets ก่อน ยังไม่ต้องสร้าง Jira
```
**คาดหวัง:** Contract เป็นจุดอ้างอิงเดียว และ tickets พร้อม acceptance; ไม่จำเป็นต้องแบ่งตาม FE/BFF/BE ล้วน ๆ ถ้าไม่ใช่ vertical slices

### 5) Implement ตาม Dependency — Coding Agent + Matt `/tdd`
```text
/tdd ช่วยวาง test case ของ API รวมยอดตามหมวดหมู่ตาม contract ก่อน
```
```text
ช่วยทำเฉพาะ subtask ที่อนุมัติใน repo นี้ตาม contract เดิม
รัน test/build แล้วสรุป diff ยังไม่ commit หรือ push
```
**คาดหวัง:** Developer checks ต่อ Repo, API contract compatibility และ integration proof เมื่อประกอบหลาย Repos; ลำดับการ merge/deploy ต้องพิจารณา backward compatibility

### 6) Integration Test + Review — Matt `/code-review`, pstack `/interrogate` เฉพาะ Risk สูง
```text
ช่วยรัน flow จาก FE ผ่าน BFF และ BE ด้วยข้อมูลทดสอบที่รู้ยอดล่วงหน้า
สรุป expected/actual และบอกส่วนที่ยังทดสอบไม่ได้
```
```text
/code-review <base-ref> ตรวจ diff repo นี้เทียบกับ spec และ coding standards
```
ถ้าเปลี่ยน Data Contract/Finance Calculation สำคัญ:
```text
/interrogate ช่วยจับผิด rounding, timezone, duplicate transactions,
refund และ compatibility ของ API จาก diff ที่เกิดขึ้นจริง
```
**คาดหวัง:** ผลตรวจแต่ละ Repo รวมกับ End-to-End evidence; reviewer ต้องรู้ว่าตรวจ Repo ไหน ไม่อ้างว่า verification สมบูรณ์ถ้าทดสอบ integration ไม่ได้

### 7) เตรียม Draft MRs — Matt `/pr`
```text
/pr เขียน Draft MR ของ repo นี้ พร้อม spec/Jira link, dependent MRs,
ผล test และ rollout risk ขอแค่ข้อความ ไม่ต้องเปิด MR
```
**คาดหวัง:** MR แยก Repo, links/dependencies, safety/rollback. Push, review, merge และ deploy ตามทีมอนุมัติเป็นราย Repo

**จำง่าย:** `grill-with-docs → how (per repo) → blast-radius/architect → to-spec/to-tickets → coding+tdd → integration/code-review → pr (per repo)`
