---
description: สร้าง diagram อธิบาย concept, flow, หรือ system ที่ระบุ
argument-hint: "[concept, flow, or system]"
---

สร้าง diagram สำหรับ "$ARGUMENTS":

เลือก format ที่เหมาะที่สุดโดยไม่ถามกลับ:
- Mermaid `flowchart` — process flow, decision tree
- Mermaid `sequenceDiagram` — interaction between components/actors
- Mermaid `erDiagram` — data model, entity relationships
- Mermaid `stateDiagram-v2` — state machine
- ASCII block diagram — system architecture, network topology
- Markdown table — comparison matrix, mapping

หลัง diagram ให้คำอธิบายสั้น 2-3 ประโยคว่า diagram อ่านยังไง และจุดสำคัญคืออะไร

ถ้า input ระบุ format → ใช้ตามนั้น
ถ้าไม่ระบุ → เลือกเองตาม content
ภาษา label ใน diagram: English (เพื่อ compatibility) แต่คำอธิบายตาม input
