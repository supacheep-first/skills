---
description: อธิบายโค้ดที่ระบุ — purpose, flow, dependencies, edge cases
argument-hint: "[file path, function name, or code selection]"
---

อธิบาย "$ARGUMENTS" โดยครอบคลุม:

## Purpose
ทำอะไร ทำไมต้องมี

## How it works
ขั้นตอนการทำงานตาม flow (numbered list ได้)

## Dependencies
- inputs ที่คาดหวัง
- external dependencies (functions, libraries, services)
- side effects ที่เกิดขึ้น

## Edge cases & gotchas
- input ที่อาจทำให้พัง
- assumption ที่ implicit
- จุดที่ควรระวังเวลาแก้

## Quick reference
หนึ่งย่อหน้าสรุปสำหรับคนที่ skim

แนวทาง:
- ถ้า $ARGUMENTS เป็น file path → อ่านไฟล์นั้นก่อน
- ถ้าเป็นชื่อ function → ใช้ Grep หา definition
- ถ้ามีคำถามเกี่ยวกับขอบเขต — ถามก่อนตอบ
- ใช้ file:line references ทุกครั้งที่อ้างถึงโค้ด
