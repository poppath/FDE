# Module 14 — Positioning, Portfolio & Interview

⏱️ **20 ชั่วโมง (สัปดาห์ 49–52)** · 🎯 ผลลัพธ์: portfolio + CV + ผ่าน interview loop

---

## 14.1 Positioning: วิธีเล่าเรื่องตัวเองในฐานะคน non-dev

### สูตร positioning
```
"[พื้นหลังเดิม] + [ความสามารถ AI engineering] + [vertical] → FDE ที่แก้ปัญหา [X] ได้จริง"
```

**ตัวอย่าง:**
- "อดีต BA สายสินเชื่อ 4 ปี ที่สร้างระบบ RAG สำหรับงาน compliance → FDE ที่เข้าใจทั้งธุรกิจและเทคโนโลยี"
- "อดีต analyst ด้าน healthcare ที่ deploy ผู้ช่วยสรุปเวชระเบียน → FDE ที่ทำงานกับโรงพยาบาลได้ทันที"
- "อดีต consultant ที่วัดผลโปรเจกต์ AI 8 โปรเจกต์ → FDE ที่บอกได้ว่าอะไรคุ้มและอะไรไม่คุ้ม"

### สิ่งที่ห้ามพูด
| ❌ อย่าพูด | ✅ พูดแบบนี้ |
|---|---|
| "ผมเพิ่งเริ่มเขียนโค้ด" | "ผมมาจากสายธุรกิจ และสร้างระบบที่ใช้งานจริงได้" |
| "ผมไม่ใช่ software engineer" | "จุดแข็งผมคือเข้าใจปัญหาธุรกิจและลงมือสร้างได้เอง" |
| "ผมเรียนจาก YouTube" | "ผมสร้างระบบ X ที่มีผู้ใช้ Y คนและวัดผลได้ Z" |
| "AI ทำแทนผมได้" | "ผมใช้ AI เร่งงาน แต่ผมรับผิดชอบการตัดสินใจและคุณภาพ" |

> 🔑 **ความจริง:** FDE ที่บริษัท AI ต้องการคือคนที่ "คุยกับลูกค้าได้ + สร้างของได้"
> คนที่จบ CS แต่คุยกับ CFO ไม่รู้เรื่องมีเยอะ — คุณมีของที่หายากกว่า

---

## 14.2 Portfolio ที่ได้ผล (ไม่ต้องมีเว็บสวย)

### โครง 3 ชิ้น
1. **Capstone** — ระบบจริง + case study + ตัวเลข (ชิ้นหลัก)
2. **Technical depth** — 1 โปรเจกต์ที่แสดงว่าคุณเข้าใจลึก (เช่น eval harness, RAG ที่วัดผลละเอียด)
3. **Craft** — design doc, security pack, adoption report (แสดงว่าคุณทำงานครบวงจร)

### GitHub profile ที่ดี
```markdown
# สวัสดี ผมชื่อ X — FDE

ผมเป็นอดีต [พื้นหลัง] ที่สร้างระบบ AI ที่ใช้งานจริงใน [vertical]

## โปรเจกต์เด่น
### [ชื่อระบบ] — ลดเวลาตอบคำถาม HR จาก 6 ชม. เป็น 3 นาที
- ผู้ใช้จริง 74% ของ 1,200 คน, citation accuracy 96%
- RAG + hybrid search + reranker, eval 62 เคส, CI gate
- [case study] [repo]

## Stack
Python · FastAPI · Postgres/pgvector · Docker · GitHub Actions · eval harness
```

### README ของแต่ละโปรเจกต์ ต้องตอบ 5 ข้อใน 20 วินาที
1. ปัญหาคืออะไร (1 ประโยค + ตัวเลข)
2. ผลลัพธ์คืออะไร (1 ประโยค + ตัวเลข)
3. ใช้เทคโนโลยีอะไร
4. รันยังไง (คำสั่งเดียว)
5. eval ทำอย่างไร / ผลเป็นเท่าไร

---

## 14.3 CV สำหรับ FDE

```
[ชื่อ] — Forward Deployed Engineer
[อีเมล | GitHub | LinkedIn | portfolio]

สรุป
อดีต [พื้นหลัง] X ปี สร้างระบบ AI ที่ใช้งานจริง [vertical]
เชี่ยวชาญ: RAG, eval design, enterprise deployment, adoption

ประสบการณ์
[บริษัท/โปรเจกต์] — สร้าง [ระบบ]
 • [ผลลัพธ์เป็นตัวเลข] ลดเวลา X → Y สำหรับ Z คน
 • [เทคโนโลยีที่ใช้ + การตัดสินใจสำคัญ]
 • [สิ่งที่วัดผล + ตัวเลขคุณภาพ]
 • [security/adoption/commercial ที่ทำ]

ทักษะ
Technical: Python, SQL, FastAPI, Docker, GitHub Actions, Postgres/pgvector
AI: RAG, hybrid search, reranking, tool use, guardrails, LLM-as-judge eval
Craft: discovery, solution design, security review, ROI modeling, change management

โปรเจกต์
[Capstone] — [ผลลัพธ์ตัวเลข] — [repo/case study]
```

> 💡 **เปลี่ยน "รับผิดชอบ..." เป็น "ลด/เพิ่ม ... จาก A เป็น B"** — recruiter มองหาตัวเลข

---

## 14.4 Interview Loop ของ FDE (และวิธีเตรียม)

| รอบ | สิ่งที่ทดสอบ | วิธีเตรียม |
|---|---|---|
| **1. Recruiter** | ทำไม FDE, ทำไมบริษัทนี้ | เขียน "ทำไม" 3 ระดับ (เทคนิค/ธุรกิจ/ส่วนตัว) |
| **2. Coding** | เขียนโค้ดได้จริงไหม | ฝึก 30–40 ข้อ (array/string/dict), Python เท่านั้น |
| **3. AI/LLM design** | ออกแบบระบบ AI ได้ไหม | ฝึก out-loud 5 เคส: RAG, agent, eval, cost, guardrail |
| **4. Customer scenario** | คุยกับลูกค้าได้ไหม | mock 3 ครั้ง (บทบาท: ลูกค้าไม่พอใจ/ไม่รู้ requirement/IT ขวาง) |
| **5. System/deployment** | เอา production ได้ไหม | อธิบาย deploy, monitoring, rollback, security |
| **6. Behavioral** | ทำงานกับความคลุมเครือได้ไหม | STAR 6 เรื่อง (ล้มเหลว/ขัดแย้ง/กดดัน/นำ/เรียนรู้/ตัดสินใจ) |
| **7. Case / take-home** | คิดครบวงจรไหม | ทำใน 4 ชม. ไม่เกิน, เน้นการตัดสินใจ + trade-off |

---

## 14.5 คำถามที่เจอบ่อย (พร้อมแนวคำตอบ)

### Coding (ระดับที่ต้องผ่าน)
1. นับความถี่ของคำในข้อความ → dict
2. รวมช่วงเวลา (merge intervals) → sort + iterate
3. หา top-k → heap หรือ sort
4. group by → defaultdict
5. parse log line → regex + dict
6. implement retry with backoff → loop + sleep
7. ออกแบบ pagination loop → while + has_more
8. dedupe ตาม key → dict/set
> 💡 FDE ส่วนใหญ่ไม่เจอ LeetCode hard — เน้นเขียนโค้ดที่อ่านรู้เรื่องและทดสอบได้

### AI/LLM design (คำถามที่แยกคนเก่ง)
1. "ออกแบบระบบตอบคำถามบนเอกสาร 10 ล้านหน้า" → ถามก่อน: สิทธิ์, ความสด, ภาษา, latency, งบ
2. "RAG ตอบผิด จะ debug อย่างไร" → แยก retrieval vs generation, ดู chunk, วัด recall
3. "จะรู้ได้อย่างไรว่า prompt ใหม่ดีกว่าเดิม" → eval set + judge + validate + gate
4. "จะคุมต้นทุนอย่างไร" → caching, model routing, ลด context, budget
5. "ทำไมไม่ fine-tune" → ข้อมูล/ต้นทุน/ความสด/การวัดผล (ต้องมีคำตอบ)
6. "จะป้องกัน prompt injection อย่างไร" → 4 ชั้น + blast radius
7. "agent ควรมี autonomy แค่ไหน" → ตามระดับความเสี่ยง + human-in-the-loop

### Customer scenario (บทบาทสมมติ)
1. "ลูกค้าบอกว่า AI ตอบผิดต่อหน้าผู้บริหาร คุณทำอะไร" → ยอมรับ, ตรวจ trace, แสดงตัวเลข, แผนแก้, ติดตาม
2. "IT บอกว่าใช้ cloud ไม่ได้" → ถาม constraint จริง, เสนอ 3 ทางเลือกพร้อมต้นทุน
3. "ผู้ใช้ไม่ยอมใช้ระบบ" → สัมภาษณ์, หา friction, champion, และดูว่าคุณค่าจริงไหม
4. "ผู้บริหารอยากได้ AI เพราะอ่านข่าว" → discovery, เสนอทางเลือกที่เล็กและวัดได้
5. "ลูกค้าขอ feature ที่จะทำให้ระบบพัง" → อธิบาย trade-off + เสนอทางเลือก + บันทึกความเสี่ยง

### Behavioral (STAR)
เตรียม 6 เรื่อง:
1. โปรเจกต์ที่ล้มเหลว และคุณเรียนอะไร
2. ครั้งที่คุณเปลี่ยนใจเพราะข้อมูล
3. ความขัดแย้งกับ stakeholder
4. ครั้งที่คุณส่งของ under pressure
5. ครั้งที่คุณทำให้คนเปลี่ยนวิธีทำงาน
6. ครั้งที่คุณบอก "ไม่" อย่างมีเหตุผล

> 💡 **สำหรับคน non-dev:** เรื่องจากประสบการณ์เดิมของคุณ (BA/consultant/analyst) ใช้ได้ 100%
> — FDE interview ให้ค่ากับ "การตัดสินใจที่ดี" มากกว่า "เทคโนโลยีที่ล้ำ"

---

## 14.6 เตรียมตัว 4 สัปดาห์สุดท้าย

| สัปดาห์ | งาน |
|---|---|
| 49 | ทำ portfolio + CV + GitHub profile ให้เสร็จ |
| 50 | ฝึก coding 20 ข้อ + AI design 5 เคส (อัดเสียง) |
| 51 | Mock interview 3 ครั้ง (coding / design / customer) + แก้จุดอ่อน |
| 52 | สมัคร 15–20 ที่ + เขียน template email + เตรียมคำถามถามบริษัท |

**คำถามที่ควรถามบริษัท (ทำให้คุณดูเป็น FDE จริง):**
- FDE ที่นี่ทำงานกับลูกค้ากี่รายพร้อมกัน
- อัตราส่วนเวลา on-site vs remote
- วัดความสำเร็จของ FDE อย่างไร
- ทีม product รับ feedback จาก field อย่างไร
- FDE มีส่วนใน roadmap แค่ไหน

---

## 14.7 Lab (สัปดาห์ 49–52)

### Lab 14.1 (6 ชม.) — Portfolio + GitHub profile
### Lab 14.2 (4 ชม.) — CV 1 หน้า + LinkedIn
### Lab 14.3 (4 ชม.) — คำตอบ "ทำไม FDE" 3 ระดับ (เขียน + ซ้อม)
### Lab 14.4 (6 ชม.) — Mock interview 3 รอบ (บันทึก + retrospective)
### Lab 14.5 (— ) — สมัครงาน 15–20 ตำแหน่ง + บันทึกผลในตาราง

---

## ✅ เกณฑ์ผ่าน

- [ ] portfolio มี 3 ชิ้นตาม 14.2
- [ ] README ของ capstone ตอบ 5 ข้อได้ใน 20 วินาที
- [ ] CV แสดงผลลัพธ์เป็นตัวเลขทุกบรรทัด
- [ ] ตอบ "ทำไม FDE" ได้ 3 ระดับ
- [ ] ผ่าน mock interview 3 รอบ
- [ ] สมัคร ≥15 ตำแหน่ง
- [ ] มีคำถาม 5 ข้อที่จะถามบริษัท

---

## 🎓 จบคอร์ส — แล้วอะไรต่อ

1. **เก็บ specialty ที่ 2** — เพิ่ม vertical หรือเทคโนโลยี (เช่น agentic workflow + healthcare)
2. **สร้าง reusable assets** — eval harness, security pack, design doc template ของตัวเอง
3. **สอนคนอื่น** — เขียน blog/พูด meetup → นี่คือสิ่งที่ทำให้คุณเป็น expert จริง
4. **กลับไปทำ discovery ใหม่** — ทุก 6 เดือน ทบทวนว่า vertical ของคุณเปลี่ยนไปแค่ไหน
5. **ติดตามงานวิจัย/เครื่องมือ** — แต่อย่าวิ่งตามทุกอย่าง ให้ยึดหลัก: วัดผลได้, ปลอดภัย, คนใช้จริง

> 🏆 **คำจำกัดความของ FDE ระดับ expert ในคอร์สนี้:**
> "คนที่สามารถพาลูกค้าจาก 'ปัญหาที่คลุมเครือ' ไปถึง 'ระบบที่คนใช้จริงและวัดผลได้'
> โดยไม่ต้องมีใครมาคุมมือ — และทำซ้ำได้กับลูกค้ารายต่อไป"
