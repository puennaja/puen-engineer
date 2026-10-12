# Example: Private Finance — เพิ่ม Feature วิเคราะห์การใช้จ่าย (ภาษาไทย)

> ตัวอย่างการพิมพ์ Prompt กับ Matt Pocock และ pstack สำหรับใช้งานจริงก่อน puen-stack สมบูรณ์


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
