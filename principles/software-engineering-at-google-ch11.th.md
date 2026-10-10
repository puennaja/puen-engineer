# Software Engineering at Google — บทที่ 11: Testing Overview

- **Approval:** 🟢 Accepted — สรุปบทและ Principles ได้รับรองเป็นแนวทางประกอบการตัดสินใจ ไม่ใช่กฎบังคับ (ตรวจสอบหลักฐานอิสระเพิ่มเติมได้)
- **สถานะ:** Draft Study Notes v0.1 — Candidate ยังไม่ได้รับรอง
- **ขอบเขต:** อ่านจากตัวบทเผยแพร่ของ Google ครบทั้งบท (รวมบทสรุปและตัวอย่าง); สรุปใหม่ ไม่ใช่คำแปลแบบคัดลอก
- **ผู้เขียน:** Adam Bender
- **อ้างอิงหลัก:** https://abseil.io/resources/swe-book/html/ch11.html
- **ฉบับอังกฤษหลัก:** [English](./software-engineering-at-google-ch11.md)
- **วันที่ตรวจสอบ:** 2026-10-10

**วิธีอ่าน:** เนื้อหาในส่วนสรุปอธิบายข้อเสนอและตัวอย่างของผู้เขียน ส่วน Candidate และ Mapping เป็นการสังเคราะห์ของ My Engineer ไม่ใช่กฎจาก Google โดยตรง ข้อมูลเชิงตัวเลขจากปี 2020 ไม่ใช่มาตรฐานปัจจุบัน

## สรุปและวิเคราะห์เนื้อหาทั้งบท

### 1. Tests make change sustainable

Automation ที่ Engineer เขียนช่วยลด Debug ซ้ำ เพิ่มความมั่นใจ เป็นตัวอย่างที่รันได้ และเผย API ที่ทดสอบยาก กรณี GWS เป็นประสบการณ์เฉพาะ ไม่ใช่ตัวเลขที่ใช้ได้กับทุกทีม

**ข้อจำกัดและวิจารณญาณ:** Test ที่เปราะบางจำนวนมากอาจขัดขวางการเปลี่ยนระบบ

### 2. Write, run, react

วงจรที่มีค่าคือ เขียน Test → รันบ่อย → ตอบสนองต่อ Test ที่ Fail อย่าปล่อยแดงค้างจนหมดความเชื่อถือ

**ข้อจำกัดและวิจารณญาณ:** มี Test ใน Repo อย่างเดียวไม่ใช่หลักฐาน หากไม่ได้รัน

### 3. Size differs from scope

Google แบ่ง Small/Medium/Large ตามการใช้ Process และทรัพยากร แยกจาก Scope ของโค้ดที่ตรวจ Small มักเร็วกว่า Medium ใช้บริการ Local ได้ Large อาจพึ่ง Network จริง

**ข้อจำกัดและวิจารณญาณ:** นิยามข้อจำกัดของ Google ไม่ใช่มาตรฐานบังคับของทุกทีม

### 4. Balance levels and test real behavior

Test Pyramid เป็น Heuristic โดย Google ยกสัดส่วนคร่าว ๆ 80/15/5 ไม่ใช่เป้าตายตัว และใช้ Dependency จริงเมื่อเหมาะแทนการ Mock ทุกชั้น

**ข้อจำกัดและวิจารณญาณ:** Coverage บอกเพียงว่าโค้ดถูกรัน ไม่ได้ยืนยันความถูกต้อง

### 5. Hermetic tests and flakiness

การแยกสภาพแวดล้อม ความแน่นอน และความเร็วช่วยรักษาความเชื่อถือ Sleep, Shared State และ Network เพิ่ม Flaky; การ Retry ไม่ได้แก้ต้นเหตุ

**ข้อจำกัดและวิจารณญาณ:** อย่าสมมติว่า E2E ทุกระบบจะไม่ Flaky ได้เลย

### 6. Culture and failure testing

Orientation, Test Certified และ Testing on the Toilet ทำให้ Test เป็นนิสัยของทีม Beyoncé Rule ชวนทดสอบพฤติกรรมที่ต้องการรักษา รวมถึงกรณีล้มเหลว

**ข้อจำกัดและวิจารณญาณ:** อย่าลอกระบบคะแนนความพร้อมของ Google โดยไม่ดูความเหมาะสม

## Candidate Principles (ยังไม่ Approved)

### EF-G19 — ใช้ Test เพื่อรองรับการเปลี่ยนแปลง

- **ชื่ออังกฤษ:** Use Tests to Protect Changeability
- **ต้นฉบับที่เกี่ยวข้อง:** Why Do We Write Tests?; Benefits of Testing Code
- **Layer A — ความหมาย / เมื่อไรควรใช้:** รักษา Business Invariants และ Compatibility ระหว่าง Refactoring
- **Layer B — Trade-offs / ข้อควรระวัง:** Test ที่ยึด Implementation เกินไปทำให้ Refactor ยาก
- **Layer C — หลักฐานและสถานะ:** อ้างอิงเฉพาะหัวข้อข้างต้นจากบทเต็ม แต่ยังไม่ได้ตรวจหลักฐานอิสระหรือลองใช้จริง; Confidence: สูงสำหรับการระบุแนวคิดในบท ต่ำ/ยังไม่ประเมินสำหรับการยกเป็นกฎสากล; ทบทวนเมื่อมีหลักฐานใหม่หรือ Pilot ไม่สนับสนุน
- **Status:** Candidate

### EF-G20 — ให้ผล Test เร็ว เชื่อถือได้ และนำไปแก้ได้

- **ชื่ออังกฤษ:** Keep Test Feedback Fast, Reliable, and Actionable
- **ต้นฉบับที่เกี่ยวข้อง:** Write, Run, React; Test Sizes; Flaky Tests Are Expensive
- **Layer A — ความหมาย / เมื่อไรควรใช้:** เน้นการตรวจที่แยกกันและให้ผลแน่นอน ตอบสนองเมื่อ Fail เร็ว
- **Layer B — Trade-offs / ข้อควรระวัง:** Mock มากเกินไปอาจพลาดปัญหา Integration
- **Layer C — หลักฐานและสถานะ:** อ้างอิงเฉพาะหัวข้อข้างต้นจากบทเต็ม แต่ยังไม่ได้ตรวจหลักฐานอิสระหรือลองใช้จริง; Confidence: สูงสำหรับการระบุแนวคิดในบท ต่ำ/ยังไม่ประเมินสำหรับการยกเป็นกฎสากล; ทบทวนเมื่อมีหลักฐานใหม่หรือ Pilot ไม่สนับสนุน
- **Status:** Candidate

### EF-G21 — ทดสอบพฤติกรรมสำคัญ ไม่ไล่ตัวเลข Coverage

- **ชื่ออังกฤษ:** Test Important Behavior, Not Coverage Targets
- **ต้นฉบับที่เกี่ยวข้อง:** The Beyoncé Rule; A Note on Code Coverage; Test Scope
- **Layer A — ความหมาย / เมื่อไรควรใช้:** เลือกกรณี Business Rule, Failure และ Contract ที่มีผลจริง
- **Layer B — Trade-offs / ข้อควรระวัง:** เปอร์เซ็นต์เดียวไม่รับประกันว่าตรวจครบ
- **Layer C — หลักฐานและสถานะ:** อ้างอิงเฉพาะหัวข้อข้างต้นจากบทเต็ม แต่ยังไม่ได้ตรวจหลักฐานอิสระหรือลองใช้จริง; Confidence: สูงสำหรับการระบุแนวคิดในบท ต่ำ/ยังไม่ประเมินสำหรับการยกเป็นกฎสากล; ทบทวนเมื่อมีหลักฐานใหม่หรือ Pilot ไม่สนับสนุน
- **Status:** Candidate

## Mapping กลับ My Engineer (เสนอ)

Implementation & Verification future workflow: tests as evidence; Technical Discovery: locate missing coverage; Solution Design: decide invariants, test seams; Delivery Planning: test work included in slices.

## ข้อควรระวังและขั้นถัดไป

แนวปฏิบัติและตัวเลขเป็นประสบการณ์ Google ภายใต้บริบทปี 2020 ไม่ใช่การรับรองว่าต้องใช้ในทุกองค์กร ก่อนเปลี่ยนเป็น Rule ให้ตรวจหลักฐานปัจจุบัน ข้อโต้แย้ง ผลกระทบ และทดลองกับงานที่ไม่เป็นความลับ ไม่เปลี่ยน AGENTS.md, Workflow ที่ Accepted หรือสร้าง Skill โดยอัตโนมัติ

**ต้นฉบับเต็ม:** https://abseil.io/resources/swe-book/html/ch11.html
