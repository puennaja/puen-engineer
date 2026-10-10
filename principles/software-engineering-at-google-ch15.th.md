# Software Engineering at Google — บทที่ 15: Deprecation

- **สถานะ:** Draft Study Notes v0.1; Candidate Principles **ยังไม่ Approved**
- **ผู้เขียนบท:** Hyrum Wright
- **แหล่งต้นฉบับ:** https://abseil.io/resources/swe-book/html/ch15.html
- **ฉบับหลักภาษาอังกฤษ:** [English](./software-engineering-at-google-ch15.md)
- **วันที่ตรวจสอบ:** 2026-10-10
- **วิธีสรุป:** อ่านบทเผยแพร่จากต้นฉบับ Google และเรียบเรียงใหม่โดยไม่คัดลอกหนังสือ; บทวิเคราะห์และ Candidate คือข้อสังเคราะห์ My Engineer

**ข้อควรระวัง:** กรณีศึกษาองค์กร Google (ปี 2020) ไม่ใช่ข้อบังคับทั่วไป ต้องตรวจสอบการใช้งานในบริบทปัจจุบันอีกครั้ง

## วิเคราะห์ประเด็นสำคัญของบท

### 1. Why removal matters

คุณค่ามาจากความสามารถที่ซอฟต์แวร์ให้ผู้ใช้ ไม่ใช่จำนวนบรรทัด ระบบเก่าที่เลิกจำเป็นแล้วกินค่าดูแลและขวางของใหม่ แต่ความเก่าไม่ได้แปลว่าต้องเลิกใช้

**ข้อจำกัด / Trade-off:** เลิกใช้แบบไม่พร้อมอาจแพงกว่าดูแลต่อ ต้องดูผู้ใช้งานจริงและความพร้อมของระบบใหม่

### 2. Advisory versus compulsory

Advisory เตือนและเสนอทางเลือกแต่คนส่วนใหญ่ไม่ย้ายเอง Compulsory มี Deadline จริง ผู้มีอำนาจตัดสิน และทีมช่วย Migration

**ข้อจำกัด / Trade-off:** Deadline โดยไม่จัดคนและงบ Migration เป็นการผลักภาระและเสี่ยงพัง

### 3. Reveal hidden dependencies and plan gradual exit

ใช้ Code Search, Runtime Logs และการทดลองปิดเป็นช่วงเพื่อหาผู้ใช้แอบแฝง ลดจำนวนผู้ใช้ทีละขั้นและกำหนด Milestone แทนรอจนปิดระบบได้ทีเดียว

**ข้อจำกัด / Trade-off:** ทดลองตัดระบบจริงต้องมีอนุมัติ เกณฑ์หยุด และวิธี Recovery

### 4. Govern deprecation as an owned project

การเลิกใช้ต้องมี Owner, การสื่อสาร, คู่มือย้าย, เครื่องมือและคนช่วย ต้องรู้ผู้ใช้เดิมทั้งหมดและหยุดการเพิ่ม Usage ใหม่ด้วย Warning/Lint

**ข้อจำกัด / Trade-off:** อย่าห้ามใช้ API เก่าถ้ายังไม่มีทางเลือกที่ใช้งานได้จริง

### 5. Plan removal at design time

ออกแบบระบบให้รู้ Owner และ Dependency เพื่อให้ถอนออกได้ภายหลัง การลบสิ่งไม่ใช้ก็เป็นการส่งมอบคุณค่า เพราะคืนทรัพยากรและลด Complexity

**ข้อจำกัด / Trade-off:** ข้อกำหนดการเก็บข้อมูล กฎหมาย และสัญญาอาจไม่อนุญาตให้ลบทันที

## Candidate Principles — ยังไม่ใช่กฎกลาง

### EF-G28 — มองการเลิกใช้เป็นงานวิศวกรรม

- **ชื่ออังกฤษ:** Treat Retirement as Engineering Work
- **ที่มาในบท:** Why Deprecate?; Managing the Deprecation Process
- **Layer A — กลไก/เมื่อควรใช้:** วางแผนค้นผู้ใช้ ช่วยย้าย และ Milestone
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ไม่ควรลบเพราะระบบเก่าเพียงอย่างเดียว
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G29 — ให้ Deadline คู่กับความช่วยเหลือในการย้าย

- **ชื่ออังกฤษ:** Match Deprecation Deadlines with Migration Support
- **ที่มาในบท:** Advisory Deprecation; Compulsory Deprecation
- **Layer A — กลไก/เมื่อควรใช้:** จัดคนและเงิน มี Owner และประกาศวันหมดอายุจริง
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** บังคับตัดโดยไม่มีอำนาจ ข้อมูลและ Recovery เสี่ยงลูกค้า
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G30 — ห้ามเพิ่ม Dependency ใหม่ขณะกำลังเลิกใช้

- **ชื่ออังกฤษ:** Prevent New Dependence During Retirement
- **ที่มาในบท:** Deprecation Tooling; Preventing backsliding
- **Layer A — กลไก/เมื่อควรใช้:** ใช้ Annotation/Warning/Lint ห้ามเพิ่มของใหม่และค่อยลดของเดิม
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ห้ามเร็วเกินไปอาจขวางงานจำเป็น
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

## เชื่อมกับ Workflow ของเรา (ยังเป็นข้อเสนอ)

Technical Discovery: locate consumers; Solution Design: compatibility and exit plans; Delivery Planning: migration staffing and milestones; Release & Operations: controlled retirement and monitoring.

## สิ่งที่ต้องตรวจสอบก่อน Approve

1. เนื้อหาจาก Google สอดคล้องกับบริบท Solo/Team/Multi-repo ของเราแค่ไหน?
2. มีข้อโต้แย้งจากงานวิจัย/แนวปฏิบัติล่าสุดหรือไม่?
3. หลักการซ้ำกับ EF-G01–24 หรือไม่ และควรรวมกันแทนเพิ่มกฎใหม่ไหม?
4. มีงานตัวอย่างที่ไม่เป็นความลับให้ทดลองและวัดผลได้หรือไม่?

**ยังไม่เปลี่ยน Accepted Workflows, Delivery Planning Draft, AGENTS.md หรือสร้าง Skills**
