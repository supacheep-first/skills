# Custom Claude Code Commands

User-level commands — ใช้ได้ทุก project โดยไม่ต้อง config เพิ่ม.

> หากต้องการ override ใน project ใด ให้ copy ไฟล์นั้นไปที่ `.claude/commands/` ในโปรเจกต์

---

## Commands

| Command | Description | Usage |
|---|---|---|
| `/keypoint` | สรุปประเด็นหลัก 3-5 ข้อ + Takeaway | `/keypoint CQRS pattern` |
| `/concise` | ตอบสั้นสุด ≤ 3 ประโยค ไม่มี preamble | `/concise what is idempotency` |
| `/eli5` | อธิบายเหมือนคุยกับเด็ก 5 ขวบ | `/eli5 how DNS works` |
| `/eli15` | อธิบายเหมือนคุยกับวัยรุ่น 15 ปี — ละเอียดกว่า eli5 | `/eli15 how public-key crypto works` |
| `/explain` | อธิบายโค้ด: purpose / flow / dependencies / edge cases | `/explain src/auth/middleware.ts` |
| `/tldr` | สรุป 2-4 ประโยค จาก file, URL, หรือ topic | `/tldr ./CLAUDE.md` |
| `/devil-advocate` | หา assumption ผิด, risks, verdict | `/devil-advocate migrate from REST to GraphQL` |
| `/compare` | ตารางเปรียบเทียบ + trade-offs + เลือกเมื่อไหร่ | `/compare Kafka vs NATS` |
| `/teach-me` | สอน 3 layers: foundation → intermediate → advanced | `/teach-me Go interfaces` |
| `/direct` | ตอบตรง ไม่ hedge ไม่ขอโทษ ไม่ fluff | `/direct ควรใช้ mutex หรือ channel` |
| `/diagram` | สร้าง Mermaid / ASCII / table diagram + คำอธิบาย | `/diagram JWT authentication flow` |
| `/outline` | โครงร่าง hierarchical 3 ระดับ + Next step | `/outline บทความ microservices สำหรับมือใหม่` |
| `/why` | root cause analysis 3 ชั้น: surface → deeper → root | `/why Go ไม่มี generics ตั้งแต่แรก` |
| `/how` | prerequisites / steps / verification / pitfalls | `/how set up goose migrations` |
| `/brainstorm` | 10-15 ไอเดีย 3-4 category + Top 3 worth exploring | `/brainstorm วิธีลด cold start ใน Lambda` |
| `/socratic` | นำทางด้วยคำถาม Socratic 1 ข้อต่อ turn ไม่เฉลย | `/socratic ควรแยก monolith เป็น microservices ไหม` |
| `/quiz` | ออกข้อสอบทีละข้อ ตอบแล้วเฉลยทันที จำนวนข้อตามความซับซ้อนของเรื่อง | `/quiz Go channels` |

---

## Usage examples

```
/keypoint onboarding flow ของแอป
/concise difference between authentication and authorization
/eli5 database transactions
/eli15 public-key cryptography
/explain internal/usecase/onboarding.go
/tldr https://go.dev/doc/effective_go
/devil-advocate เพิ่ม caching layer ก่อน service ทุกตัว
/compare PostgreSQL vs MongoDB
/teach-me gRPC streaming ทั้งหมด
/direct ควรใช้ mutex หรือ channel สำหรับ shared state ใน Go
/diagram request flow จาก frontend ถึง gRPC service
/outline บทความ event-driven architecture
/why distributed tracing ยาก
/how เพิ่ม service ใหม่ใน monorepo
/brainstorm วิธีลด test flakiness ใน e2e suite
/socratic ควรย้าย business logic ออกจาก GraphQL resolver ไหม
/quiz internal/usecase/onboarding.go
```

---

## Notes

- ทุก command รองรับทั้งภาษาไทยและอังกฤษ — ตอบตาม input
- `/explain`, `/tldr` และ `/quiz` จะอ่านไฟล์จริงถ้า argument เป็น path
- `/keypoint`, `/compare`, `/tldr` จะ web search ถ้าหัวข้อเป็น real-time info
- `/teach-me` หยุดหลัง Layer 1 และถามก่อน ถ้าไม่ได้บอกว่าเอาทั้งหมด
- `/diagram` เลือก Mermaid / ASCII / table เองตาม content — ไม่ถามกลับ
- `/socratic` ใช้ 1 คำถามต่อ turn — รอผู้ใช้ตอบก่อนเสมอ
- `/quiz` ถามทีละข้อ ไม่กำหนดจำนวนข้อล่วงหน้า — สั่งกลางทางได้: ยากขึ้น / ง่ายลง / ข้าม / เฉลย / พอ
- `/brainstorm` เก็บ wild ideas ไว้ใน category แยก ไม่ตัดทิ้ง
