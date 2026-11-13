# Module 00 — Roadmap ละเอียด & เกณฑ์ผ่านแต่ละ Phase

## 1. ตารางเรียน 12 เดือน (ฉบับทำงานไปด้วย 12 ชม./สัปดาห์)

| สัปดาห์ | Phase | เนื้อหา | Artifact ที่ต้องส่ง |
|---|---|---|---|
| 0 | 0 | Orientation, self-assessment, เลือก vertical | `my-fde-profile.md` |
| 1–2 | 1 | Module 1: software, HTTP, API, Git | repo + REST client เล็ก ๆ |
| 3–4 | 1 | Module 2: data literacy + SQL | ชุด query 20 ข้อบน dataset จริง |
| 5–8 | 1 | Module 3: Python | CLI tool ที่อ่านข้อมูลจริงและมี test |
| 9–11 | 2 | Module 4: LLM engineering | LLM-powered feature + prompt versioning |
| 12–15 | 2 | Module 5: RAG | RAG service บนเอกสารจริงขององค์กรหนึ่ง |
| 16–18 | 2 | Module 6: agents / tool use | agent ที่ทำงาน 3 tools + guardrails |
| 19–20 | 2 | Module 7: eval & observability | eval suite + dashboard |
| 21–23 | 3 | Module 8: deploy | service ที่มี CI/CD + logging + rollback |
| 24–28 | 3 | Module 9: enterprise & security | security review pack + threat model |
| 29–31 | 4 | Module 10: discovery | opportunity assessment + design doc |
| 32–35 | 4 | Module 11: delivery & adoption | delivery plan + adoption metrics |
| 36–38 | 4 | Module 12: commercial | business case + ROI model |
| 39–40 | 4 | ทบทวน + ยิง mock discovery 3 ครั้ง | บันทึก mock sessions + retrospective |
| 41–48 | 5 | Module 13: capstone | ระบบ live + case study |
| 49–52 | 5 | Module 14: positioning | portfolio site + CV + mock interviews |

---

## 2. Phase Gate — จะไป Phase ถัดไปได้ต้องผ่านอะไร

### Gate 0 → 1
- [ ] เขียนได้ว่าตัวเองเก่ง domain ไหน และจะใช้มันเป็นจุดต่างอย่างไร
- [ ] ระบุ target role ได้ (บริษัทแบบไหน, ระดับไหน)

### Gate 1 → 2 — *Technical Floor*
- [ ] อธิบายได้ว่าเมื่อเปิดเว็บหนึ่งหน้า เกิดอะไรขึ้นบ้างตั้งแต่ DNS ถึง render
- [ ] เรียก REST API ด้วย `curl`/Postman ได้เอง รวมถึงอ่าน response code และ error
- [ ] ใช้ Git ได้: branch, commit, PR, แก้ merge conflict ง่าย ๆ
- [ ] เขียน SQL ได้: JOIN 3 ตาราง, GROUP BY, window function เบื้องต้น
- [ ] เขียน Python ~300 บรรทัดได้เอง มี function, error handling, และ test
- [ ] **เกณฑ์จริง:** สร้าง CLI tool ที่อ่านไฟล์ CSV/API จริง แล้วสร้างรายงานได้โดยไม่ต้องก๊อปโค้ดคนอื่นมาทั้งดุ้น

### Gate 2 → 3 — *AI Engineering*
- [ ] ออกแบบระบบ LLM ได้: เลือก model, ออก prompt, จัดการ context window, คุมต้นทุน
- [ ] สร้าง RAG ที่มี chunking strategy ที่ให้เหตุผลได้ + วัด retrieval quality
- [ ] สร้าง agent ที่ใช้ tool 3 ตัว และมี guardrail (timeout, retry, human-in-the-loop)
- [ ] **เขียน eval suite ที่รันซ้ำได้** และอธิบายได้ว่า regression คืออะไร
- [ ] อธิบาย hallucination, prompt injection, และวิธีบรรเทาได้

### Gate 3 → 4 — *Production*
- [ ] deploy service ด้วย Docker ได้ และตั้ง CI/CD พื้นฐาน
- [ ] อ่าน log, ตั้ง alert, และ rollback ได้
- [ ] ประมาณการต้นทุน running cost ต่อเดือนได้
- [ ] ทำ threat model และตอบคำถาม IT security ขององค์กรได้

### Gate 4 → 5 — *FDE Craft*
- [ ] ทำ discovery กับลูกค้าจริง/จำลอง 90 นาที แล้วสรุปเป็น problem statement ได้
- [ ] เขียน design doc ที่ CTO อ่านแล้วอนุมัติได้
- [ ] วางแผน adoption ที่มีตัวเลขวัดผล ไม่ใช่ "จะจัด training"
- [ ] คำนวณ ROI และนำเสนอผู้บริหารใน 10 นาทีได้

### Gate 5 — *Expert*
- [ ] capstone รันจริง มีผู้ใช้จริง (อย่างน้อย 5 คน) ต่อเนื่อง 4 สัปดาห์
- [ ] มี case study ที่เล่าตัวเลขผลลัพธ์ได้
- [ ] ผ่าน mock interview ครบ loop (coding, system design, customer scenario)

---

## 3. สัดส่วนเวลารายวัน (สำหรับ 12 ชม./สัปดาห์)

| กิจกรรม | ชม./สัปดาห์ | หมายเหตุ |
|---|---|---|
| เรียน concept | 3 | วิดีโอ/อ่าน/doc — **ห้ามเกินนี้** |
| Lab / เขียนโค้ด | 6 | สัดส่วนมากที่สุด |
| อ่านโค้ดคนอื่น / code review | 1 | วิธีเร็วที่สุดในการเก่งขึ้น |
| เขียน/blog/สรุป | 1 | บังคับให้เข้าใจจริง |
| คุยกับคน (community/mock) | 1 | FDE เป็นงานคน |

---

## 4. กับดักที่คน non-dev เจอบ่อย (และวิธีแก้)

| กับดัก | อาการ | วิธีแก้ |
|---|---|---|
| **Tutorial hell** | ดูคอร์สจบ 5 คอร์ส แต่เขียนเองไม่ได้ | กฎ 1:1 — ดู 1 ชม. ต้องเขียน 1 ชม. |
| **ลอกโค้ด AI ทั้งดุ้น** | งานเสร็จเร็ว แต่ตอบคำถามใน interview ไม่ได้ | หลังทุกครั้งที่ AI ช่วย เขียน 3 bullet ว่าโค้ดทำอะไร |
| **หนีคณิตศาสตร์** | กลัวจนไม่อ่านเลย | ไม่ต้องเรียน calculus — เรียน statistics พื้นฐาน + eval |
| **ข้าม eval** | demo สวย แต่พอลูกค้าใช้จริงพัง | บังคับ eval ตั้งแต่ Module 4 |
| **ไม่กล้าแตะ terminal** | พึ่ง GUI อย่างเดียว | ตั้งกฎ: งานเดิมทำด้วย CLI ทุกครั้งที่ทำได้ |
| **เก่งเทคนิคแต่ขายไม่ได้** | สร้างของดีแต่ไม่มีใครใช้ | Phase 4 ให้น้ำหนัก 30% |
| **สร้างสิ่งที่ลูกค้าไม่ได้ขอ** | ภูมิใจใน solution แต่ไม่ตอบโจทย์ | discovery ก่อนสร้างเสมอ |

---

## 5. การวัดความคืบหน้า

ทำ **weekly review** ทุกสัปดาห์ (ดู `templates/weekly-ops-review.md`) และให้คะแนน 1–5 ใน 4 มิติ:

1. **Technical** — เขียนโค้ด/แก้ปัญหาเองได้แค่ไหน
2. **AI** — สร้างและวัดระบบ LLM ได้แค่ไหน
3. **Delivery** — ส่งของถึงมือผู้ใช้ได้แค่ไหน
4. **Communication** — อธิบายให้คนไม่เทคนิคเข้าใจได้แค่ไหน

ถ้ามิติใดต่ำกว่า 3 ติดกัน 3 สัปดาห์ → หยุดเดินหน้า แล้วทุ่มเวลาซ่อมมิตินั้น

---

## 6. หลังจบคอร์สแล้วทำอะไรต่อ

- เข้า community FDE / AI engineering และแชร์ capstone
- สมัครตำแหน่งที่ใกล้เคียงก่อนถ้ายังไม่มั่นใจ: Applied AI Engineer, Solutions Architect (AI), AI Consultant, Deployment Strategist
- เก็บ specialty ที่สอง (เช่น healthcare + agentic workflows) เพื่อเป็น T-shaped ที่ลึกจริง
- กลับมาทำ eval/เทมเพลตของตัวเองให้เป็น reusable asset — นี่คือสิ่งที่ FDE ระดับ senior ทำ
