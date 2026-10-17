# Design Patterns — Refactoring.Guru (สรุปภาษาไทย)

เอกสารสรุป Design Patterns ทั้ง 22 ตัว เรียบเรียงใหม่จากแนวคิดตาม [Refactoring.Guru Catalog](https://refactoring.guru/design-patterns/catalog) โดยแยกเป็น **หนึ่งไฟล์ต่อหมวด** พร้อมปัญหาที่แก้ วิธีทำงาน ตัวอย่าง Backend/BFF เกณฑ์เลือกใช้ และข้อควรระวัง

- [Creational Patterns (5)](./creational.th.md) — การสร้างและประกอบ objects
- [Structural Patterns (7)](./structural.th.md) — การจัดองค์ประกอบและเชื่อม components
- [Behavioral Patterns (10)](./behavioral.th.md) — การประสานงาน algorithms และ state

> เป็นบทสรุปเพื่อการศึกษา ไม่ใช่งานแปล/คัดลอกบทความ ไดอะแกรม หรือ source code จากเว็บต้นฉบับ; ลิงก์บทความแต่ละเรื่องอยู่ในไฟล์หมวด

## วิธีใช้ในงานจริง

เริ่มจากปัญหาและความเปลี่ยนแปลงที่ต้องรองรับก่อน แล้วเทียบ pattern กับวิธีง่าย ๆ เช่น function, interface, DI, enum หรือ standard library; ประเมิน trade-offs/testability และบันทึกเหตุผลใน ADR เมื่อต้องตัดสินใจเชิงสถาปัตยกรรม
