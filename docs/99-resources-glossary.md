# แหล่งเรียน & Glossary ไทย

## 1. แหล่งเรียนฟรี (เรียงตามลำดับที่ควรใช้)

### Technical Floor
| แหล่ง | ใช้กับ | หมายเหตุ |
|---|---|---|
| [MDN Web Docs](https://developer.mozilla.org/) | HTTP, web basics | อ่านเฉพาะส่วน HTTP |
| [Pro Git (ฟรี)](https://git-scm.com/book/th) | Git | มีฉบับภาษาไทย |
| [SQLBolt](https://sqlbolt.com/) | SQL | เรียนจบใน 3 ชม. |
| [Mode SQL Tutorial](https://mode.com/sql-tutorial/) | SQL ธุรกิจ | ตัวอย่างจริง |
| [Python Official Tutorial](https://docs.python.org/3/tutorial/) | Python | อ่านครบ 9 บท |
| [Automate the Boring Stuff](https://automatetheboringstuff.com/) | Python ใช้งานจริง | ฟรีทั้งเล่ม |
| [Exercism Python](https://exercism.org/tracks/python) | ฝึกเขียน | มี mentor ฟรี |
| [Docker Getting Started](https://docs.docker.com/get-started/) | Docker | – |

### AI Engineering
| แหล่ง | ใช้กับ |
|---|---|
| เอกสารผู้ให้บริการ LLM (OpenAI/Anthropic/Google) | prompt, tool use, structured output |
| [Prompt Engineering Guide](https://www.promptingguide.ai/) | เทคนิค prompt |
| เอกสาร pgvector / Qdrant / Chroma | vector DB |
| บล็อก/เปเปอร์เรื่อง RAG evaluation | วัดผล RAG |
| เอกสาร eval framework (Ragas, promptfoo, DeepEval) | eval harness |
| [FastAPI Docs](https://fastapi.tiangolo.com/) | API |

### Enterprise / Security
| แหล่ง | ใช้กับ |
|---|---|
| [OWASP Top 10 for LLM Applications](https://genai.owasp.org/) | ความเสี่ยง LLM |
| [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework) | governance |
| PDPA (สคส.) — เอกสารทางการ | กฎหมายไทย |
| ISO 27001 overview | ภาษาที่ IT ใช้ |
| เอกสาร SOC 2 | ตอนลูกค้าขอ |

### Craft
| แหล่ง | ใช้กับ |
|---|---|
| *The Mom Test* (Rob Fitzpatrick) | discovery interviews |
| *Continuous Discovery Habits* (Teresa Torres) | discovery เป็นระบบ |
| *Inspired* (Marty Cagan) | product thinking |
| *Made to Stick* | การสื่อสาร |
| *The Trusted Advisor* | ความสัมพันธ์กับลูกค้า |

---

## 2. แหล่งงาน & ชุมชน

- หน้าอาชีพของบริษัท AI (OpenAI, Anthropic, Databricks, Google Cloud, Palantir, Scale, Cohere)
- ตำแหน่งชื่ออื่นที่เทียบเท่า: **Applied AI Engineer, Deployment Strategist, AI Solutions Engineer, Forward Deployed AI Engineer, Solutions Architect (AI)**
- ชุมชน: AI engineering meetups, Discord/Slack ของเครื่องมือที่คุณใช้ (FastAPI, LangChain, LlamaIndex)
- แนะนำ: เขียน blog 1 ชิ้น/เดือน → เป็นแม่เหล็กที่ดีที่สุด

---

## 3. Glossary (ไทย–อังกฤษ)

### บทบาท & องค์กร
| คำ | ความหมาย |
|---|---|
| **FDE (Forward Deployed Engineer)** | วิศวกรที่ฝังตัวในทีมลูกค้า สร้างระบบ production และรับผิดชอบผลลัพธ์ |
| **Forward Deployed Strategist** | บทบาทคู่ขนานที่เน้นธุรกิจ/กระบวนการมากกว่าการเขียนโค้ด |
| **Embedded** | ฝังตัวอยู่ในทีมลูกค้า (ประชุม, Slack, codebase) |
| **Discovery** | กระบวนการทำความเข้าใจปัญหาก่อนสร้าง |
| **Adoption** | การที่ผู้ใช้เปลี่ยนมาใช้จริง |
| **Handover** | การส่งมอบให้ทีมลูกค้าดูแลต่อ |
| **Vertical slice** | ฟีเจอร์ 1 อย่างที่ครบวงจรตั้งแต่ต้นจนจบ |

### Technical
| คำ | ความหมาย |
|---|---|
| **API** | ช่องทางให้โปรแกรมคุยกัน |
| **Endpoint** | URL หนึ่งจุดของ API |
| **Status code** | รหัสบอกผลของ HTTP (200, 404, 429...) |
| **Rate limit** | จำนวนครั้งที่เรียกได้ต่อช่วงเวลา |
| **Idempotent** | เรียกซ้ำกี่ครั้งผลเท่าเดิม |
| **Latency** | เวลาตอบสนอง |
| **Throughput** | ปริมาณงานต่อหน่วยเวลา |
| **Fallback** | ทางเลือกสำรองเมื่อของหลักล่ม |
| **CI/CD** | ระบบทดสอบและ deploy อัตโนมัติ |
| **Rollback** | ย้อนกลับเวอร์ชันก่อน |
| **Runbook** | คู่มือแก้ปัญหาเมื่อระบบมีอาการ |
| **RBAC / ABAC** | การให้สิทธิ์ตามบทบาท / ตามคุณสมบัติ |
| **SSO / OIDC** | เข้าสู่ระบบครั้งเดียว / มาตรฐานยืนยันตัวตน |
| **PII** | ข้อมูลส่วนบุคคล |
| **DPA** | สัญญาการประมวลผลข้อมูล |
| **DPIA** | การประเมินผลกระทบด้านข้อมูลส่วนบุคคล |
| **Data residency** | ข้อกำหนดว่าข้อมูลต้องอยู่ประเทศ/ภูมิภาคใด |
| **Air-gapped** | ระบบที่แยกจากอินเทอร์เน็ตโดยสิ้นเชิง |
| **RTO / RPO** | เวลากู้คืนที่ยอมรับได้ / ข้อมูลที่ยอมเสียได้ |

### AI
| คำ | ความหมาย |
|---|---|
| **Token** | หน่วยย่อยของข้อความที่โมเดลประมวลผล |
| **Context window** | จำนวน token ที่โมเดลเห็นได้พร้อมกัน |
| **Temperature** | ระดับความสุ่มของคำตอบ |
| **Prompt** | คำสั่ง/ข้อมูลที่ส่งให้โมเดล |
| **System prompt** | คำสั่งตั้งต้นที่กำหนดบทบาทและนโยบาย |
| **Few-shot** | ให้ตัวอย่าง 2–3 ตัวอย่างใน prompt |
| **Structured output** | บังคับผลลัพธ์เป็น JSON/schema |
| **Tool use / Function calling** | ให้โมเดลเรียกฟังก์ชันได้ |
| **Agent** | ระบบที่วางแผนและใช้ tool หลายรอบเอง |
| **Guardrail** | ข้อจำกัดเพื่อความปลอดภัย (ขั้นตอน, เวลา, งบ, สิทธิ์) |
| **Hallucination** | การตอบที่ดูมั่นใจแต่ผิด/ไม่มีที่มา |
| **Grounding** | ยึดคำตอบกับแหล่งข้อมูลจริง |
| **RAG** | ค้นข้อมูลแล้วให้โมเดลตอบจากข้อมูลนั้น |
| **Chunk** | ชิ้นส่วนเอกสารที่ใช้ค้น |
| **Embedding** | เวกเตอร์แทนความหมายของข้อความ |
| **Vector DB** | ฐานข้อมูลที่ค้นด้วยความหมาย |
| **Hybrid search** | รวมค้นด้วยความหมาย + คำสำคัญ |
| **Reranker** | ตัวจัดอันดับผลค้นใหม่ให้แม่นขึ้น |
| **Recall@k** | สัดส่วนที่ค้นเจอของที่ถูกใน top-k |
| **Eval** | การวัดคุณภาพอย่างเป็นระบบ |
| **Golden set** | ชุดคำถาม/คำตอบอ้างอิงที่คนยืนยันแล้ว |
| **LLM-as-judge** | ให้ LLM ให้คะแนนคำตอบตาม rubric |
| **Regression** | คุณภาพถอยลงหลังเปลี่ยนอะไรบางอย่าง |
| **Observability** | ความสามารถในการมองเห็นพฤติกรรมระบบ |
| **Prompt injection** | การลักลอบใส่คำสั่งเพื่อให้โมเดลทำสิ่งที่ไม่ควร |
| **Indirect injection** | การฝังคำสั่งในเอกสาร/เว็บที่ระบบไปอ่าน |
| **Abstention** | การตอบ "ไม่พบข้อมูล" อย่างถูกต้อง |
| **Fine-tuning** | ฝึกโมเดลเพิ่มด้วยข้อมูลเฉพาะ |
| **Distillation** | ใช้โมเดลใหญ่สอนโมเดลเล็ก |
| **Agentic workflow** | กระบวนการที่ AI ตัดสินใจหลายขั้นตอน |

### Commercial
| คำ | ความหมาย |
|---|---|
| **ROI** | ผลตอบแทนเทียบกับต้นทุน |
| **Payback period** | เวลาที่ผลตอบแทนคืนทุน |
| **TCO** | ต้นทุนรวมตลอดอายุการใช้งาน |
| **Baseline** | ค่าตัวเลขก่อนเริ่มโปรเจกต์ |
| **FTE** | เทียบเท่าพนักงานเต็มเวลา |
| **Champion** | คนในองค์กรลูกค้าที่ผลักดันโปรเจกต์ |
| **Executive sponsor** | ผู้บริหารที่สนับสนุนและอนุมัติ |
| **Change management** | การบริหารการเปลี่ยนแปลงพฤติกรรมคน |
| **Scope creep** | ขอบเขตงานบานปลาย |
| **Pilot** | การทดลองใช้กับกลุ่มเล็ก |

---

## 4. เทมเพลตที่ควรมีใน toolbox ของคุณ

สร้างให้เป็น "asset" ที่ใช้ซ้ำได้ทุกโปรเจกต์:
- Opportunity assessment (1 หน้า)
- Solution design doc (3–6 หน้า)
- Eval plan + golden set schema
- Security review pack (14 ไฟล์)
- Delivery plan + milestone
- Adoption dashboard spec
- ROI model (สคริปต์)
- Handover pack (10 ไฟล์)
- Executive pitch deck (10 สไลด์)

> 🏆 **นี่คือสิ่งที่ทำให้คุณเป็น senior:** ไม่ใช่รู้ทุกอย่าง แต่มี **ของที่ใช้ซ้ำได้** และปรับให้เข้ากับลูกค้าแต่ละรายได้เร็ว
