# skills

Claude Code skills ของฉัน เก็บรวมไว้ที่นี่ ตัวจริงอยู่ใน repo นี้ แล้ว symlink ไปให้ Claude Code อ่านที่ `~/.claude/skills/<name>`

## เขียนเอง

| skill | ทำอะไร |
|---|---|
| [`ponytail`](ponytail/) | บีบให้เขียนโค้ดสั้นที่สุดที่ยังทำงานได้ — YAGNI, reuse ก่อนเขียนใหม่, stdlib ก่อน dependency |
| [`pordee`](pordee/) | โหมดสื่อสารไทย+อังกฤษแบบบีบคำ ลด token ~60-75% |
| [`pordee-adhd`](pordee-adhd/) | pordee + จัดโครงคำตอบให้ลงมือทำง่าย (action-first, numbered steps) |
| [`trim-context`](trim-context/) | ตัดไฟล์ CLAUDE.md/AGENTS.md/runbook ที่บวมให้เหลือแต่ส่วนที่ session ใหม่ต้องรู้ |

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
for name in ponytail pordee pordee-adhd trim-context diagram-design last30days wait-what; do
  ln -s ~/.local/share/skills/$name ~/.claude/skills/$name
done
```

แก้ skill ที่ `~/.local/share/skills/<name>` แล้ว commit/push ตามปกติ — `~/.claude/skills/<name>` เป็น symlink ตามไปเห็นทันที
