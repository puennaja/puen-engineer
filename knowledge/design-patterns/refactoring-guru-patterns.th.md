# Design Patterns — Refactoring.Guru (Thai Engineering Field Guide)

> Original synthesis for My Engineer, reviewed 2026-10-17. Source: https://refactoring.guru/design-patterns/catalog . This is **not** a translation, reproduction, or mirror of the original site. Consult linked articles for full diagrams and implementation walkthroughs.

## Scope and interpretation

- Covers **all 22 classic GoF patterns** catalogued by Refactoring.Guru: 5 creational, 7 structural, 10 behavioral.
- Each record includes purpose, selection signal, trade-off and **original My Engineer BE/BFF application idea**. These application ideas are examples, not claims made by the original site.
- Pattern is a vocabulary for a recurring design trade-off, **not** a requirement to introduce a class hierarchy. Prefer simple functions, dependency injection, and standard library facilities when they solve the problem.
- Refactoring techniques and code smells are separate catalogs, **not** included in the count of 22 design patterns. See https://refactoring.guru/refactoring/techniques and https://refactoring.guru/refactoring/smells .

## Selection rules

1. State the concrete change pressure first (what is likely to vary; who owns it; how often).
2. Write the simplest implementation that meets today's requirements; do not predict imaginary variants.
3. Compare alternatives including no pattern; estimate maintenance, testability, runtime, operational and cognitive costs.
4. Preserve service boundaries: patterns inside BFF do not justify distributed transactions or tightly coupled services.
5. Validate with tests including errors, retries, timeouts, concurrency, security and idempotency where applicable.
6. Record non-obvious decisions in ADR, and verify existing repository idioms before introducing new abstractions.

## Creational (5)

### Factory Method

- **Core idea:** ให้ subclass เลือกชนิด object ที่สร้างผ่าน contract กลาง
- **Consider when:** framework extension ที่ implementation ต่างตาม environment
- **Cost / misuse:** การสร้างชนิดใหม่ทำให้เกิด subclass จำนวนมาก
- **BE/BFF example (original):** เลือก transport handler ตามชนิด fulfillment
- **Source:** https://refactoring.guru/design-patterns/factory-method

### Abstract Factory

- **Core idea:** สร้างชุด object ที่สัมพันธ์กันและใช้ร่วมกันได้
- **Consider when:** เปลี่ยนทั้ง family ของ integrations/configurations
- **Cost / misuse:** มี interface และ factory มากเกินเมื่อมี variation น้อย
- **BE/BFF example (original):** สร้าง payment client + webhook verifier ของแต่ละ provider
- **Source:** https://refactoring.guru/design-patterns/abstract-factory

### Builder

- **Core idea:** ประกอบ object ที่ตั้งค่าหลายอย่างทีละขั้น
- **Consider when:** request/query/configuration ที่มี optional fields มาก
- **Cost / misuse:** validation และค่า default อาจกระจาย
- **BE/BFF example (original):** ประกอบ report query พร้อม filters
- **Source:** https://refactoring.guru/design-patterns/builder

### Prototype

- **Core idea:** สร้าง object ใหม่จากการ clone ต้นแบบ
- **Consider when:** การตั้งค่าต้นแบบแพงหรือซับซ้อน
- **Cost / misuse:** shallow copy ทำให้ mutable reference รั่ว
- **BE/BFF example (original):** clone template ของ notification
- **Source:** https://refactoring.guru/design-patterns/prototype

### Singleton

- **Core idea:** จำกัด instance ให้มีหนึ่งตัวพร้อมจุดเข้าถึงร่วม
- **Consider when:** state/registry เดียวที่มีเหตุผลชัด
- **Cost / misuse:** global mutable state ทำให้ test ยากและเกิด concurrency issues
- **BE/BFF example (original):** ใช้ application-scoped config ผ่าน DI แทน singleton แบบ global เมื่อทำได้
- **Source:** https://refactoring.guru/design-patterns/singleton

## Structural (7)

### Adapter

- **Core idea:** แปลง interface ของ dependency ให้ตรง contract ที่ระบบต้องการ
- **Consider when:** เชื่อม legacy/third-party APIs ที่รูปแบบไม่ตรงกัน
- **Cost / misuse:** adapter มากเกินไปปกปิด incompatibilities
- **BE/BFF example (original):** normalize response จาก merchant service ไป BFF contract
- **Source:** https://refactoring.guru/design-patterns/adapter

### Bridge

- **Core idea:** แยก abstraction ออกจาก implementation เพื่อเปลี่ยนแปลงอิสระ
- **Consider when:** มีสองมิติของความแปรผัน
- **Cost / misuse:** เพิ่ม abstraction โดยยังไม่มี variation จริง
- **BE/BFF example (original):** notification type x delivery channel
- **Source:** https://refactoring.guru/design-patterns/bridge

### Composite

- **Core idea:** จัดการ leaf และกลุ่ม object ผ่าน interface เดียวกัน
- **Consider when:** ข้อมูลมีโครงสร้างเป็นต้นไม้
- **Cost / misuse:** การรับประกัน type constraints ของ leaf/group ยาก
- **BE/BFF example (original):** permission/menu tree
- **Source:** https://refactoring.guru/design-patterns/composite

### Decorator

- **Core idea:** ห่อ component เดิมเพื่อเพิ่ม behavior แบบประกอบได้
- **Consider when:** logging metrics cache รอบ service
- **Cost / misuse:** ลำดับ wrapper และ debugging ซับซ้อน
- **BE/BFF example (original):** wrap repository ด้วย tracing + metrics
- **Source:** https://refactoring.guru/design-patterns/decorator

### Facade

- **Core idea:** เปิด API เรียบง่ายครอบ subsystem ที่ซับซ้อน
- **Consider when:** caller ต้องประสานหลาย service
- **Cost / misuse:** กลายเป็น god service ถ้ารวม business logic ทุกอย่าง
- **BE/BFF example (original):** BFF use-case facade เรียก admin + merchant services
- **Source:** https://refactoring.guru/design-patterns/facade

### Flyweight

- **Core idea:** แชร์ intrinsic state ที่ใช้ซ้ำเพื่อลด memory
- **Consider when:** จำนวน object มากและ shared state immutable
- **Cost / misuse:** ค่าใช้จ่ายจัดการ cache และ state segregation
- **BE/BFF example (original):** reuse immutable metadata ใน rules engine
- **Source:** https://refactoring.guru/design-patterns/flyweight

### Proxy

- **Core idea:** object ตัวแทนควบคุมการเข้าถึงอีก object
- **Consider when:** lazy load remote calls permission cache
- **Cost / misuse:** เพิ่ม network-like behavior แบบมองไม่เห็น
- **BE/BFF example (original):** client proxy ทำ auth/retry โดยต้องมี timeout policy
- **Source:** https://refactoring.guru/design-patterns/proxy

## Behavioral (10)

### Chain of Responsibility

- **Core idea:** ส่ง request ผ่านชุด handlers ตามลำดับ
- **Consider when:** validation/middleware แบบ pipeline
- **Cost / misuse:** order-dependent behavior ทำให้หา bug ยาก
- **BE/BFF example (original):** auth → permission → rate limit middleware
- **Source:** https://refactoring.guru/design-patterns/chain-of-responsibility

### Command

- **Core idea:** ห่อ action เป็น object/ข้อมูลสำหรับส่งต่อหรือจัดคิว
- **Consider when:** queue retry audit undo
- **Cost / misuse:** ต้องออกแบบ idempotency เมื่อ execute ซ้ำ
- **BE/BFF example (original):** คำสั่งเปลี่ยนสถานะ operation log
- **Source:** https://refactoring.guru/design-patterns/command

### Iterator

- **Core idea:** เข้าถึงสมาชิกชุดข้อมูลโดยไม่เปิดเผยโครงสร้างภายใน
- **Consider when:** pagination/streaming traversal
- **Cost / misuse:** hidden IO เมื่อ iterate อาจเกิด N+1
- **BE/BFF example (original):** iterator สำหรับ paginated API
- **Source:** https://refactoring.guru/design-patterns/iterator

### Mediator

- **Core idea:** รวมการประสานงานของ components แทนการอ้างถึงกันเอง
- **Consider when:** หลาย component ส่ง message ประสานกัน
- **Cost / misuse:** mediator อาจกลายเป็น god object
- **BE/BFF example (original):** workflow coordinator ระหว่าง validation, approval, publish
- **Source:** https://refactoring.guru/design-patterns/mediator

### Memento

- **Core idea:** เก็บ snapshot state เพื่อ restore โดยไม่เปิดเผย implementation
- **Consider when:** undo หรือ checkpoint
- **Cost / misuse:** snapshot มีต้นทุน storage และ sensitive data
- **BE/BFF example (original):** เก็บ version ของ draft configuration
- **Source:** https://refactoring.guru/design-patterns/memento

### Observer

- **Core idea:** publisher แจ้ง subscribers เมื่อเกิด event
- **Consider when:** event notification ภายใน process
- **Cost / misuse:** event order และ lifecycle/subscription leak
- **BE/BFF example (original):** หลัง update entity แจ้ง metrics และ audit listeners
- **Source:** https://refactoring.guru/design-patterns/observer

### State

- **Core idea:** แยกพฤติกรรมตามสถานะออกเป็น state objects
- **Consider when:** state transitions ซับซ้อน
- **Cost / misuse:** จำนวน class และ transitions สูง
- **BE/BFF example (original):** Draft → Pending → Approved → Rejected
- **Source:** https://refactoring.guru/design-patterns/state

### Strategy

- **Core idea:** สลับ algorithm ผ่าน interface เดียวกัน
- **Consider when:** business rules แตกต่างกันตาม configuration
- **Cost / misuse:** overengineering หากมีเพียงสองเงื่อนไขง่ายๆ
- **BE/BFF example (original):** คำนวณ fee/discount ตาม policy
- **Source:** https://refactoring.guru/design-patterns/strategy

### Template Method

- **Core idea:** กำหนดลำดับขั้นตอน algorithm ใน base class เปิดบางขั้นให้ override
- **Consider when:** workflow มีขั้นตอนหลักคงที่และ hook บางจุดเปลี่ยน
- **Cost / misuse:** inheritance coupling; composition อาจง่ายกว่า
- **BE/BFF example (original):** import pipeline validate→transform→persist
- **Source:** https://refactoring.guru/design-patterns/template-method

### Visitor

- **Core idea:** แยก operations ออกจากชนิด element ใน object structure
- **Consider when:** เพิ่ม operation ใหม่บน AST/tree ที่ชนิดคงที่
- **Cost / misuse:** เพิ่ม element type ใหม่ต้องแก้ visitors ทุกตัว
- **BE/BFF example (original):** ตรวจ rules และ render AST ต่างรูปแบบ
- **Source:** https://refactoring.guru/design-patterns/visitor

## Pattern comparison quick checks

| Confusion | Distinction |
|---|---|
| Factory Method / Abstract Factory / Builder | One overridable creation hook / coordinated product family / staged assembly of a complex object. |
| Adapter / Facade / Proxy | Translate incompatible interface / simplify a subsystem / preserve interface while controlling access. |
| Decorator / Proxy | Compose optional behavior / mediate controlled access; structures can resemble each other but intent differs. |
| Strategy / State | Caller/context selects interchangeable algorithms / behavior follows a domain state machine and transitions. |
| Observer / Mediator | One-to-many event notification / centralized collaboration among peers. |
| Template Method / Strategy | Reuse algorithm skeleton via inheritance / inject interchangeable behavior via composition. |
| Composite / Decorator | Tree structure / wrap one object to add behavior. |

## My Engineer application protocol

During feature planning, identify variation points; during design, document candidate patterns **and simpler alternatives**; during implementation, enforce repository conventions in AGENTS.md; during independent verification, check behavior + trade-offs, not pattern naming. Do **not** auto-apply a pattern by keyword matching. Suggested evidence: requirement → pain point → alternatives → trade-offs → chosen design → tests → measured outcome. Store pilot outcomes and failures under the pilot project's `docs/`, consistent with My Engineer project separation; the reusable pattern knowledge stays in `puen-engineer`.

## Provenance and rights

Reference catalog: https://refactoring.guru/design-patterns/catalog (accessed 2026-10-17). Original content © its respective owner; respect https://refactoring.guru/content-usage-policy . This independent condensed study guide uses original wording and newly devised engineering examples, with links rather than copied article text, illustrations, diagrams, or sample code.
