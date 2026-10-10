# A Philosophy of Software Design — วิเคราะห์ Author Talk และ Stanford Lectures (v0.1)

- **สถานะ:** 🟡 Draft / Candidate — ยังไม่อนุมัติ Principles ใหม่
- **ประเภท:** Author Talk + Author Lecture Study Notes **ไม่ใช่ Full-Chapter Summary**
- **ภาษาอังกฤษหลัก:** [English](./a-philosophy-of-software-design-author-talk.md)
- **วันที่:** 2026-10-10
- **ขอบเขตที่ตรวจจริง:** Official Talks at Google metadata, ส่วน Transcript ที่เข้าถึงได้จากแหล่งรองพร้อมเวลา, Stanford CS190 Lecture Notes ปี 2018 และ 2021 และบทสัมภาษณ์ที่ยืนยันข้อควรระวัง
- **ยังไม่สำเร็จ:** ไม่สามารถดาวน์โหลด Official Second Edition Extract เพื่ออ่านภายในได้ และไม่ได้เล่นวิดีโอเต็มแบบผู้ชม ดังนั้นไม่อ้างว่าครบวิดีโอหรือหนังสือ

## หลักฐานและที่มา

- [A1 — Official Talks at Google 2018](https://www.youtube.com/watch?v=bmSAYlu0NcY): ยืนยันผู้บรรยายและยุคฉบับแรก
- [A2 — Transcript พร้อม Timestamps](https://lilys.ai/cn/notes/808736): แหล่งถอดข้อความโดยบุคคลที่สาม ควรย้อนตรวจคลิปก่อนนำคำพูดไปอ้างตรง
- [A3 — Stanford CS190 Winter 2018: Modular Design](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter18/lecture.php?topic=modularDesign): เอกสารจากผู้เขียนโดยตรง
- [A4 — Stanford CS190 Winter 2021: Introduction](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter21/lecture.php?topic=intro): เอกสารจากผู้เขียนโดยตรง
- [A5 — ผู้เขียนยืนยันความต่างของฉบับ 2](https://web.stanford.edu/~ouster/cgi-bin/book.php)
- [A6 — 2025 Interview Transcript](https://podscripts.co/podcasts/the-pragmatic-engineer/the-philosophy-of-software-design-with-john-ousterhout): แหล่งถอดบทสัมภาษณ์ ใช้เป็นหลักฐานเตือนเรื่อง Error Handling

## สรุปและประเมินทีละแนวคิด

### 1. ความซับซ้อนเกิดจาก Dependency ความไม่ชัดเจน และการประนีประนอมเล็ก ๆ ที่สะสม

**คำอธิบายจากแหล่งที่ตรวจ:** ใน Stanford CS190 ผู้เขียนให้มอง Complexity ว่าเป็นสิ่งในโครงสร้างระบบที่ทำให้เข้าใจและเปลี่ยนยาก เช่น ต้องรู้หลาย Module ก่อนแก้เรื่องเดียว หรือหาข้อมูลสำคัญไม่พบ ปัญหาเล็ก ๆ สะสมไปเรื่อย ๆ จน Refactor ครั้งใหญ่แพงมาก

**อ้างอิง:** CS190 Winter 2021, Introduction: Complexity; 2018 talk, strategic/tactical discussion around 34–38 min

**ข้อจำกัด:** ไม่ใช่ทุก Dependency เป็นสิ่งไม่ดี บาง Contract และ Invariant จำเป็น

### 2. Deep Module ซ่อนความซับซ้อนภายใต้ Interface ที่ใช้งานง่าย

**คำอธิบายจากแหล่งที่ตรวจ:** Ousterhout มองความคุ้มค่าของ Module จากสิ่งที่ทำได้เทียบกับความยุ่งยากของ Interface ซึ่งรวม Side Effects และข้อกำหนดการใช้ ไม่ใช่นับแค่จำนวน Parameter ตัวอย่างคือ UNIX File I/O ที่ Interface เรียบง่ายแต่ Implementation ซับซ้อน ส่วน Wrapper บาง ๆ อาจเพิ่มต้นทุนโดยไม่ได้ซ่อนอะไร

**อ้างอิง:** CS190 Winter 2018, Modular Design: Classes Should Be Deep, Information Hiding, New Layer New Abstraction; 2018 talk around 12:34–16:15

**ข้อจำกัด:** Deep ไม่ได้แปลว่าไฟล์ยาว จึงไม่ควรบังคับจำนวนบรรทัดเป็นกฎ และบาง Abstraction เล็ก ๆ ก็สมเหตุสมผล

### 3. ออกแบบให้บาง Error ไม่จำเป็นต้องเกิด แต่ห้ามมองข้าม Error ที่มีอยู่จริง

**คำอธิบายจากแหล่งที่ตรวจ:** แนวคิด Define Errors Out of Existence คือปรับ Contract ให้บางสถานการณ์ไม่เป็น Error อีกต่อไปอย่างสมเหตุสมผล ไม่ใช่ Catch แล้วทิ้ง Ousterhout ย้ำในการสัมภาษณ์ภายหลังว่า Network Fail หรือ Server Crash ยังต้องถูกจัดการจริง

**อ้างอิง:** 2018 Talks at Google: discussion of exceptions and error complexity; Pragmatic Engineer interview 2025 28:03–30:36 (corroboration of author's warning)

**ข้อจำกัด:** ข้อผิดพลาดด้าน Security การเงิน หรือ Network ไม่หายไปเพียงเพราะหยุด Throw Error

### 4. Strategic Design ลงทุนกับการเปลี่ยนระบบในอนาคต ไม่ใช่แค่ส่ง Feature ถัดไป

**คำอธิบายจากแหล่งที่ตรวจ:** การแก้แบบ Tactical ทำให้ของใช้ได้วันนี้ แต่สร้างทางลัดที่สะสมจนแก้ยาก ขณะที่ Strategic ลงทุนกับความสามารถเปลี่ยนในอนาคต วิธีทำไม่ใช่ออกแบบทั้งหมดก่อนเริ่ม แต่เป็นวงจรออกแบบบางส่วน ลงมือ ตรวจ Review แล้วแก้ไขซ้ำ

**อ้างอิง:** Talk approx. 34–44 min; CS190 Winter 2021 Introduction: Role of Design in the Software Development Process

**ข้อจำกัด:** โค้ดใช้ครั้งเดียวหรือการแก้ Incident เร่งด่วนอาจเลือกทางที่เร็วกว่าได้ ต้องดูอายุระบบและความเสี่ยง

## Candidate Principles ที่เสนอ (ยังไม่ Approved)

| ID | Candidate | ความสัมพันธ์กับ Principles เดิม |
| --- | --- | --- |
| AP-C01 | ลด Apparent Complexity ในจุดที่ต้องเปลี่ยน | ใกล้กับ EF-G02; เพิ่มเรื่อง Dependency/Obscurity และ Cognitive Load |
| AP-C02 | ออกแบบ Deep Modules ด้วย Information Hiding | เพิ่มความละเอียดเรื่อง Module Boundary; ไม่ใช่กฎความยาวไฟล์ |
| AP-C03 | จัดการ Error ที่ต้นทางด้วย Contract ที่สมเหตุสมผล | เพิ่มด้าน API & Failure; ไม่ใช่ให้ Ignore Failure |
| AP-C04 | ลงทุนกับ Strategic Design อย่างเป็นสัดส่วน | ใกล้ EF-G01/EF-G06; ควรรวมมากกว่าสร้างกฎซ้ำ |

### Three-layer review (ต้องตรวจเพิ่มเติม)

**AP-C02 — Deep Modules:** Understanding: ให้ Capability ที่มีคุณค่าสูงผ่าน Interface ที่ต้องรู้น้อย; Judgment: ใช้เมื่อ Interface มี Pass-through Layers หรือ Leakage แต่ระวัง God Modules และการรวมความรับผิดชอบที่ไม่เกี่ยวข้อง; Evidence: A2/A3 รองรับผู้เขียนกล่าว แต่ยังไม่มี Independent Evaluation หรือ Pilot ใน My Engineer

**AP-C03 — Error Contract Design:** Understanding: ลดสถานการณ์พิเศษด้วยการออกแบบ Semantic ให้ถูก ไม่ใช่ปิด Error; Judgment: เหมาะกับ Operation ที่เป็น Idempotent หรือการ Normalize Input บางประเภท แต่ไม่ควรปกปิดการผิดเงื่อนไขธุรกิจหรือ Network Failure; Evidence: A2 และ A6; ต้องทดสอบ Failure Modes ก่อนรับรอง

## ตัวอย่างเพื่ออภิปราย (My Engineer สร้างเอง ไม่ใช่ตัวอย่างในหนังสือ)

Service สำหรับการ Import CSV อาจมี `ParseAndValidateStatement(file)` ที่ซ่อนรายละเอียด Encoding, Mapping, Date Parsing และ Validation ไว้หลัง Contract ที่ชัดเจน การแยก Wrapper 5 ชั้นโดยแต่ละชั้นส่ง Argument ต่อเหมือนเดิม อาจทำให้ระบบเป็น Shallow Layers แต่หาก Rule บางอย่างจำเป็นต้องแบ่ง Boundary เพื่อความปลอดภัย ก็อย่ารวมทุกอย่างจนกลายเป็น God Service

## ยังเหลืออะไรต้องอ่าน?

Official PDF Extract ฉบับ 2 ยัง **ไม่ถูกอ่าน** เพราะช่องทางดาวน์โหลดไม่สำเร็จ ต้องรอไฟล์ที่เข้าถึงได้หรือ User Upload ก่อนทำ Book Extract Study; ยังต้องตรวจ Transcript เต็มพร้อม Timecode เพื่อกล่าวว่า Full Talk Reviewed; และเปรียบเทียบกับเอกสารหลักฐานอิสระ เช่น Parnas ด้าน Information Hiding

**ไม่มีการเปลี่ยน Approved Workflows, AGENTS.md หรือ EF-G01–39 จากเอกสารนี้**
