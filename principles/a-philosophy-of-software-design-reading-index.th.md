# A Philosophy of Software Design — สารบัญแหล่งข้อมูลและแผนอ่าน

- **สถานะ:** 🟡 Draft v0.1 — จัดทำรายการแหล่งข้อมูล ยังไม่ได้ Approve หลักการใหม่จาก APOSD
- **ผู้เขียน:** John Ousterhout
- **ฉบับที่ต้องการศึกษา:** Second Edition (2021)
- **จัดทำ:** 2026-10-10
- **ฉบับหลักภาษาอังกฤษ:** [English reading index](./a-philosophy-of-software-design-reading-index.md)
- **เป้าหมาย:** เติม Architecture & System Design กับ Code Quality & Maintainability ใน My Engineer

## ขอบเขตและวิธีอ้างอิง

ไฟล์นี้เป็น **สารบัญแหล่งข้อมูลและแผนอ่าน** ยัง **ไม่ใช่สรุปหนังสือเต็มเล่ม** และไม่อ้างว่าอ่านครบทุกบท

| ประเภทแหล่ง | อ้างอะไรได้ | สิ่งที่ห้ามอ้าง |
| --- | --- | --- |
| Official Second Edition Extract | เนื้อหาหนังสือจาก **หน้าหรือส่วนที่ได้อ่านจริง** | สรุปทั้งเล่มหรือบทที่ยังไม่มีต้นฉบับ |
| Talks at Google ปี 2018 | คำอธิบายแนวคิดจากผู้เขียนในยุคฉบับแรก | ถือเป็นข้อความฉบับปี 2021 โดยอัตโนมัติ |
| Stanford CS190 | Lecture/Slide และตัวอย่างที่ผู้เขียนสอน | เรียกว่าอ่านบทหนังสือครบ |
| เว็บไซต์หนังสือของผู้เขียน | ประวัติฉบับและความเปลี่ยนแปลง | เนื้อหาภายในบทที่ไม่ได้เปิดให้อ่าน |
| บันทึก/Transcript บุคคลที่สาม | ช่วยหาหัวข้อและเวลาเพื่อย้อนตรวจต้นทาง | ยกเป็น Primary Source ทั้งหมด |

สรุปในอนาคตต้องใส่ป้าย **[BOOK EXTRACT]**, **[AUTHOR TALK]**, **[AUTHOR LECTURE]**, **[AUTHOR WEBSITE]** หรือ **[MY ENGINEER SYNTHESIS]** พร้อมหน้าหรือ Timestamp เท่าที่ตรวจได้

## รายการแหล่งข้อมูล

| ID | แหล่ง | ประเภทและยุค | ตรวจสอบแล้ว | ประโยชน์ | สถานะ |
| --- | --- | --- | --- | --- | --- |
| AP-S01 | [Official Second Edition Extract](https://web.stanford.edu/~ouster/cgi-bin/aposd2ndEdExtract.pdf) | เนื้อหาจากหนังสือ ฉบับ 2 | หน้าเว็บผู้เขียนยืนยันว่ามีไฟล์นี้; **ยังไม่ได้อ่านเนื้อหาใน PDF ครบ** | ส่วนใหม่/ส่วนแก้ไข เช่น *Decide What Matters*, Ch.6, เปรียบเทียบ *Clean Code* | 🟡 รอสำรวจหน้าจริง |
| AP-S02 | [Talks at Google — A Philosophy of Software Design](https://www.youtube.com/watch?v=bmSAYlu0NcY&t=233s) | วิดีโอผู้เขียน ปี 2018 | ยืนยันชื่อ ผู้บรรยาย และคำอธิบายวิดีโอ; **ยังไม่ได้ยืนยันว่าได้ดูครบ** | Complexity, Abstraction, Modularity และปรัชญาการออกแบบ | 🟡 รอไล่ Transcript/วิดีโอ |
| AP-S03 | [Stanford CS190 Winter 2024](https://web.stanford.edu/~ouster/cs190-winter24/) | สไลด์และเอกสารการสอน | ตรวจหน้าเว็บรายวิชาแล้ว | ตัวอย่างการ Design, Code Review และการปรับแก้งาน | 🟡 เลือก Lecture มาอ่าน |
| AP-S04 | [Stanford CS190 Winter 2021 — Introduction](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter21/lecture.php?topic=intro) | Lecture Notes ของผู้เขียน | ตรวจเนื้อหาแล้ว | Complexity, Design Awareness, Iteration | 🟡 อ่านอ้างอิง |
| AP-S05 | [Official Book Page](https://web.stanford.edu/~ouster/cgi-bin/book.php) | ข้อมูลฉบับหนังสือ | ตรวจแล้ว | ยืนยัน Second Edition ปี 2021 และส่วนที่เปลี่ยน | 🟢 ตรวจข้อมูลแหล่งแล้ว |

**สำคัญ:** วิดีโอปี 2018 เป็น Primary Source จากผู้เขียนเอง แต่ไม่ใช่หลักฐานว่าเนื้อหาทุกอย่างตรงกับ Second Edition ปี 2021 โดยเฉพาะบทใหม่และบทที่เขาปรับแก้

**ข้อจำกัด PDF:** ตัว Extract ขนาดประมาณ 14 MB ทำให้ตัวอ่านเว็บในรอบนี้ดึงเนื้อหามาไม่ได้ แต่หน้าเว็บผู้เขียนยืนยันแหล่งนี้ จึงต้องตรวจหน้า/หัวข้อจริงก่อนทำสรุปลงลึก

## เลือกหัวข้อที่จะอ่าน — ไม่แอบอ้างว่าเป็นบทเต็ม

| หัวข้อ | แหล่งเริ่มต้น | ต้องการเรียนรู้อะไร | สถานะ |
| --- | --- | --- | --- |
| Complexity | AP-S02, AP-S04 | เหตุใดระบบจึงเข้าใจและเปลี่ยนยาก | 🟡 Draft |
| Deep Modules / Information Hiding | AP-S02, AP-S03 | Boundary และต้นทุน Abstraction | 🟡 Draft |
| Strategic vs Tactical Programming | AP-S02, AP-S03 | สมดุลความเร็ววันนี้และความง่ายในการเปลี่ยนวันหน้า | 🟡 Draft |
| General-Purpose Modules | AP-S01, AP-S03 | อ่านเหตุผล **ฉบับ 2 ที่แก้ไขแล้ว** เรื่อง Generality | 🟡 Draft |
| Decide What Matters | AP-S01, AP-S05 | อ่าน **บทใหม่ในฉบับ 2** ก่อนสรุป | 🟡 Draft |
| Comments, Method Length และ *Clean Code* | AP-S01, AP-S03 | เปรียบเทียบแนวคิดที่ขัดกันโดยไม่ถือว่าใครถูกทั้งหมด | 🟡 Draft |
| Design Review / Refactoring Judgment | AP-S03, AP-S04 | ตรวจความเหมาะสมของ Design จากงานและตัวอย่างจริง | 🟡 Draft |

## วิธีทำงานต่อ

1. เปิด Extract ที่ผู้เขียนเผยแพร่ ตรวจสารบัญย่อยและหน้าในไฟล์ก่อน แล้วสรุปเฉพาะสิ่งที่อ่านจริง
2. อ่าน Transcript หรือดู Talks at Google ให้ครบและจด Timestamp
3. ตรวจเทียบ Stanford Lectures กับส่วนที่ต่างกันระหว่างฉบับแรกและฉบับ 2
4. เขียนสรุปแยกหัวข้อ **อังกฤษ .md + ไทย .th.md** บอกชัดว่าแหล่งเป็น Book Extract หรือ Lecture ไม่เรียกว่า Full Chapter ถ้าอ่านไม่ครบ
5. สกัด **Candidate Principles** ที่มี Understanding, Engineering Judgment, Evidence & Evolution พร้อม Counterexamples และตรวจว่าซ้ำกับ EF-G01–39 หรือไม่
6. ให้มึง Review ก่อน Approve เป็นแนวทาง Engineering Judgment ไม่ใช่สร้างกฎบังคับใน `AGENTS.md` ทันที

## สถานะเทียบกับ My Engineer

สรุป *Software Engineering at Google* 11 บท และ EF-G01–39 เป็น **Knowledge Baseline ที่อนุมัติแล้วสำหรับใช้ประกอบ Judgment** ส่วนเอกสารและหลักการของ *A Philosophy of Software Design* ที่จะสร้างต่อจากนี้ยังเป็น Draft/Candidate ทั้งหมด

**ไฟล์นี้ไม่ได้เปลี่ยนสถานะ Workflow, AGENTS.md หรือสร้าง Skills ใด**
