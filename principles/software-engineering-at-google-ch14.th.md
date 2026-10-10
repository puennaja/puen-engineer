# Software Engineering at Google — บทที่ 14: Larger Testing

- **สถานะ:** Draft Study Notes v0.1; Candidate Principles **ยังไม่ Approved**
- **ผู้เขียนบท:** Joseph Graves
- **แหล่งต้นฉบับ:** https://abseil.io/resources/swe-book/html/ch14.html
- **ฉบับหลักภาษาอังกฤษ:** [English](./software-engineering-at-google-ch14.md)
- **วันที่ตรวจสอบ:** 2026-10-10
- **วิธีสรุป:** อ่านบทเผยแพร่จากต้นฉบับ Google และเรียบเรียงใหม่โดยไม่คัดลอกหนังสือ; บทวิเคราะห์และ Candidate คือข้อสังเคราะห์ My Engineer

**ข้อควรระวัง:** กรณีศึกษาองค์กร Google (ปี 2020) ไม่ใช่ข้อบังคับทั่วไป ต้องตรวจสอบการใช้งานในบริบทปัจจุบันอีกครั้ง

## วิเคราะห์ประเด็นสำคัญของบท

### 1. Why large tests exist and what they miss

การทดสอบขนาดใหญ่เพิ่มความสมจริงต่อระบบ Production และจับช่องโหว่ที่ Unit Tests ไม่เห็น เช่น Mock ล้าสมัย, Configuration, Load, Unexpected Inputs และพฤติกรรมจากการเชื่อมหลายบริการ ขนาด Test กับขอบเขตพฤติกรรมที่ทดสอบไม่ใช่สิ่งเดียวกัน

**ข้อจำกัด / Trade-off:** ความสมจริงสูงขึ้นแลกกับต้นทุน เวลา และความไม่แน่นอนของผล

### 2. Why not make everything E2E

Large Test อาจช้า Flaky ไม่แยกสภาพแวดล้อม ค่าใช้จ่ายสูง และไม่มีเจ้าของชัด การทดสอบครบทุกเส้นทางในระบบหลาย Service ไม่ Scale จึงควรลด System Under Test ให้เล็กที่สุดเท่าที่ยังตรวจความเสี่ยงจริงได้

**ข้อจำกัด / Trade-off:** ลด SUT มากเกินไปก็พลาด Bug Integration ที่ต้องการค้น

### 3. Construct useful systems and data

Large Test มีการเตรียม SUT, Seed Data, ยิงคำสั่ง และตรวจผล ต้องชั่งระหว่าง Hermeticity กับ Fidelity ใช้ข้อมูลสังเคราะห์หรือข้อมูลที่มีสิทธิ์ และระวัง Seed ผ่าน Database ตรงจนข้าม Validation จริง

**ข้อจำกัด / Trade-off:** ข้อมูลจริงเสี่ยง Privacy และการยิง Third Party จริงอาจสร้างค่าใช้จ่ายหรือผลกระทบ

### 4. Choose risk-specific verification

หนังสือไล่ Functional, UI, Load/Stress, Config, Exploratory, Differential, UAT, Prober/Canary, Disaster Recovery/Chaos และ User Evaluation; บางกรณีต้องเทียบผลหลายเวอร์ชัน ไม่ใช่มีแค่ Assertion

**ข้อจำกัด / Trade-off:** Test บน Production กระทบผู้ใช้จริงได้ การทำ Chaos ต้องมีขอบเขตปลอดภัย

### 5. Fit large tests into daily workflow

Large Test ต้องเร็วตามจุดที่ใช้ มี Failure Output ที่ช่วย Debug มี Owner ชัด ลด Sleep ด้วย Polling/Event และใช้ Record/Replay หรือ Contract Testing เมื่อช่วยลดภาระบริการจริง

**ข้อจำกัด / Trade-off:** ไฟล์ Replay เก่าได้ การ Retry กลบ Flaky และไม่จำเป็นต้องรันทุก Large Test ก่อน Merge

## Candidate Principles — ยังไม่ใช่กฎกลาง

### EF-G25 — เลือกความสมจริงของ Test ตามความเสี่ยง

- **ชื่ออังกฤษ:** Choose Test Fidelity by Risk
- **ที่มาในบท:** Fidelity; Common Gaps in Unit Tests; Types of Larger Tests
- **Layer A — กลไก/เมื่อควรใช้:** เลือก Integration/Config/Load ที่เหมือนจริงเมื่อ Unit Tests ไม่ครอบคลุม Failure Mode
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ไม่ต้องเลือก Fidelity สูงสุดเสมอ Production/Shared Test เสี่ยงและแพง
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G26 — ใช้ SUT เล็กที่สุดที่เพียงพอ

- **ชื่ออังกฤษ:** Prefer the Smallest Sufficient System Under Test
- **ที่มาในบท:** Larger Tests at Google Scale; Reducing the size of your SUT
- **Layer A — กลไก/เมื่อควรใช้:** แบ่ง Test ตาม Boundary พร้อม Contract ที่ตรวจได้
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** แบ่งเล็กและ Mock มากไปจนพลาด Integration จริง
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G27 — กำหนดเจ้าของและทำให้ Integration Test Debug ได้

- **ชื่ออังกฤษ:** Make Integration Tests Diagnosable and Owned
- **ที่มาในบท:** Large Tests and the Developer Workflow; Owning Large Tests
- **Layer A — กลไก/เมื่อควรใช้:** มี Owner, Failure Diagnostic และความถี่ในการรันที่เหมาะ
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ภาระการกำกับเกินจำเป็นหรือ Test Flaky จนทุกคนมองข้าม
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

## เชื่อมกับ Workflow ของเรา (ยังเป็นข้อเสนอ)

Technical Discovery: discover actual boundaries/configuration risks; Solution Design: plan failure evidence; Delivery Planning: budget integration test work; Implementation & Verification / Release: choose testing levels and production-safe checks.

## สิ่งที่ต้องตรวจสอบก่อน Approve

1. เนื้อหาจาก Google สอดคล้องกับบริบท Solo/Team/Multi-repo ของเราแค่ไหน?
2. มีข้อโต้แย้งจากงานวิจัย/แนวปฏิบัติล่าสุดหรือไม่?
3. หลักการซ้ำกับ EF-G01–24 หรือไม่ และควรรวมกันแทนเพิ่มกฎใหม่ไหม?
4. มีงานตัวอย่างที่ไม่เป็นความลับให้ทดลองและวัดผลได้หรือไม่?

**ยังไม่เปลี่ยน Accepted Workflows, Delivery Planning Draft, AGENTS.md หรือสร้าง Skills**
