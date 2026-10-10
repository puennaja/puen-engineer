# Software Engineering at Google — บทที่ 16: Version Control and Branch Management

- **Approval:** 🟢 Accepted — สรุปบทและ Principles ได้รับรองเป็นแนวทางประกอบการตัดสินใจ ไม่ใช่กฎบังคับ (ตรวจสอบหลักฐานอิสระเพิ่มเติมได้)
- **สถานะ:** Draft Study Notes v0.1; Candidate Principles **ยังไม่ Approved**
- **ผู้เขียนบท:** Titus Winters
- **แหล่งต้นฉบับ:** https://abseil.io/resources/swe-book/html/ch16.html
- **ฉบับหลักภาษาอังกฤษ:** [English](./software-engineering-at-google-ch16.md)
- **วันที่ตรวจสอบ:** 2026-10-10
- **วิธีสรุป:** อ่านบทเผยแพร่จากต้นฉบับ Google และเรียบเรียงใหม่โดยไม่คัดลอกหนังสือ; บทวิเคราะห์และ Candidate คือข้อสังเคราะห์ My Engineer

**ข้อควรระวัง:** กรณีศึกษาองค์กร Google (ปี 2020) ไม่ใช่ข้อบังคับทั่วไป ต้องตรวจสอบการใช้งานในบริบทปัจจุบันอีกครั้ง

## วิเคราะห์ประเด็นสำคัญของบท

### 1. VCS establishes history and source of truth

Version Control มีประวัติ การเปลี่ยนแบบ Atomic และ Source of Truth ที่ทีมเห็นร่วมกัน ลดความผิดพลาดในการประสานงาน และต้องแยก Tool จาก Branching Policy

**ข้อจำกัด / Trade-off:** มี Working Copy หลายชุดได้แต่ต้องมี Integration Source of Truth

### 2. Trunk-based versus long-lived development branches

Google เห็นว่า Long-lived Dev Branch เลื่อนการรวมงาน เสี่ยง Conflict และหาต้นเหตุยาก จึงแนะนำ Small Changes บน Trunk ที่มี CI, Review และ Feature Flags แต่ Release Branch ยังอาจจำเป็น

**ข้อจำกัด / Trade-off:** Trunk-based โดยไม่มี Test และ Rollback ที่เชื่อถือได้ไม่ทำให้ปลอดภัยเอง

### 3. One-Version Rule and scaling

เมื่อแต่ละทีมเลือก Version ของ Component เดียวกันคนละแบบ ต้นทุน Integration เพิ่ม Google's One-Version Rule และ Monorepo ช่วยลดมิติการตัดสินใจ แต่ต้องมี Tooling และ Coordination มาก

**ข้อจำกัด / Trade-off:** นโยบาย One-Version ใช้ตรง ๆ ไม่ได้กับ Vendor ภายนอก Permission แยก หรือทุก Multi-repo

### 4. Repository choice is contextual

บทนี้เปรียบเทียบ Centralized/Distributed, Monorepo/Manyrepo และ Virtual Monorepo; Multi-repo เหมาะกับ Permission/Release แยก ขณะที่ Monorepo ช่วย Cross-code Search และ Refactor

**ข้อจำกัด / Trade-off:** อย่าย้าย Repo เพียงเพราะ Google ใช้ Monorepo ต้องมีหลักฐานว่าปัญหาคืออะไร

### 5. Current practice boundaries

แนวทาง Google อิง Infrastructure ขนาดใหญ่และปี 2020 ส่วนการคาดการณ์อนาคตของ Version Control เป็นสมมติฐาน ไม่ใช่ข้อเท็จจริงวันนี้

**ข้อจำกัด / Trade-off:** ต้องดู GitLab CI, Cross-repo Contracts และ Release Constraints จริงก่อนกำหนดนโยบาย

## Candidate Principles — ยังไม่ใช่กฎกลาง

### EF-G31 — รักษาแหล่งอ้างอิงหลักสำหรับการ Integrate

- **ชื่ออังกฤษ:** Maintain an Explicit Integration Source of Truth
- **ที่มาในบท:** Source of Truth; Why Version Control Matters
- **Layer A — กลไก/เมื่อควรใช้:** ตกลง Revision ที่ใช้รวมงานและชุด Component ที่เข้ากัน
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** หลายระบบอาจมี Trunk ร่วมกันไม่ได้ ต้องระบุ Scope
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G32 — รวมงานชิ้นเล็กเร็วพร้อมหลักฐานตรวจ

- **ชื่ออังกฤษ:** Integrate Small Changes Early with Verification
- **ที่มาในบท:** Dev Branches; Trunk-based development; Release Branches
- **Layer A — กลไก/เมื่อควรใช้:** ใช้ Branch อายุสั้น CI/Test และแยก Feature ที่ยังไม่พร้อม
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ถ้าไม่มีระบบความปลอดภัย Merge เร็วอาจกระจาย Bug
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G33 — ลด Version Skew เมื่อคุ้มต้นทุนประสานงาน

- **ชื่ออังกฤษ:** Constrain Version Divergence Where Coordination Pays Off
- **ที่มาในบท:** One-Version Rule; Monorepo/Manyrepo trade-offs
- **Layer A — กลไก/เมื่อควรใช้:** ระบุ Version/Contract Compatibility และเจ้าของงาน Upgrade ข้าม Repo
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** อย่าบังคับ Version เดียวทั่วระบบที่ Release อิสระ
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

## เชื่อมกับ Workflow ของเรา (ยังเป็นข้อเสนอ)

Delivery Planning: dependency sequencing and merge cadence; Technical Discovery: compatible service versions; Implementation/Verification: CI on small changes; Release: release branch and deployment constraints.

## สิ่งที่ต้องตรวจสอบก่อน Approve

1. เนื้อหาจาก Google สอดคล้องกับบริบท Solo/Team/Multi-repo ของเราแค่ไหน?
2. มีข้อโต้แย้งจากงานวิจัย/แนวปฏิบัติล่าสุดหรือไม่?
3. หลักการซ้ำกับ EF-G01–24 หรือไม่ และควรรวมกันแทนเพิ่มกฎใหม่ไหม?
4. มีงานตัวอย่างที่ไม่เป็นความลับให้ทดลองและวัดผลได้หรือไม่?

**ยังไม่เปลี่ยน Accepted Workflows, Delivery Planning Draft, AGENTS.md หรือสร้าง Skills**
