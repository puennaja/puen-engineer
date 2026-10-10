# Software Engineering at Google — บทที่ 21: Dependency Management

- **สถานะ:** Draft Study Notes v0.1; Candidate Principles **ยังไม่ Approved**
- **ผู้เขียนบท:** Titus Winters
- **แหล่งต้นฉบับ:** https://abseil.io/resources/swe-book/html/ch21.html
- **ฉบับหลักภาษาอังกฤษ:** [English](./software-engineering-at-google-ch21.md)
- **วันที่ตรวจสอบ:** 2026-10-10
- **วิธีสรุป:** อ่านบทเผยแพร่จากต้นฉบับ Google และเรียบเรียงใหม่โดยไม่คัดลอกหนังสือ; บทวิเคราะห์และ Candidate คือข้อสังเคราะห์ My Engineer

**ข้อควรระวัง:** กรณีศึกษาองค์กร Google (ปี 2020) ไม่ใช่ข้อบังคับทั่วไป ต้องตรวจสอบการใช้งานในบริบทปัจจุบันอีกครั้ง

## วิเคราะห์ประเด็นสำคัญของบท

### 1. Dependencies form a changing graph

Dependency ไม่ได้มีแค่หนึ่ง Package แต่เป็นกราฟที่มี Transitive Dependencies ผู้ดูแลคนละองค์กรเปลี่ยน Version อิสระ ทำให้ Diamond Dependency Conflict เกิดได้

**ข้อจำกัด / Trade-off:** Solver หาเวอร์ชันที่ตรง Constraint ได้ไม่ได้แปลว่าปลอดภัยหรือทำงานจริง

### 2. Versioning is a model, not a guarantee

SemVer และ Minimum Version Selection พยายามบอก Compatibility แต่ผู้ใช้ยังพึ่งพาพฤติกรรมที่ไม่ได้สัญญา Version Pinning, Time และ Diamond Dependency เกี่ยวกัน

**ข้อจำกัด / Trade-off:** ไม่ใช่ว่า Pin หรือ Unpin ทุกอย่างคือคำตอบเดียว

### 3. Bundled versions versus Live at Head

Bundled Distribution ให้ Distributor ตรวจชุด Version ที่เข้ากัน ส่วน Live at Head ของ Google ใช้รุ่นปัจจุบันร่วมกันและ CI ตรวจผลกระทบ โดยผู้เขียนยอมรับต้นทุนสูงและยังทำให้ OSS ทั้งโลกใช้แบบนี้ไม่ได้

**ข้อจำกัด / Trade-off:** อย่ายก Live at Head มาเป็นกฎกับ Dependency ภายนอกโดยไม่มีโครงสร้างรองรับ

### 4. Compatibility tests and update cadence

กลยุทธ์ Upgrade ต้อง Build ซ้ำได้ จัดการ Security และใช้ CI ตรวจผู้ใช้ downstream เท่าที่เสี่ยง/คุ้มค่า การรันทุก Test ของทุก Dependents มีต้นทุนมหาศาล

**ข้อจำกัด / Trade-off:** CI ไม่พิสูจน์ทุกพฤติกรรม และ Security/License ต้องมีหลักฐานร่วมสมัย

### 5. Exporting dependencies and OSS sustainability

การเผยแพร่ Library สร้างภาระ Maintenance, Reputation, License และ Community; กรณี gflags แสดง Fork ภายในกับภายนอกที่ Sync กันไม่ไหว

**ข้อจำกัด / Trade-off:** บางกรณีต้อง Fork เพื่อ Security, Fix เร่งด่วน หรือข้อกำกับ

## Candidate Principles — ยังไม่ใช่กฎกลาง

### EF-G34 — ประเมินความเสี่ยงทั้งกราฟ Dependency

- **ชื่ออังกฤษ:** Assess Dependency Graph Risk, Not Just Direct Imports
- **ที่มาในบท:** Why Is Dependency Management So Difficult?; Diamond Dependency
- **Layer A — กลไก/เมื่อควรใช้:** ดู Transitive Version, Compatibility และ Owner ของตัวสำคัญ
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ไล่ทุกตัวละเอียดอาจแพง ให้เน้น Critical
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G35 — มอง SemVer เป็นสัญญาณที่ต้องตรวจเพิ่ม

- **ชื่ออังกฤษ:** Treat Version Promises as Hypotheses Requiring Tests
- **ที่มาในบท:** Semantic Versioning; Live at Head; Compatibility
- **Layer A — กลไก/เมื่อควรใช้:** ใช้ Lockfile/Reproducible Build กับ Update/Contract Tests
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** Pin มากไปค้างช่องโหว่ Unpin มากไป Build ไม่ซ้ำ
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G36 — กำหนดเจ้าของและแนวทางอัปเดต Dependency

- **ชื่ออังกฤษ:** Make Dependency Ownership and Update Paths Explicit
- **ที่มาในบท:** Live at Head; Exporting Dependencies; gflags
- **Layer A — กลไก/เมื่อควรใช้:** รู้ว่าใครดูแลตัวสำคัญและจะ Sync Update/Fork อย่างไร
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** บังคับ Provider รับภาระแทน Consumer ข้ามองค์กรอาจทำไม่ได้
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

## เชื่อมกับ Workflow ของเรา (ยังเป็นข้อเสนอ)

Technical Discovery: dependency evidence; Solution Design: vendor/build-vs-buy risk; Delivery Planning: upgrade work and compatibility tests; Implementation/Operations: SBOM/security updates and lockfile governance (modern cross-check needed).

## สิ่งที่ต้องตรวจสอบก่อน Approve

1. เนื้อหาจาก Google สอดคล้องกับบริบท Solo/Team/Multi-repo ของเราแค่ไหน?
2. มีข้อโต้แย้งจากงานวิจัย/แนวปฏิบัติล่าสุดหรือไม่?
3. หลักการซ้ำกับ EF-G01–24 หรือไม่ และควรรวมกันแทนเพิ่มกฎใหม่ไหม?
4. มีงานตัวอย่างที่ไม่เป็นความลับให้ทดลองและวัดผลได้หรือไม่?

**ยังไม่เปลี่ยน Accepted Workflows, Delivery Planning Draft, AGENTS.md หรือสร้าง Skills**
