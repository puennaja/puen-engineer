# Behavioral Design Patterns — สรุปภาษาไทย

อ้างอิง: https://refactoring.guru/design-patterns/behavioral-patterns | สรุปและตัวอย่างเรียบเรียงใหม่สำหรับ My Engineer (2026-10-17)

เอกสารนี้เป็นโน้ตศึกษา ไม่ใช่งานแปลต้นฉบับหรือกฎบังคับใช้ pattern; ตัวอย่าง Backend/BFF เป็นกรณีประยุกต์ที่จัดทำขึ้นใหม่

## ภาพรวม

มุ่งแยกความรับผิดชอบในการสื่อสาร การเลือกอัลกอริทึม และการจัดการสถานะ/คำสั่ง

## 1. Chain of Responsibility

**ปัญหาที่แก้:** ขั้นตอนตรวจหรือจัดการ request มีหลาย handler และอยากประกอบลำดับได้

**หลักการและวิธีทำงาน:** Handler รับ request แล้ว handle หรือส่งต่อ next; กำหนดหยุด chain เมื่อไรและเมื่อไม่มีใครรับผิดชอบ

**ตัวอย่าง BE/BFF:** API pipeline authentication → authorization → rate-limit → handler

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ middleware หรือ validation chain ที่ลำดับชัด

**ข้อเสียและจุดทดสอบ:** การเปลี่ยน order ส่งผลต่อ security; log จุด reject และต้องมี default-deny

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/chain-of-responsibility

## 2. Command

**ปัญหาที่แก้:** ต้องส่งต่อ บันทึก คิว หรือทำซ้ำ action โดยแยก invoker ออกจาก executor

**หลักการและวิธีทำงาน:** Command encapsulate intent และ data; Invoker dispatch; Handler/Receiver execute; อาจ serialize เพื่อ queue และทำ audit

**ตัวอย่าง BE/BFF:** ApproveOperationCommand ส่งไป worker ที่ validate state และ write audit log

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ async jobs, history, scheduling และ undo ในบางโดเมน

**ข้อเสียและจุดทดสอบ:** command execution อาจซ้ำได้; design idempotency key, retries, permissions และ versioning

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/command

## 3. Iterator

**ปัญหาที่แก้:** caller ไม่ควรรู้โครงสร้าง collection หรือกระบวนการ traverse

**หลักการและวิธีทำงาน:** Iterator มี next/hasNext หรือ idiomatic language iterator; collection จัดการ traversal; streaming iterator อาจ fetch lazily

**ตัวอย่าง BE/BFF:** client iterator อ่าน paginated merchant events ทีละ page

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ tree traversal, stream หรือ paginated records

**ข้อเสียและจุดทดสอบ:** lazy IO ทำให้ latency และ N+1 แฝง; ระบุ error/cancel semantics

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/iterator

## 4. Mediator

**ปัญหาที่แก้:** components ติดต่อกันหลายต่อหลายจน coupling สูง

**หลักการและวิธีทำงาน:** components แจ้ง Mediator แทนเรียกกันโดยตรง; mediator ประสาน interaction ตาม event หรือ use case

**ตัวอย่าง BE/BFF:** ApprovalCoordinator ประสาน validator, notifier และ audit publisher

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ coordination ซับซ้อนที่ต้องแก้การสื่อสารโดยไม่แก้ peers

**ข้อเสียและจุดทดสอบ:** mediator อาจกลายเป็น god object; domain rules ควรอยู่เจ้าของที่ถูกต้อง

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/mediator

## 5. Memento

**ปัญหาที่แก้:** ต้องเก็บ snapshot เพื่อย้อนคืน state โดยไม่เปิดเผย internals ของ object

**หลักการและวิธีทำงาน:** Originator สร้าง memento; Caretaker เก็บ snapshot แต่ไม่แก้เนื้อหา; restore ผ่าน Originator

**ตัวอย่าง BE/BFF:** draft configuration version ก่อนแก้ไข เพื่อ rollback

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ undo, editor history และ checkpoints

**ข้อเสียและจุดทดสอบ:** ขนาด snapshot, schema evolution, encryption/PII และ retention ต้องจัดการ

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/memento

## 6. Observer

**ปัญหาที่แก้:** หนึ่ง event ควรแจ้งผู้สนใจหลายคนโดย publisher ไม่รู้ implementations

**หลักการและวิธีทำงาน:** Subject maintain subscribers; notify เมื่อ event เกิด; observers ทำ independent response; ต้องนิยาม sync หรือ async

**ตัวอย่าง BE/BFF:** เมื่อ operation status เปลี่ยน ส่งต่อ audit และ metrics consumers

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ in-process event subscription; ข้ามบริการมักต้อง durable eventing

**ข้อเสียและจุดทดสอบ:** error isolation, ordering, duplicate delivery และ unsubscribe lifecycle สำคัญ

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/observer

## 7. State

**ปัญหาที่แก้:** object เปลี่ยน behavior ตามสถานะ และ if/switch ตาม state โตขึ้น

**หลักการและวิธีทำงาน:** Context ถือ State interface; concrete state จัดการ action และ transition; transition invalid ต้อง reject

**ตัวอย่าง BE/BFF:** Payout: Draft → Pending → Approved หรือ Rejected แต่ละสถานะอนุญาต actions ต่างกัน

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ business lifecycle ที่ transitions และ behavior มีความหมาย

**ข้อเสียและจุดทดสอบ:** หาก state น้อย ใช้ enum + transition table อาจง่ายกว่า; ต้องทดสอบทุก transition

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/state

## 8. Strategy

**ปัญหาที่แก้:** มีหลาย algorithm ทำงาน contract เดียวกันและต้องสลับได้

**หลักการและวิธีทำงาน:** Strategy interface ประกาศ algorithm; concrete strategies implement; Context เลือกหรือรับ injection โดยไม่รู้รายละเอียด

**ตัวอย่าง BE/BFF:** DiscountStrategy สำหรับ membership tiers หรือ fee policies

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะเมื่อ algorithms สลับกันด้วย config หรือ business variant

**ข้อเสียและจุดทดสอบ:** เพิ่ม class/registration มากเกินสำหรับ logic ง่าย ๆ; อย่าซ่อนการเลือก strategy

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/strategy

## 9. Template Method

**ปัญหาที่แก้:** หลาย workflows มี skeleton เหมือนกันแต่บางขั้นต่างกัน

**หลักการและวิธีทำงาน:** Base class กำหนด algorithm ลำดับตายตัว; subclasses override hooks/steps; invariant steps อยู่ base

**ตัวอย่าง BE/BFF:** ImportJob: parse → validate → transform → persist โดย CSV/JSON ต่างที่ parse

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ framework ที่ inheritance เป็น extension mechanism อยู่แล้ว

**ข้อเสียและจุดทดสอบ:** inheritance coupling และ fragile base class; ใน Go/TS อาจใช้ composition/higher-order functions

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/template-method

## 10. Visitor

**ปัญหาที่แก้:** ต้องเพิ่ม operations ต่อโครงสร้าง node หลายชนิด โดยไม่แก้ logic ของ node ทุกครั้ง

**หลักการและวิธีทำงาน:** Visitor มี visit method ต่อ node type; Element.accept(visitor) dispatch ชนิดจริง (double dispatch)

**ตัวอย่าง BE/BFF:** AST nodes ใช้ visitors แยก validate, render, collect-dependencies

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะเมื่อ node types ค่อนข้างคงที่แต่ operations เพิ่มบ่อย

**ข้อเสียและจุดทดสอบ:** เพิ่ม node type ใหม่ต้องแก้ visitor ทุกตัว; อาจใช้ pattern matching ตามภาษา

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/visitor

## แนวทางนำไปใช้ใน My Engineer

ก่อนเลือก pattern ให้ระบุ requirement, change pressure, ทางเลือกที่เรียบง่ายกว่า, ค่าใช้จ่ายด้าน maintenance และ test plan; ใช้ pattern เท่าที่ evidence รองรับเท่านั้น บันทึก ADR หากการตัดสินใจมีนัยสำคัญ และบันทึกผล pilot/ปัญหาจริงใน pilot-projects/docs แยกจาก reusable knowledge นี้
