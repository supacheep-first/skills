# skills

Claude Code skills ของฉัน เก็บรวมไว้ที่นี่ ตัวจริงอยู่ใน repo นี้ แล้ว symlink ไปให้ Claude Code อ่านที่ `~/.claude/skills/<name>`

## เขียนเอง

| skill | ทำอะไร |
|---|---|
| [`ponytail`](ponytail/) | บีบให้เขียนโค้ดสั้นที่สุดที่ยังทำงานได้ — YAGNI, reuse ก่อนเขียนใหม่, stdlib ก่อน dependency |
| [`pordee`](pordee/) | โหมดสื่อสารไทย+อังกฤษแบบบีบคำ ลด token ~60-75% |
| [`pordee-adhd`](pordee-adhd/) | pordee + จัดโครงคำตอบให้ลงมือทำง่าย (action-first, numbered steps) |
| [`trim-context`](trim-context/) | ตัดไฟล์ CLAUDE.md/AGENTS.md/runbook ที่บวมให้เหลือแต่ส่วนที่ session ใหม่ต้องรู้ |

## งานพัฒนา (ยกมาจากโปรเจกต์ client เก่า — ตัด reference client ออกแล้ว)

เขียนไว้ตอนทำงานโปรเจกต์ client หนึ่ง แต่เนื้อเป็น workflow ทั่วไป ไม่ผูกโดเมนเฉพาะ — ตัดชื่อ repo/org/ticket-tracker/hostname ที่เจาะจงออกแล้วก่อนยกมาไว้ที่นี่ (`implement`, `fix-bug`, `open-pr`, `commit-message` มีจุดที่แก้เนื้อเพื่อ genericize ตัวอื่นในกลุ่มนี้ไม่มีร่องรอย client หลุดมา — เช็กด้วย grep แล้ว)

| skill | ทำอะไร |
|---|---|
| [`implement`](implement/) | ทำงานจาก spec/ticket set — ticket loop, 3 gates (build/review/e2e), one ticket per session |
| [`tdd`](tdd/) | Test-driven development, red-green-refactor |
| [`fix-bug`](fix-bug/) | loop แก้บั๊ก: confirm goal → repro (red) → root cause → plan → delegate → review loop → close |
| [`diagnosing-bugs`](diagnosing-bugs/) | loop ไล่บั๊กยาก/performance regression ที่ root cause ยังไม่ชัด |
| [`code-review`](code-review/) | รีวิว diff คู่ 2 แกน (coding standard + ตรงกับ spec ไหม) รันเป็น sub-agent คู่กัน |
| [`resolving-merge-conflicts`](resolving-merge-conflicts/) | ไล่ merge/rebase conflict ที่ค้างอยู่ |
| [`open-pr`](open-pr/) | เปิด/แก้ MR หรือ PR ผ่าน `glab`/`gh` แบบ draft-first ต้อง confirm ก่อนสร้างจริง |
| [`commit-message`](commit-message/) | จัดฟอร์แมต commit message — ตัวอย่าง policy (ภาษา, ความยาว, trailer), ปรับตาม repo จริง |
| [`to-tickets`](to-tickets/) | ตัดแผน/spec/conversation เป็น tracer-bullet ticket พร้อม blocking edge |
| [`to-spec`](to-spec/) | สังเคราะห์ conversation ที่คุยกันแล้วเป็น spec ส่งเข้า issue tracker (ไม่สัมภาษณ์ใหม่) |
| [`wayfinder`](wayfinder/) | วางแผนงานก้อนใหญ่เกินหนึ่ง session เป็น decision ticket บน issue tracker |
| [`domain-modeling`](domain-modeling/) | สร้าง/คมคำศัพท์ domain model, เขียน/แก้ CONTEXT.md หรือ ADR |
| [`codebase-design`](codebase-design/) | ศัพท์กลางออกแบบ deep module — หา seam, เพิ่ม testability |
| [`improve-codebase-architecture`](improve-codebase-architecture/) | สแกนหา deepening opportunity ทำเป็น HTML report แล้ว grill ทีละจุดที่เลือก |
| [`prototype`](prototype/) | prototype ทิ้งได้ เพื่อเช็ก state model หรือหน้า UI ก่อนลงจริง |
| [`grill-me`](grill-me/) | สัมภาษณ์เค้นแผน/design ให้แน่นก่อนลงมือ |
| [`grilling`](grilling/) | เค้นคำถามผู้ใช้เรื่องแผน/decision/ไอเดีย ให้ผู้ใช้คิดเอง |
| [`grill-with-docs`](grill-with-docs/) | เหมือน grill-me แต่สร้าง ADR/glossary ไปด้วยระหว่างคุย |
| [`research`](research/) | สืบเรื่องจาก primary source ที่เชื่อได้ แล้วบันทึกผลเป็น .md ในโค้ด |
| [`handoff`](handoff/) | บีบ conversation ปัจจุบันเป็น handoff doc ให้ agent/session อื่นรับต่อ |
| [`writing-for-agents`](writing-for-agents/) | เขียน/แก้ skill ใหม่, หรือแก้ AGENTS.md/CLAUDE.md |

## third-party (vendor เข้ามาใช้)

ก็อปมาจาก repo อื่น ไม่ใช่ของเขียนเอง เก็บไว้ที่นี่เพื่อ sync ไปทุกเครื่องที่ใช้ — **เช็ก license ของ upstream ก่อนถ้าจะเปิด repo นี้เป็น public**

| skill | ทำอะไร | ที่มา |
|---|---|---|
| [`diagram-design`](diagram-design/) | สร้าง diagram/chart หลายแบบเป็น HTML/SVG/PNG | [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) |
| [`last30days`](last30days/) | รวบรวมว่าคนพูดถึงเรื่องไหนใน 30 วันล่าสุด (Reddit, X, YouTube, HN, ...) | ติดตั้งผ่าน plugin/marketplace ของ Claude skills |
| [`wait-what`](wait-what/) | เบรกให้อธิบายใหม่แบบง่ายเมื่อคำอธิบายก่อนหน้าไม่เคลียร์ | [mattpocock/skills](https://github.com/mattpocock/skills) (plugin `mattpocock-skills`) |

ไม่รวม: `ego-browser` (มาคู่กับแอป ego lite เอง ไม่ใช่ package แยก) และ `synced` (เป็น cache/bucket dir ของเครื่องมือ sync ไม่ใช่ skill)

## sync เข้าเครื่องใหม่

```bash
git clone git@github.com:supacheep-first/skills.git ~/.local/share/skills
for name in ponytail pordee pordee-adhd trim-context \
            implement tdd fix-bug diagnosing-bugs code-review resolving-merge-conflicts open-pr commit-message \
            to-tickets to-spec wayfinder domain-modeling codebase-design improve-codebase-architecture prototype \
            grill-me grilling grill-with-docs research handoff writing-for-agents \
            diagram-design last30days wait-what; do
  ln -s ~/.local/share/skills/$name ~/.claude/skills/$name
done
```

แก้ skill ที่ `~/.local/share/skills/<name>` แล้ว commit/push ตามปกติ — `~/.claude/skills/<name>` เป็น symlink ตามไปเห็นทันที
