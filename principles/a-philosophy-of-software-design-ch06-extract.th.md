# A Philosophy of Software Design ฉบับ 2 — บทที่ 6: General-Purpose Modules are Deeper

- **สถานะ:** 🟡 Draft v0.1 — **อ่านต้นฉบับได้บางส่วนและอ่าน Lecture ผู้เขียน** ยังไม่ใช่สรุปเต็มบท
- **ผู้เขียน:** John Ousterhout
- **Extract จากผู้เขียน:** https://web.stanford.edu/~ouster/cgi-bin/aposd2ndEdExtract.pdf
- **Stanford Lecture (อ่านครบ):** https://web.stanford.edu/~ouster/cgi-bin/cs190-winter18/lecture.php?topic=modularDesign
- **ฉบับภาษาอังกฤษหลัก:** [English](./a-philosophy-of-software-design-ch06-extract.md)
- **ขอบเขตหลักฐาน:** ระบบค้นหาแสดงข้อความเปิด Chapter 6 และ §6.1 ของฉบับ 2; ยังเปิด PDF ทั้งไฟล์ไม่ได้ เนื้อหาหัวข้อถัดไปใช้ Stanford Lecture ของผู้เขียนประกอบและไม่อ้างว่าอ่านต้นฉบับทุกหัวข้อครบ

## บทนี้กำลังสอนอะไร?

**[BOOK EXTRACT]** ผู้เขียนบอกว่าการสร้าง Method ที่เฉพาะเจาะจงกับทุก Use Case อาจทำให้ระบบซับซ้อนขึ้น เพราะทำให้ Module ชั้นล่างต้องรู้เรื่องของชั้นบนมากเกินไป ทางเลือกคือสร้าง Interface ที่ใช้กับงานหลายรูปแบบได้ โดยที่ยังเรียบง่าย

แต่หัวใจคือ **Somewhat General-Purpose** ไม่ใช่ General จนเกินเหตุ

- **Functionality:** สร้างเท่าที่ต้องใช้จริงวันนี้
- **Interface:** ออกแบบให้ไม่ผูกกับ Use Case เดียวจนเกินไป
- **Goal:** คนใช้ไม่ต้องรู้ Implementation และไม่ต้องเรียนรู้ Method ใหม่ทุกครั้งที่โจทย์เปลี่ยน

## ตัวอย่างจากผู้เขียน: Text Editor

**[AUTHOR LECTURE]** สมมติ Text Storage เปิด Method เป็น `backspace()`, `deleteKey()`, `cutSelection()` ทุกครั้งที่ UI เพิ่มปุ่มใหม่ Storage ต้องเพิ่ม Method ตาม เพราะชั้น Storage รู้จักการกระทำของ UI มากเกินไป

ถ้าแยกให้ Storage มี `deleteRange(start,end)` จะเกิดผลที่ชัดขึ้น: UI เป็นคนตัดสินว่าต้องลบช่วงไหน ส่วน Storage รับผิดชอบแก้ไขข้อความและรักษา Invariants

นี่คือการแยก **Policy** ออกจาก **Mechanism** โดยยังต้องบอก Contract เช่น ตำแหน่งที่ไม่ถูกต้อง และพฤติกรรม Undo ให้ชัด

หมายเหตุ: หัวข้อ Editor อยู่ในสารบัญย่อยของ Chapter 6 แต่ตัวอย่างที่สรุปนี้ตรวจจาก Lecture โดยตรง ไม่ใช่อ้างว่าอ่าน §§6.2–6.4 ใน PDF ครบ

## แล้ว General มากไปจะผิดไหม?

ผิดได้เหมือนกันว่ะ ถ้าเราสร้าง Plugin System, Generic Factory, Configuration Options หรือ Framework ที่ยังไม่มีใครใช้ นั่นอาจเพิ่ม Complexity มากกว่าช่วย

Ousterhout เสนอให้ **General ที่ Interface เท่าที่เหมาะ** ส่วนพฤติกรรมที่ต้อง Implement ตอนนี้ก็ทำตามความต้องการจริง และอย่าเพิ่มชั้น Wrapper ที่ส่งต่อ Argument อย่างเดียวโดยไม่ได้สร้าง Abstraction ใหม่

## ตัวอย่าง My Engineer (กูยกเอง ไม่ใช่ตัวอย่างหนังสือ)

สำหรับ Merchant Approval ถ้าเรามี `transitionRequest(id, status, actor)` มันอาจ Reusable กว่า `approveRequest()` กับ `rejectRequest()`

แต่ **ไม่ใช่ว่า Generic Method ดีกว่าเสมอ** ถ้า Approve และ Reject มีสิทธิ์ บันทึก Audit และ Invariant ต่างกันมาก การแยก Operation ชัดเจนอาจปลอดภัยและอ่านเข้าใจง่ายกว่า

คำถามจึงไม่ใช่ “Generic กว่าไหม?” แต่คือ **“ซ่อน Complexity โดยไม่ซ่อน Business Contract สำคัญหรือเปล่า?”**

## Candidate Principles ใหม่ (ยังไม่ Approve)

### AP-C07 — ออกแบบ Interface ให้ General พอเหมาะ ไม่เพิ่ม Feature ที่ยังไม่ต้องใช้

- **Understanding:** Interface เรียบง่ายและรองรับ Use Case ปัจจุบันได้หลายแบบโดยซ่อนรายละเอียดที่เปลี่ยนง่าย
- **Judgment:** ใช้เมื่อเราต้องเพิ่ม Method เฉพาะทางซ้ำ ๆ ใน Module ชั้นล่าง แต่หลีกเลี่ยง Generic Payload / Extension Point ที่ไม่มี Requirement
- **Evidence:** เปิดบทและ §6.1 จาก Extract ฉบับสอง; Stanford Lecture เรื่อง Generic Classes
- **Overlap:** AP-C02, AP-C05, EF-G02 — ตรวจว่าจะรวม Principle เดิมได้ไหม
- **ทบทวนเมื่อ:** Interface เริ่มมี Method เฉพาะซ้ำ ๆ หรือ Caller ต้องรู้เรื่องภายในมากขึ้น

### AP-C08 — แยก Specialized Policy จาก Reusable Mechanism ตามความจำเป็น

- **Understanding:** ชั้น UI/Workflow เป็นคนรู้ความหมายของ Use Case ส่วน Storage/Mechanism ควรรู้รายละเอียดเฉพาะของตัวเอง
- **Judgment:** มีประโยชน์กับ Parser, Editor, Domain Orchestration และ Shared Library แต่ไม่ใช่เหตุผลให้เพิ่ม Layer เสมอ ต้องระวัง Transaction Boundary และ Wrapper ที่ไม่มีคุณค่า
- **Evidence:** Stanford Lecture หัวข้อ Text Editor และ New Layer New Abstraction
- **Overlap:** AP-C06 และ EF-G03
- **ทบทวนเมื่อ:** เปลี่ยน UI แล้วต้องแก้ Storage ทุกครั้ง หรือเพิ่ม Layer แต่ไม่มีความรู้อะไรถูกซ่อนเพิ่ม

## คำถามที่ใช้กับ Solution Design / Code Review ได้ทันที

1. Use Case ที่ต้องรองรับตอนนี้มีอะไรบ้าง?
2. Caller จำเป็นต้องรู้อะไรเพื่อใช้ API อย่างถูกต้อง?
3. อะไรเป็น Mechanism ที่ใช้ซ้ำได้ และอะไรเป็น Business Policy?
4. Generic API ทำให้ใช้ง่ายขึ้นจริง หรือทำให้พฤติกรรมสำคัญคลุมเครือ?
5. Module นี้ Deep ขึ้น หรือแค่มี Pass-through Layer เพิ่ม?
6. มีหลักฐานอะไรให้เรากลับมา Refactor แทนการเดาอนาคตตอนนี้?

## ข้อจำกัดและขั้นต่อไป

**ยังไม่ได้อ่าน Chapter 6 ฉบับ 2 ครบทั้ง §§6.2–6.9** เพราะเครื่องมือไม่สามารถเปิด PDF ทั้งไฟล์ Stanford Lecture ปี 2018 ให้ความเข้าใจเพิ่มได้แต่ไม่แทนคำเขียนปี 2021 ต้องตรวจเพิ่มและหา Independent Evidence ก่อน Approve AP-C07/08

ไม่มีการเปลี่ยน Workflow, AGENTS.md หรือ Google SWE Principles ที่รับรองไปก่อนหน้า
