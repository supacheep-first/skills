---
description: สรุปไฟล์/เอกสาร/หัวข้อเป็นย่อหน้าเดียว
argument-hint: "[file path, URL, or topic]"
---

สรุป "$ARGUMENTS" เป็น TL;DR:

- ความยาว: 2-4 ประโยค
- รวมเฉพาะข้อมูลที่ถ้าไม่รู้แล้วจะ miss key insight
- ตัด background, history, ของแถมทั้งหมด
- ถ้าเป็นเอกสารยาว ระบุที่มาบรรทัดสุดท้าย ("Source: <path>")

แนวทาง:
- ถ้า $ARGUMENTS เป็น file path → อ่านไฟล์
- ถ้าเป็น URL → web fetch
- ถ้าเป็นหัวข้อทั่วไป → web search ถ้าเป็น current/recent
- ภาษาตาม input
