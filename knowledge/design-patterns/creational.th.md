# Creational Design Patterns — สรุปภาษาไทย

อ้างอิง: https://refactoring.guru/design-patterns/creational-patterns | สรุปและตัวอย่างเรียบเรียงใหม่สำหรับ My Engineer (2026-10-17)

เอกสารนี้เป็นโน้ตศึกษา ไม่ใช่งานแปลต้นฉบับหรือกฎบังคับใช้ pattern; ตัวอย่าง Backend/BFF เป็นกรณีประยุกต์ที่จัดทำขึ้นใหม่

## ภาพรวม

มุ่งแยกวิธีสร้าง object ออกจากการใช้งาน เพื่อควบคุมชนิด ความซับซ้อนในการประกอบ และวงจรชีวิต

## 1. Factory Method

**ปัญหาที่แก้:** ปัญหาคือ client ผูกกับ concrete constructor ทำให้เพิ่มชนิด product แล้วต้องแก้ caller หลายจุด

**หลักการและวิธีทำงาน:** ประกาศ Product contract; Creator มี factory method คืน Product; ConcreteCreator override เพื่อเลือกชนิดจริง; business operation ใช้ Product contract ไม่รู้คลาสจริง

**ตัวอย่าง BE/BFF:** ตัวส่ง notification มี EmailCreator และ SMSCreator; โค้ด dispatch ไม่เรียก new EmailSender โดยตรง

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ framework หรือ extension point ที่ subclass ต้องควบคุมการสร้าง; ถ้ามีเพียง map ประเภทไป constructor ง่าย ๆ ใช้ factory function พอ

**ข้อเสียและจุดทดสอบ:** เพิ่ม subclass และ navigation cost; ไม่เท่ากับ static factory function

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/factory-method

## 2. Abstract Factory

**ปัญหาที่แก้:** ต้องสร้างหลาย product ที่เข้าชุดกัน โดยไม่ให้แต่ละจุดในระบบรู้ concrete class

**หลักการและวิธีทำงาน:** ประกาศ contract ให้แต่ละชนิด product และ Factory contract มี method สร้าง product ทุกชนิด; เปลี่ยน concrete factory หนึ่งตัวเท่ากับเปลี่ยนทั้ง family

**ตัวอย่าง BE/BFF:** PaymentProviderFactory สร้าง PaymentClient และ WebhookVerifier ที่เข้าคู่กับ provider เดียวกัน

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะเมื่อชุด implementation ต้องเข้ากันและต้องสลับทั้งชุด

**ข้อเสียและจุดทดสอบ:** เพิ่ม product type ใหม่ต้องแก้ทุก factory; ระวัง interface โต

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/abstract-factory

## 3. Builder

**ปัญหาที่แก้:** constructor มี parameters มากและบาง field เป็น optional ทำให้เกิด invalid configuration

**หลักการและวิธีทำงาน:** แยกการประกอบเป็น steps; Builder เก็บ intermediate state และ build ตรวจ invariant; Director เป็น optional สำหรับ recipe ใช้บ่อย

**ตัวอย่าง BE/BFF:** ReportQueryBuilder เพิ่มช่วงเวลา filters sorting ก่อน build เป็น query object

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ object ประกอบยาก; ถ้าเป็น simple DTO หรือ named options ใช้ object literal + validator

**ข้อเสียและจุดทดสอบ:** ต้องกำหนด default และ validation จุดเดียวเพื่อไม่ให้เกิด partial object

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/builder

## 4. Prototype

**ปัญหาที่แก้:** การสร้าง object ซับซ้อนหรือต้องเริ่มจาก instance ที่ตั้งค่าแล้ว

**หลักการและวิธีทำงาน:** กำหนด clone operation ใน prototype หรือใช้ clone service; ผู้เรียก copy template โดยไม่รู้ concrete class; แยก shared immutable กับ mutable nested state

**ตัวอย่าง BE/BFF:** clone campaign notification template แล้วแก้เฉพาะ receiver และ schedule

**เมื่อไรควรใช้ / ทางเลือก:** ใช้เมื่อ cloning ถูกกว่า reconstruction และ semantics ของการคัดลอกชัด

**ข้อเสียและจุดทดสอบ:** shallow copy อาจแชร์ slice/map/pointer ที่ถูกแก้ไขภายหลัง; ระวัง ID และ secrets

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/prototype

## 5. Singleton

**ปัญหาที่แก้:** ต้องรับประกัน instance เดียวภายใน scope ที่กำหนดและเข้าถึงซ้ำได้

**หลักการและวิธีทำงาน:** ซ่อน construction, เก็บ instance เดียวและมี accessor; ถ้า concurrent ต้อง initialize อย่างปลอดภัย; นิยาม scope ว่า per-process ไม่ใช่ทั้ง cluster

**ตัวอย่าง BE/BFF:** shared application configuration หรือ registry; ปกติ prefer dependency injection with application scope

**เมื่อไรควรใช้ / ทางเลือก:** จำกัดไว้กับ resource ที่ต้อง one instance จริง ไม่ใช่ service ทั่วไป

**ข้อเสียและจุดทดสอบ:** global dependency ซ่อน coupling, test isolation และ parallel tests ยาก; ไม่รับประกันหนึ่งเดียวข้ามหลาย pods

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/singleton

## แนวทางนำไปใช้ใน My Engineer

ก่อนเลือก pattern ให้ระบุ requirement, change pressure, ทางเลือกที่เรียบง่ายกว่า, ค่าใช้จ่ายด้าน maintenance และ test plan; ใช้ pattern เท่าที่ evidence รองรับเท่านั้น บันทึก ADR หากการตัดสินใจมีนัยสำคัญ และบันทึกผล pilot/ปัญหาจริงใน pilot-projects/docs แยกจาก reusable knowledge นี้
